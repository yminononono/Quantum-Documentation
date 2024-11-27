# Qiskit-metal

## Ansys HFSS で電磁界シミュレーション

[高周波デバイスの設計とシミュレーション](http://accwww2.kek.jp/oho/oho08/txt08/02%20yoshida2.080825p.pdf) の p.11 を参照。
Ansys HFSSには3つの解析方法があり、解析したいモデルの状況により適切に解析方法を選ぶ必要がある。　　

- Driven Modal : 伝搬モードに着目した電磁界解析。導波管などの解析に用いる。 
- Driven Terminal : ノードに着目した電磁界解析。デジタル信号の解析に用いる。 
- Eigenmode : 固有値解析。どんな電磁波が閉じ込めることができるかを調べる解析。 

### Lumped port と Wave port

[オープンリング共振器を用いた高周波帯非接触インターフェイスの研究](https://ohnolab.deca.jp/wp-content/lab_data/pdf_a/2011_M_Abe_doc.pdf)の p.45 を参照。
HFSS 上で給電部分の設定は Wave port と Lumped port の 2 つが使用できる。  
ここで、Wave portとは「自然給電」のことであり、port が設定されている面の特性インピーダンスを解析し、その結果として得られた特性インピーダンスで給電するものである。  
一方、Lumped portとは「強制給電」のことであり、portが設定されている面に対して解析者が指定した特性インピーダンスで強制的に給電するものである。通常、実験環境をシミュレーションに考慮する場合、Lumped portを仮定する。  
しかし、port面が無反射なWave portに対して、Lumped port はport面では反射が発生する。  

### シミュレーションの CPU Core の変更

```Tools``` -> ```Edit Active Analysis Configuration``` から使用するコア数を変更することができる。([image](<Screenshot 2024-11-21 at 17.12.16.png>){ width="50%" })

### 電磁場解析

```Analysis``` -> ```Setup``` -> 電磁場解析を行いたい setup の ```property``` から ```3D Fields Save Options``` -> ```Save Fields``` で frequency sweep を行う際に電磁場も保存するようにする。  

![](<Screenshot 2024-11-22 at 16.43.41.png>){ width="50%" }

保存した電磁場を表示するには、表示したいオブジェクトを選択した状態で ```Field Overlays``` を右クリックし、```Plot Fields``` -> ```E``` -> ```Mag_E``` で設定画面を表示する。  
表示した画面の ```Setup``` から Fast sweep を行った analysis を選択肢し、電磁場の周波数を選択する。

![](<Screenshot 2024-11-22 at 16.44.20.png>){ width="50%" }

### メッシュの切り方

Resonator などメッシュの細かさによって解析結果が変わるものについては、ある程度細かく切るように指定する必要がある。
ただし、全て細かく切ってしまうと非常に時間がかかってしまう。
以下の図は resonator と ground がつながっているため、周りの ground のメッシュも細かく切ってしまった場合を示している。

![alt text](<Screenshot 2024-11-25 at 18.19.26.png>){ width="50%" }

### 実際の script の書き方

以下では 1 本のフィードラインに同じ共振周波数の 2 本の resonator を capacitive に coupling させて読み出す場合の script で説明する。  
一方の読み出しをフィードラインに、もう一方を resonator の先につけている。

![](<Screenshot 2024-11-21 at 10.32.03.png>){ width="50%" }

```python

# Resonator の共振周波数はメッシュの細かさによって変動するため、以下のようにメッシュを細かくしたいオブジェクトについては指定しておく。
# for acurate simulations, make sure the mesh is fine enough for the meander
accurate_meshlist = [

    # 2 つの resonator (trace は線を示している)
    "trace_meander1",
    "trace_meander2",

    # resonator と resonator を繋ぐ capacitance pad
    "pad_right_my_cap_res_res",
    "pad_left_my_cap_res_res",
    "cpw_right_my_cap_res_res",
    "cpw_left_my_cap_res_res",   

    # resonator と launch pad を繋ぐ capacitance pad
    "pad_right_my_cap_res_lp",
    "pad_left_my_cap_res_lp",
    "cpw_right_my_cap_res_lp",
    "cpw_left_my_cap_res_lp",    
]
hfss.modeler.mesh_length(
                'cpw_mesh',
                accurate_meshlist,
                MaxLength='0.01mm')

```

Ansys で render したオブジェクトにカーソルを合わせるとその名前が表示される。

![](<Screenshot 2024-11-21 at 10.00.45.png>){ width="50%" }

また実際にどのようにメッシュが切られたか確認するには、  
1. Ansys でオブジェクトを選択 (全て選択したい場合は、```Sheets``` を右クリックして全選択する。)

![](<Screenshot 2024-11-21 at 11.11.15.png>){ width="50%" }

2. 選択した状態で ```FieldOverlays``` を右クリックして ```Plot Mesh``` を選択する。

![](<Screenshot 2024-11-21 at 11.13.46.png>){ width="50%" }


## GDS file を import してシミュレーション

2D transmon のデザインはポジ描画用のため、誘電体部分がgdsで書かれている。
そのため、gds で作成している間に反転したものを用意しておく。

1. まずは新規プロジェクトの作成 (```File``` -> ```New```)
2. HFSS design の作成 (```Project``` -> ```Insert HFSS design```)
3. HFSS design の Solution type の設定
    - S-parameter の測定などを行いたい場合には、HFSS design を右クリックして ```Solution Type``` を選択
        - ![](<Screenshot 2024-11-25 at 14.43.43.png>){ width="50%" }
    - ```HFSS with Hybrid and Arrays```, ```Network Analysis```, ```Modal``` を選択する。
        - ![](<Screenshot 2024-11-25 at 14.44.01.png>){ width="50%" }
4. GDS design の import (```Modeler``` -> ```Import```)
    - ![](<Screenshot 2024-11-25 at 17.12.47.png>){ width="50%" }
    - ![](<Screenshot 2024-11-25 at 17.13.35.png>){ width="50%" }
5. Boundary の設定
    - Ground や feedline, resonator など金属が蒸着されている部分を選択し、右クリックで ```Assign Boundary``` -> ```Perfect E``` を設定する。
    - ![](<Screenshot 2024-11-25 at 17.18.03.png>){ width="50%" }
6. シリコン基盤と vacuum の作成
    - Model の中に solid を定義する
    - ```Draw``` -> ```Box``` で box を定義する。
    - シリコン基盤の場合は、デザインの下に密着するように配置し、```Properties``` から silicon を選択する。
        - ![](<Screenshot 2024-11-25 at 17.19.36.png>){ width="50%" }
        - ![](<Screenshot 2024-11-25 at 17.19.08.png>){ width="50%" }
    - vacuum の場合は、デザイン全体を覆うように選択する。
7. Port の作成
    - Launch pad の横に四角形の model を用意する。(```Draw``` -> ```Rectangle```)
        - ![](<Screenshot 2024-11-25 at 17.26.27.png>){ width="50%" }
    - 作成した model を選択し、Lumped port として指定する。(```Assign Excitation``` -> ```Port``` -> ```Lumped Port```)
        - ![](<Screenshot 2024-11-25 at 17.31.09.png>){ width="50%" }
    - ```Integration Line``` の指定が必要になるので、図から指定する。(図の赤い矢印)
        - ![](<Screenshot 2024-11-25 at 17.31.25.png>){ width="50%" }
        - ![](<Screenshot 2024-11-25 at 17.36.54.png>){ width="50%" }
    - これで入力と出力となるポートを作成する
8. Mesh の指定
    - Mesh を細かく切りたいオブジェクトを選択し、```Assign Mesh Operation``` -> ```On Selection``` -> ```Length Based``` を選択
        - ![](<Screenshot 2024-11-25 at 17.44.06.png>){ width="50%" }
        - ![](<Screenshot 2024-11-25 at 17.44.19.png>){ width="50%" }
9.  Analysis の setup
    - ```Analysis``` -> ```Add Solution Setup``` -> ```Advanced```
        - ![](<Screenshot 2024-11-25 at 17.45.06.png>){ width="50%" }
        - ![](<Screenshot 2024-11-25 at 17.45.40.png>){ width="50%" }
        - ![](<Screenshot 2024-11-25 at 17.45.46.png>){ width="50%" }
    - Frequency Sweep を追加する
        - ![](<Screenshot 2024-11-25 at 17.46.29.png>){ width="50%" }
10. Analysis を走らせる
    - ```<Setup した Analysis>``` -> ```Analyze``` 
    - ![](<Screenshot 2024-11-25 at 17.46.53.png>){ width="50%" }
11. Results の確認
    - ```Results``` -> ```Create Modal Solution Data Report``` -> ```Rectangular Plot```
        - ![](<Screenshot 2024-11-25 at 17.56.38.png>){ width="50%" }
    - Solution から解析したい setup を指定することを忘れずに
        - ![](<Screenshot 2024-11-25 at 17.57.57.png>){ width="50%" }
    - 図のような S-parameter の plot が表示される。
        - ![](<Screenshot 2024-11-26 at 13.30.07.png>){ width="50%" }