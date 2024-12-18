# GDS

GDS の作成には gdstk, PHIDL, gdsfactory など様々なパッケージが存在する。  
それぞれの違いとしては以下のテーブルにまとめている。

|        Feature         |     gdsfactory      |     gdspy      |        gdstk        |        PHIDL        |  KLayout Scripting  |
| ---------------------- | ------------------- | -------------- | ------------------- | ------------------- | ------------------- |
| **Abstraction Level**  | High                | Medium         | Medium              | High                | High/GUI-based      |
| **Speed**              | Medium              | Medium         | High                | Medium              | Low (scripted)      |
| **Ease of Use**        | High                | Low            | Low                 | High                | Medium              |
| **Simulation Support** | Yes                 | No             | No                  | No                  | No                  |
| **Maintenance**        | Actively Maintained | Deprecated     | Actively Maintained | Actively Maintained | Actively Maintained |
| **Use Case Focus**     | Photonics           | General Layout | General Layout      | General Layout      | Interactive Design  |

それぞれのパッケージのインストールには Anaconda で conda 環境を用意すると非常に便利。

## Reference

- [KQCircuits](https://iqm-finland.github.io/KQCircuits/index.html)
- [PHIDL](https://phidl.readthedocs.io/en/latest/index.html)

## Anaconda

データサイエンスに必要な各種ツール、ライブラリを提供するプラットフォームです。
Python に限らず、データサイエンスに関わるコマンドやプログラミング言語も提供しています。
また、GUI（Anaconda Navigator） と CUI（conda） 、両方での操作が可能です。

### conda 環境

Conda環境は独立したPythonの実行環境で、他の環境に影響を与えずにPythonのバージョンを用途によって切り替えたり、パッケージをインストールしたりできます。

```
$ conda info -e
# conda environments:
#
base                  *  /opt/anaconda3
klayout                  /opt/anaconda3/envs/klayout

$ conda activate klayout

(klayout)
$ which python
/opt/anaconda3/envs/klayout/bin/python


$ conda deactivate
```

#### conda 環境の export

作成した conda 環境を yaml ファイルに export すると、簡単に同じ環境の conda 環境を作成することができる。

```txt title=conda環境のexport
// conda 環境に入ってから
$ conda env export > environment.yml
// conda 環境の外から
$ conda env export  -n [仮想環境名] > environment.yml
```

```txt title=conda環境のimport
conda env create -f environment.yml
```


#### channel について

channel とはユーザーが独自にパッケージを登録・管理するリポジトリのことを指す。  
以下のように matplotlib は ```pkgs/main``` にあるので ```conda install matplotlib``` でインストールすることができる。  

```
$ conda search --full-name matplotlib
Loading channels: done
# Name                       Version           Build  Channel             
matplotlib                     1.5.3     np110py27_0  conda-forge         
matplotlib                     1.5.3     np110py34_0  conda-forge 
...
matplotlib                     3.9.2  py39h6e9494a_2  conda-forge         
matplotlib                     3.9.2  py39hecd8cb5_0  pkgs/main 
```

しかし、パッケージによっては  conda-forge などの channel にしかない場合がある。

```
$ conda search --full-name gdsfactory
Loading channels: done
# Name                       Version           Build  Channel             
gdsfactory                    5.12.8    pyhd8ed1ab_0  conda-forge         
gdsfactory                    5.12.9    pyhd8ed1ab_0  conda-forge         
...  
gdsfactory                     7.3.2    pyh7448d05_0  conda-forge         
gdsfactory                     7.3.5    pyh7448d05_0  conda-forge  
```

その場合は、以下のように channel を追加することができる。

```
# 優先順位を高くする
$ conda config --add channels conda-forge
# 優先順位を低くする
$ conda config --append channels conda-forge
# 優先順位の確認
$ conda config --get 

$ conda config --set channel_priority strict
# channel の削除
$ conda config --remove channels
```


### 各種パッケージのインストール
#### klayout のインストール

```
# 以下 klayout が環境名
$ conda create -n klayout python=3.11 
$ conda activate klayout
$ conda install gdstk
$ conda install matplotlib 
```

#### qiskit-metal のインストール

```
$ git clone https://github.com/Qiskit/qiskit-metal.git
$ cd qiskit-metal

# 新しく conda 環境を作成
$ conda env create -n qiskit-metal -f environment.yml
$ conda activate qiskit-metal
$ python -m pip install --no-deps -e .
```

#### gdsfactory のインストール

klayout から gds の表示を行うには、[gdsfactory training](https://gdsfactory.github.io/gdsfactory-photonics-training/index.html) に書いてあるように module を追加して klive を利用できるようにしておく。

```
$ conda create -n gdsfactory python=3.11
$ conda activate gdsfactory
$ conda config --add channels conda-forge
$ conda config --set channel_priority strict
$ conda install gdsfactory
```

## PHIDL

### 他の gds file からデザインを読み込む

```python
input_gds = "../../PHIDL-based/output/Kobayashi_300Cu.gds"  # Replace with your GDS file name
library = gdstk.read_gds(input_gds)

lib = wf.get_lib()
lib.add(*library.cells) # 他の　library に cell を追加する

main_cell = library.top_level()[0]
wf.add(gdstk.Reference(main_cell))
```

### resonator の作り方

実際の path の長さは以下のように確認することができる。

```python
print(f"Length : {P.length()} [um]")
```

また path に沿って曲率がどのように変化するかは以下のように確認することができる。

```python
import matplotlib.pyplot as plt

s, K = P.curvature()
plt.plot(s, K, ".-")
plt.xlabel("Position along curve (arc length)")
plt.ylabel("Curvature")
```