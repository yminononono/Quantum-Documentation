# Material & Components

## Material

### 転移温度

よく利用する超伝導体の転移温度は以下のサイトにまとめられている。

### 電気抵抗率

- [各種物質の電気抵抗率](https://www.hakko.co.jp/library/qa/qakit/html/h01100.htm)

転移温度測定用のベタ膜を測定したいときなどに、電気抵抗率$\rho\;\mathrm{[\Omega\cdot m]}$などからおおよその抵抗が求められる。

|  金属 |  電気抵抗率 [$\Omega\cdot\mathrm{\mu m}$] | 参考文献 | 
|:---:|:---:|---|
| Al  | 0.025   | | 
| Nb  | 0.152   | |
| TiN | 0.78 (?) | https://www.chemicalbook.com/ChemicalProductProperty_JP_CB6761156.htm | 

例えば、

- Nb で作成した厚み $150\;\mathrm{nm}$、幅 $20\mathrm{\mu m}$、長さ $2200\mathrm{\mu m}$ の抵抗測定用のサンプルの抵抗は、$R = (0.152 \times 10^{-6}) \times \frac{2200 \times 10^{-6}}{150 \times 10^{-9} \times 20 \times 10^{-6}} \approx 110\;\Omega$ 程度の値になる。
- Al で作成した厚み $100\;\mathrm{nm}$、幅 $20\mathrm{\mu m}$、長さ $5600\mathrm{\mu m}$ の feed line を持つ 2D 量子ビットの抵抗は、$R = (0.025 \times 10^{-6}) \times \frac{5600 \times 10^{-6}}{100 \times 10^{-9} \times 20 \times 10^{-6}} \approx 70\;\Omega$ 程度の値になる。


## Components

### 同軸コネクタ

[同軸コネクタ基本ガイド](https://jp.rs-online.com/web/content/discovery/ideas-and-advice/coaxial-connectors-guide?srsltid=AfmBOoo2KrCA4bnH1LJvDSBPpP7ftIidNAlH-xXoRbAT2oDZajBNNoSC)

- SMA （Sub Miniature Type A）コネクタ
    - マイクロウェーブ用として高性能小型コネクタのひとつに数えられ、使用できる周波数の範囲も広く、また耐久性に優れている。
    - SMA コネクタの種類
        - レセプタクル : ピンを受け入れる側のコネクタで、メス型のコネクタとも呼ばれる。ソケットやジャックとも呼ばれ、英語のReceptacleは「受け口」や「受容器」という意味。嵌合部の反対は、機器の基板（プリント基板）やパネル機器、同軸ケーブルに取り付ける。
        - アダプタ : SMAのオスまたはメス開口部を両端に持つコネクタで、延長・中継・分岐といった役割をもつ。ケーブル接続はせず、コネクタ同士を繋ぐ。
- BNC コネクタ
    - 同軸ケーブルに使用される小型のクイック接続/切断無線周波数コネクタである。

| コネクタの種類 |     結合方式     | 使用周波数範囲 |  特徴　|
| :------: | -------- | -------- | -------------- |
| BNC | バヨネットロック | 4GHz以下 | 同軸コネクタとして、最も一般的。 バヨネット式で脱着が容易。嵌合後にケーブルの軸回転が可能で扱い易い。 |
| SMA | ねじ | 12.4 GHz 以下  | 形状が小さく、ケーブルとの段差が少ないので広帯域まで対応する。 マイクロ波機器にも広く採用されている。 |

## Equipment

### 装置

| 装置名  | 用途 | 例 |
|:---:|---| --- |
| DC電流電圧源 | 正確な電圧または電流を印加する | [Advantest R6144](https://3d-yd.com/jpdf2/6144.pdf) |
| マルチメータ<br>(DMM : Digital MultiMeter) |　電圧計、電流計、抵抗計の機能を兼ね備えた装置 | | 
| ソースメータ<br>(SMU : Source Measure Unit)  | 電圧または電流を正確に印加すると同時に、電圧、電流を測定できる機器  | [横河計測 GS210]()|


### ピエゾ素子

ピエゾ（圧電素子）は印加電圧を変えることにより、ナノメートル領域の極めて微小な伸張の変化を起こすことができる。