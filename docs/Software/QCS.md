Keysight が提供している Quantum Control System (QCS)

参考文献
- https://docs.keysight.com/pages/viewpage.action?pageId=896926022
- Integration filter : https://fernandovldrs.github.io/posts/readout-opt

## KEK の測定手順

1. Time of flight 測定 : acquisition delay の調整
2. Resonator spectroscopy
	1. Lamb shift の観測
	2. Readout Power の調整
	3. Readout frequency の判定
3. Qubit spectroscopy
	1. Qubit の周波数の判定
4. Time Rabi
	1. Qubit の pulse duration の判定
5. Ramsey
	1. Qubit の周波数の微調整
	2. Time Rabi で再度 pulse duration の調整
6. Dispersive shift
	1. e-state の resonator frequency の判定
7. Qubit spectroscopy (f21)


## 新しい Sweep program を用意したい場合

Calibration Experiment や Ramsey などは Experiment を継承して、独自の configure_repetitions を実装している場合があるため、より自由に変数の Sweep が行いたい場合は Experiment を利用する。
ただし、Calibration Experiment などには calibration_set を書き換えるための method が用意されていたりするので、それを利用したい場合には利用すると良い・

Calibration Experiment : operation に linker の名前を渡すと、linker に設定された hardware operation に書き換えられて、その中の変数の sweep が可能となる


## Integration filter の実装

- Integration filter とは : https://fernandovldrs.github.io/posts/readout-opt

## Pulse の表示
以下のように表示可能

```python
experiment.render(
    channel_subplots=False,
    lo_frequency=5e9,
    sweep_index=0, # さらにsweepする変数が二つの場合は、(9,1)
    sample_rate=5e9, # デフォルトは 1e9 のため、500 MHz 以上のパルスの形はおかしくなる
)
```

## hdf5 file から metadata の表示

```python
program = qcs.load("swept_program.hdf5")
program.repetitions

NestedRepetition(Sweep(rf=Array(name=freq_vals, shape=(11, 4), dtype=float, unit=none)), Repeat(10))
```

## 定義済みの Gate の確認

```python
qcs.GATES.aliases

{'cx',
 'cy',
 'cz',
 'h',
 'id',`
 'iswap',
 'swap',
 'x',
 'x90',
 'y',
 'y90',
 'z',
 'z90'}
```



## Dynamical Decoupling (CMPG etc.)

https://docs.keysight.com/pages/viewpage.action?pageId=896926028&__cf_chl_f_tk=fz0bglMLWsN121.kDb7FSaT.FzV22lg5aJHu.IhljYg-1783408738-1.0.1.1-2Q.MTXl37d8nLOWIuSv8fG6C75fRR2OyKRTVnczn29c

spin echo を参考にした実装例 : 
```python
program = qcs.Program()

delay = qcs.Array("pulse_delay", shape=(len(qubits),), dtype=float)

program.add_gate(qcs.GATES.x90, qubits)
for i in range(n_pulses):
	if i == 0:
		program.add_gate(qcs.GATES.x, qubits, pre_delay=delay)
	else:
		program.add_gate(qcs.GATES.x, qubits, pre_delay=2*delay)
program.add_gate(qcs.GATES.x90, qubits, pre_delay=delay)

program.add_measurement(qubits)

echo_experiment = Experiment(
	backend=backend,
	calibration_set=calibration_set,
	targets=qubits,
	program=program,
	save_path=save_path,
)

echo_experiment.configure_repetitions(
	n_shots=n_shots, hw_sweep=hw_sweep, pulse_delay = delays
)
```