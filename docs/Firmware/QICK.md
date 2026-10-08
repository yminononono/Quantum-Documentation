# QICK

## RFSoC 4✖️2

- 2x RF DAC (9.85 GSPS)
- 4x RF ADC (5 GSPS)

Gigasample per second (GSPS)

## QICK configuration の取得 

```python
from qick import *
# Load bitstream with custom overlay
soc = QickSoc()
soccfg = soc
print(soccfg)
```

<details><summary> Qick configuration の出力例 (ZCU118 の場合)</summary>

```text
QICK configuration:

	Board: ZCU111

	Software version: 0.2.181
	Firmware timestamp: Wed Aug 16 13:39:03 2023

	Global clocks (MHz): tProcessor 384.000, RF reference 204.800

	7 signal generator channels:
	0:	axis_signal_gen_v6 - tProc output 1, envelope memory 65536 samples
		DAC tile 0, blk 0, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz
	1:	axis_signal_gen_v6 - tProc output 2, envelope memory 65536 samples
		DAC tile 0, blk 1, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz
	2:	axis_signal_gen_v6 - tProc output 3, envelope memory 65536 samples
		DAC tile 0, blk 2, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz
	3:	axis_signal_gen_v6 - tProc output 4, envelope memory 65536 samples
		DAC tile 1, blk 0, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz
	4:	axis_signal_gen_v6 - tProc output 5, envelope memory 65536 samples
		DAC tile 1, blk 1, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz
	5:	axis_signal_gen_v6 - tProc output 6, envelope memory 65536 samples
		DAC tile 1, blk 2, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz
	6:	axis_signal_gen_v6 - tProc output 7, envelope memory 65536 samples
		DAC tile 1, blk 3, 32-bit DDS, fabric=384.000 MHz, f_dds=6144.000 MHz

	2 readout channels:
	0:	axis_readout_v2 - controlled by PYNQ
		ADC tile 0, blk 0, 32-bit DDS, fabric=512.000 MHz, fs=4096.000 MHz
		maxlen 16384 (avg) 1024 (decimated)
		triggered by output 0, pin 14, feedback to tProc input 0
	1:	axis_readout_v2 - controlled by PYNQ
		ADC tile 0, blk 1, 32-bit DDS, fabric=512.000 MHz, fs=4096.000 MHz
		maxlen 16384 (avg) 1024 (decimated)
		triggered by output 0, pin 15, feedback to tProc input 1

	7 DACs:
		DAC tile 0, blk 0 is DAC228_T0_CH0 or RF board output 0
		DAC tile 0, blk 1 is DAC228_T0_CH1 or RF board output 1
		DAC tile 0, blk 2 is DAC228_T0_CH2 or RF board output 2
		DAC tile 1, blk 0 is DAC229_T1_CH0 or RF board output 4
		DAC tile 1, blk 1 is DAC229_T1_CH1 or RF board output 5
		DAC tile 1, blk 2 is DAC229_T1_CH2 or RF board output 6
		DAC tile 1, blk 3 is DAC229_T1_CH3 or RF board output 7

	2 ADCs:
		ADC tile 0, blk 0 is ADC224_T0_CH0 or RF board AC input 0
		ADC tile 0, blk 1 is ADC224_T0_CH1 or RF board AC input 1

	8 digital output pins:
	0:	PMOD0_0_LS (output 0, pin 0)
	1:	PMOD0_1_LS (output 0, pin 1)
	2:	PMOD0_2_LS (output 0, pin 2)
	3:	PMOD0_3_LS (output 0, pin 3)
	4:	PMOD0_4_LS (output 0, pin 4)
	5:	PMOD0_5_LS (output 0, pin 5)
	6:	PMOD0_6_LS (output 0, pin 6)
	7:	PMOD0_7_LS (output 0, pin 7)

	tProc axis_tproc64x32_x8: program memory 8192 words, data memory 4096 words
		external start pin: PMOD1_0_LS

	DDR4 memory buffer: 1073741824 samples, 256 samples/transfer
		wired to readouts [0, 1], triggered by output 0, pin 8

	MR buffer: 8192 samples, wired to readouts [0, 1], triggered by output 0, pin 9
```
</details>

## RFSoC を用いた繰り返し処理

```AveragerProgram``` などの Helper class が用意されているので、