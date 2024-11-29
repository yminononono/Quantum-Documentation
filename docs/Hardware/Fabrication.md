# Fabrication

## ナノハブ

### 多元スパッタリング装置 (B02)

[装置マニュアルのスキャン](https://drive.google.com/file/d/1CDGH-UQ3srnKDo0zAdyyckcSyTnAlaUv/view?usp=drive_link)

B01 も多元スパッタリング装置だがターゲットは固定で、B02 は要望によってターゲットの交換を行うことできる。

#### 装置予約

- ターゲットの交換は技術職員が行うため、装置予約の前に諫早さんにメールでターゲットの連絡を行う。
- 窒素を使用する場合は酸素との交換が必要になるため、装置担当者に事前連絡

#### 装置準備

- 成膜面を下に向けて、サンプルを基板トレーに設置

!!!warning
    - 手袋の着用を忘れずに
    - 装置の起動・停止は不要 -> 触るのはロードロックチェンバー(LL室)・オペレーションパネルくらい
    - 基板トレーに何も乗せずにスパッタリングをしないこと
        - ウエハーを半分に割ってから置いたりしないこと
        - 例えば3連トレーを用いる場合に使わない場所があってもダミーウエハーで埋める

#### レシピの作成

レシピは 11 番まで標準のものが用意されており、12 番以降はユーザーで登録したものが用意されている。
京都の量子チームでは主に以下のレシピを使用する。

| レシピ番号 | Target |       説明       | 成膜レート |
| ---------- | ------ | ---------------- | ------- |
| 3          | Al     | 標準レシピ       | 5.2 (5.7) nm/分|
| 7          | Ti     | 標準レシピ       | 5 nm/分 |
| 35         | Nb     | CMB の武藤レシピ | |


- T/S : Target と Sample の距離 (125 mm に設定)
- Rotate speed : 10.0 rpm
- TGT SEL : どの番号にどの Target が設置されているかボードに書いてあるので、成膜したい金属に合わせて調整する。

#### 自動運転

!!!warning
    - 基板トレーが LL 室に戻ってきた際に場所がずれている可能性があるため、次の充填の際にちょっと揺らしてみて確認する
    - 緊急時は非常停止 (EMO) スイッチを押して対処
    - 操作中に問題やトラブルが発生したときは、装置担当者(担当者不在の場合は他の技術職員)に連絡

#### 装置使用後

- 標準レシピで HEAT を ON にしたら最後に OFF に戻す

!!!warning 装置使用後
    - 最後ウェハーを取り除いたら真空を引いた状態で終了する