
# QUP

- [XLD400各種情報](https://docs.google.com/document/d/16otyHMNd5tmi_xl9mQ8esiEbh2H0Zfm6/edit?usp=sharing&ouid=109150202109451728792&rtpof=true&sd=true)
- [Measurement manual](https://docs.google.com/document/d/1XVcxPWp-U0HxeLB7X69TAbZOD16N44kaFmRCdPP3zPc/edit?usp=sharing) (by 中村くん)

## 測定準備
---
### 配線
- RF line や DC line に何がつながっているかはホワイトボードに書かれている
- RF line は 1 ~ 7 と 1 ~ 10 の 2 セットが書かれているが、これはポートが二つあることに対応している。希釈の外のラインの番号は上から連続で 1 ~ 17 に対応するので、2 セット目の 1 ~ 10 は 8 ~ 17 と読み替える必要がある。
- RF line は手前から 1 ~ 4, 5 ~ 8 ... と番号が続いている。(写真の C が 2 番目)
![[IMG_5595.jpg|300]]

### Switch の切り替え

- Switch に何がつながっているかはホワイトボードに
- 黄色の線が Switch A, 黒色の線が Switch B に対応している
- ON にしている Line が n 番目とすると、n 番目の線を赤の鰐口クリップに、n+1 番目の線を黒色の鰐口クリップに挟んでスイッチを押す

![[IMG_5593.jpg|300]]![[IMG_5594.jpg|300]]

### TWPA の設定

- CWを使って特定の周波数と強度のpump toneを入れてやる必要がある
- VNA を使って RF power を確認できるようにしておく
	- ==Power は -40 dB 程度 (Lamb shift 後の値程度) に設定しておく==
- TWPA 用の CW の USB を PC に繋ぐ
- ソフトを起動する
	- 起動した瞬間に CW の output が始まるので、RF Enable ボタンを押して止める
	- RF frequency と Power を調整し、RF Enable を押す
	- VNA の出力を確認しながら、Gain が最も大きくなる値に Power と RF frequency を調整

> [!warning] 
> Labtop の電源のコンセントが刺さっているかを必ずチェックしておく
> 測定中に Sleep mode に入ると TWPA が切れる


![[IMG_5597.jpg|300]]
![[IMG_5598.jpg|300]]![[IMG_5603 1.jpg|300]]
![[IMG_5600.jpg|300]]


## 測定マニュアル
---
### Thermal Excitation の測定

1. VNA で Resonator の peak の確認 (Lamb-shift 前後の peak の位置を記録しておく)
	- `qup_exp_qcodes/power_scan_M9804A.ipynb` で power scan も可能
2. Time domain 用の配線に変更する
3. `01. time_of_flight.ipynb` で time of flight から適切な ==acquisition_delays== を取得
	- 正しく測定できているときは plot が山型になるので、最大となる時間を acquisition_delays として設定
4. `02-b resonator_spectroscopy_2D.ipynb` で ==readout_pulse_amplitudes== を最適化
5. `02-a resonator_spectroscopy.ipynb` で設定した readout_pulse_amplitudes が最適かを確認
	- resonator の peak の位置を確認し、==readout_frequencies== を最適化
6. `03. qubit_spectroscopy.ipynb` で Qubit の frequency を確認し、==xy_pulse_frequencies== を最適化
7. `05-c time_rabi.ipynb` で rabi oscillation を確認し、Fit 結果から周期の半分を ==xy_pulse_durations== として設定
8. `07. t1.ipynb` で T1 を測定
9. `08. ramsey.ipynb` で T2r を測定
	- detuning frequency からのずれを用いて、==xy_pulse_frequencies== をさらに最適化
10. `02-d resonator_spectroscopy_dispersive_shift.ipynb` で Rx pulse あり/なしの場合の S21 を確認
	- e-state に対応する resonator peak の位置を確認し、==readout_frequencies== を変更
11.  `03. qubit_spectroscopy_f21.ipynb` で Qubit の frequency (f21) を確認し、==x12_pulse_frequencies== を最適化
12. `05-d time_rabi_f21.ipynb` で rabi oscillation を確認し、Fit 結果から周期の半分を ==x12_pulse_durations== を設定
13. `08-b. ramsey_f21.ipynb` で T2r を測定
	- detuning frequency からのずれを用いて、==x12_pulse_frequencies== をさらに最適化
14. `02-e. resonator_spectroscopy_gef.ipynb` で f-state, e-state, g-state の dispersive shift を確認する
15. `05-e. time_rabi_thermal_excitation.ipynb` で熱励起を測定


## Shutdown
---


- QCS はそのまま起動しておいても良い
- 以下の電源を切っておく
	- HEMT の電源
	- LNFの電源
	- 室温アンプの電源
	- TWPA のソフト(PC から × ボタンで落とすだけ)
- - 昇温の際には、新田さんに連絡する

![[IMG_5601.jpg|300]]![[IMG_5604.jpg|300]]
![[IMG_5602.jpg|300]]
