[PPMS manual](https://web.njit.edu/~tyson/PPMS_Documents/PPMS_Manual/1070-150%20Rev.%20B5%20PQ%20%20PPMS%20Hardware.pdf)
[ＰＰＭＳ抵抗](https://masuda.issp.u-tokyo.ac.jp/ppms/ayumi/ppmsres.html)
​
# 東大低温センター
---
## 使用登録
---
- **年度初めに** [https://www.crc.u-tokyo.ac.jp/openlab_HP/equipment_guide.html](https://www.crc.u-tokyo.ac.jp/openlab_HP/equipment_guide.html) から **“共同利用装置使用申請” を提出し、稲田研の登録を行う**
- 研究室が登録されると [PPMS予約](https://www.crc.u-tokyo.ac.jp/openlab_HP/ppmsschedule.html) から予約をする

## 使用方法
---
### ~~記録簿の記入~~(→ 2025年6月時点で google form への記入に変更)

- He 液量 : ディスプレイの左下に記載
- ~~流量計 : 右の壁に記載(PPMS と書いてあるメータ)~~

### サンプルの取り付け

> [!warning] 
> まずは sample に電流を流す状態になっていないかチェック
> ここで大きな電流が流れているとサンプルがショートする可能性がある

- vent cont で 1 気圧程度 (~730 Torr ?) に戻して蓋を開ける
- 1.5 K くらいの測定の場合
    - 棒の先にサンプルをつけて、ノッチがはまるように少し押す
    - 入ったらサンプルを外して棒を引き上げる
    - バッファーがついた蓋をつける(放射を防ぐため)
    - purge seal する
- He 3 option の場合
    - **専用のサンプルホルダーに取り付ける (<u>channel 3 は温度計用になっているので、2 channel しかつけられない</u>)**
    - channel 3 は接続しておかないと温度が見れないので、Bridge channels で必ず ON にしておく

### 測定

> [!info] 
> He3 engine が activate されていると、0.5 K まで温度を下げれるように sequence で選択範囲が広がる (option が activate されてないと 1.4 K までしか選べない)

- Instrument -> Bridge channels で入れた sample の抵抗を確認する
	- **<u>ここで Status に何も表示されていないときは後ろの配線が間違っている可能性があるので PPMS 担当者に確認する</u>**
- Utilities -> Activate Option で Resistivity を activate 状態にしないと抵抗値をログに残す sequence を使用できないので注意 (Measure に Resistivity や Scan Excitation などの選択肢が現れる)
	- Resistivity Option が表示されるので、データを保存する file を開いておく (sequence file に change data file を指定している場合でも、最初はとりあえず何か file を開いておかないと動かない)
	- 転移点付近で 0.002 K step で細かい scan をしたい場合、Scan Temperature の Approach は Fast Settle に設定しておく
- He3 option を使用する場合は、Activate Option で Helium 3 を activate する
- <u>保存する file を作成する</u> または <u>右の sequence commands から Measurement Commands -> Resistivity -> Change Datafile を選択して datafile を開くように変更する</u>
- Sequence を選択して run
	- 過去の sequence は `C:\QdPpms\Data\Resistivity\coop\icepp` に保存しているので、それを開いて 

> [!warning] 
> - He 3 の場合は、低温では温度が安定しにくいので、sweep よりも fast で行って、一点ごとに温度をきっちり合わせてから測定する
> - He 3 での温度が安定しない(Sequence の途中で chasing が終わらない)場合、一度測定したい最低温度まで温度を下げてから温度を上げながら測定した方がやりやすい
> - 温度変化などを調べた場合、He3 option の logger から VIEW を押してグラフを表示すると良い

### IV scan

- scan excitation を右から選んで sequence に入れる
- 出力するファイルを変更するような sequence も作成できる

> [!warning]
> Scan excitation では voltage や current に対して upper limit を設けているため、例えば一つの channel の抵抗値が高い場合、scan の途中で limit に引っかかって測定が止まってしまう場合がある。それを防ぐために、scan excitation で測定を行わない channel は bridge configuration で該当 channel を off にしておく。( No action だと disable したはずの channel の電流値は動いてしまうので、off に設定)


### 立ち下げ

- 最後は 300 K に戻して sample を取り出しておく (sequence でも手でも)
- google form から使用簿の記入