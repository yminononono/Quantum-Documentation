# SCPI

SCPI (Standard Commands for Programmable Instruments) は測定器の統一的な制御を目的としたコマンド群を示している。  

- [自動計測におけるGPIB、LAN、USBとは。各インターフェースの特徴と選択方法](https://www.keysight.com/jp/ja/lib/resources/training-materials/resource-2304657.html)
- [Advantest R3768 のマニュアル](https://www3.advantest.com/documents/11348/146752/pdf_mn_JR3860A_3770_3768_OPERATING%20MAMUAL.pdf)
- [Keysight の VISA library を用いた GPIB、PyVISAで完全自動測定 ](https://qiita.com/go_yu_/items/2c03b7c2570628059b18)

## GPIB と VISA

VISA（Virtual Instrument Software Architecture）とはSCPI環境で使用されるライブラリであり
インターフェース（RJ45、RS-232、USB、GPIB）の違いを吸収し、統一的な環境を提供する。

- [NI-VISA, NI-488.2 などのソフトウェアの構成について](https://knowledge.ni.com/KnowledgeArticleDetails?id=kA00Z0000019XKkSAM&l=ja-JP)
- [NI-488.2 のダウンロード](https://www.ni.com/ja/support/downloads/drivers/download.ni-488-2.html?srsltid=AfmBOooj28ohyYZohEJhhl67ltz_W5HY5YhIbm-58OgZm_Do-qM9Vdxz#544048)
- [NI-VISA のダウンロード](https://www.ni.com/ja/support/downloads/drivers/download.ni-visa.html?srsltid=AfmBOoq1qhdSAuvWoXnIa8Lw39QJvhhl6r-uwqwdGWBEIw-HoPJKVA4m#544206)
- [Rasberry Pi で 計測器制御 (GPIB-USB-HS経由)](https://zenn.dev/hroabe/articles/ceccb8ce114372#python%E3%81%A7%E8%A8%88%E6%B8%AC%E5%99%A8%E5%88%B6%E5%BE%A1%E3%81%99%E3%82%8B-(pyvisa))
- [GPIB計測器制御チュートリアル](https://knowledge.ni.com/KnowledgeArticleDetails?id=kA03q000000x2YDCAY&l=ja-JP)

GPIB を使って SCPI コマンドを送るためのアプリケーションとして

- National Instruments 
    - NI MAX (Measurement ＆ Automation Explorer)
    - NI-VISA Interactive Control 
- Keysight
    - Keysight IO Libraries Suite

を利用することができる。また python の API (pyVISA) を使った接続も可能である。

!!!note
    - アンインストール方法
        - keysight の場合は、OS -> Program Files -> Keysight -> uninstall のフォルダを探す
            - IO Libraries SUITE Main Uninstall
        - NI の場合は、NI package manager から uninstall

### Error code と対処法

- [タイムアウトする場合に、コマンドの終端文字が正しいか確認](https://pyvisa.readthedocs.io/en/latest/introduction/communication.html#getting-the-instrument-configuration-right)
- [高速なコンピュータに変更したらGPIBタイムアウトエラー（EABO）が発生する](https://knowledge.ni.com/KnowledgeArticleDetails?id=kA00Z0000019LvPSAU&l=ja-JP)
- [GPIB Error Codes and Common Solutions (Part 1)](http://digital.ni.com/public.nsf/allkb/2FA525A8585A92E9862566EE002A3755?OpenDocument)
- [GPIB Error Codes and Common Solutions (Part 2)](http://digital.ni.com/public.nsf/allkb/99CE192EABD83D6E86257F620051EBA9)
- [GPIB Error Codes and Common Solutions (Part 3)](http://digital.ni.com/public.nsf/allkb/463FA163E3B9FAA686256EFB00568691?OpenDocument)

## pyVISA

### インストール

```
$ conda create -n pyvisa python=3.11
$ conda install pyvisa
$ conda install PyVISA-py
$ conda install zeroconf
```

### 使い方

```python
import pyvisa
rm = pyvisa.ResourceManager()
rm.list_resources()
```

ここで pyvisa は VISA library を利用しているが、Natural instruments が提供する　NI-VISA などいくつか種類が存在する。
どの library を使用するかは ResourceManager に渡すパスによって変わる。

```python
rm = pyvisa.ResourceManager('@py')
```

pyvisa で利用できる library の場所などは以下のコマンドで確認することができる。

```
$ pyvisa-info
```

!!!warning
    NI の GPIB を使用したときは、list_resources から GPIB は確認できなかったが、NI-VISA instrument control から確認したデバイス名を入力するとアクセスすることができた。  


## Ethernet

[イーサネットによる Windows計測器のリモート制御の基本](https://dl.cdn-anritsu.com/ja-jp/test-measurement/files/Application-Notes/Application-Note/ms2840a-ms269xa-mg37x0a-jf1101.pdf)


## 直流電圧/電流発生器(Advantest R6144)との通信

- [GPIBでADVANTEST R6144を制御してみる](http://analog.tea-nifty.com/blog/2022/04/post-e036da.html)
- [R6144 のマニュアル](https://3d-yd.com/jpdf2/6144.pdf)

1. R6144 で GPIB を使用するように設定する
   - ```STEP``` -> ```local``` から ```A-XX``` という表示にする
   - ```XX``` が　GPIB の address になる
   - 設定が終わったら　```STEP``` で元のメニューに戻る

!!!warning
    BCD パラレルインターフェイスになっていると動かないので、GPIB の　address 設定画面になっているか確認する

!!!info
    GPIB が正しく接続できている場合には、NI-VISA Interactive Control や　PyVisa などで ```GPIB::1::INTFC``` のように表示される。
