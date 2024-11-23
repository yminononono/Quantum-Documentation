# Qiskit-metal

## Ansys HFSS で電磁界シミュレーション

[高周波デバイスの設計とシミュレーション](http://accwww2.kek.jp/oho/oho08/txt08/02%20yoshida2.080825p.pdf) の p.11 を参照。
Ansys HFSSには3つの解析方法があり、解析したいモデルの状況により適切に解析方法を選ぶ必要がある。 
- Driven Modal : 伝搬モードに着目した電磁界解析。導波管などの解析に用いる。 
- Driven Terminal : ノードに着目した電磁界解析。デジタル信号の解析に用いる。 
- Eigenmode : 固有値解析。どんな電磁波が閉じ込めることができるかを調べる解析。 

### Lumped port

[オープンリング共振器を用いた高周波帯非接触インターフェイスの研究](https://ohnolab.deca.jp/wp-content/lab_data/pdf_a/2011_M_Abe_doc.pdf)の p.45 を参照。
HFSS 上で給電部分の設定は Wave port と Lumped port の 2 つが使用できる。  
ここで、Wave portとは「自然給電」のことであり、port が設定されている面の特性インピーダンスを解析し、その結果として得られた特性インピーダンスで給電するものである。  
一方、Lumped portとは「強制給電」のことであり、portが設定されている面に対して解析者が指定した特性インピーダンスで強制的に給電するものである。通常、実験環境をシミュレーションに考慮する場合、Lumped portを仮定する。  
しかし、port面が無反射なWave portに対して、Lumped port はport面では反射が発生する。  

### シミュレーションの CPU Core の変更

```Tools``` -> ```Edit Active Analysis Configuration``` から使用するコア数を変更することができる。

![alt text](<Screenshot 2024-11-21 at 17.12.16.png>)

### 電磁場解析

```Analysis``` -> ```Setup``` -> 電磁場解析を行いたい setup の ```property``` から ```3D Fields Save Options``` -> ```Save Fields``` で frequency sweep を行う際に電磁場も保存するようにする。
![alt text](<Screenshot 2024-11-22 at 16.43.41.png>) 

保存した電磁場を表示するには、表示したいオブジェクトを選択した状態で ```Field Overlays``` を右クリックし、```Plot Fields``` -> ```E``` -> ```Mag_E``` で設定画面を表示する。  
表示した画面の ```Setup``` から Fast sweep を行った analysis を選択肢し、電磁場の周波数を選択する。
![alt text](<Screenshot 2024-11-22 at 16.44.20.png>)

### 実際の script の書き方

以下では 1 本のフィードラインに同じ共振周波数の 2 本の resonator を capacitive に coupling させて読み出す場合の script で説明する。  
一方の読み出しをフィードラインに、もう一方を resonator の先につけている。

![alt text](<Screenshot 2024-11-21 at 10.32.03.png>)

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
![alt text](<Screenshot 2024-11-21 at 10.00.45.png>)

また実際にどのようにメッシュが切られたか確認するには、  
1. Ansys でオブジェクトを選択 (全て選択したい場合は、```Sheets``` を右クリックして全選択する。)

![alt text](<Screenshot 2024-11-21 at 11.11.15.png>)

2. 選択した状態で ```FieldOverlays``` を右クリックして ```Plot Mesh``` を選択する。

![alt text](<Screenshot 2024-11-21 at 11.13.46.png>)
