
- IQM Qiskit : https://docs.meetiqm.com/iqm-client/user_guide_qiskit.html
- IQM Pulla (for pulse-level access)
	- https://docs.meetiqm.com/iqm-pulla/
	- https://docs.meetiqm.com/iqm-pulse/

## IQM Qiskit
### Step

1. 使用する量子コンピュータにトークンを使用してアクセス
2. ゲート操作のための回路図やブロックダイアグラムを書く
3. トランスパイルで作成した回路図を変換
4. 量子コンピュータにトランスパイルしたものを submit
5. job から results を取得


> [!info] コンパイルとトランスパイルの違い
> コンパイル → 高級言語 → 低級言語（機械が直接理解できる形へ変換）
> トランスパイル → 高級言語 → 高級言語（人間が読める形のまま別の言語へ変換）


### 回路の実行結果のSimulation

- StateVector or Density Matrix
- Ideal simulation : AerSimulator
- Noisy simulation : IQM Fake backend

#### StateVector or Density Matrix

- https://quantum.cloud.ibm.com/docs/en/guides/plot-quantum-states

```python
from math import pi
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector

# Create a Bell state for demonstration
qc = QuantumCircuit(2)
qc.h(0)
qc.crx(pi / 2, 0, 1)
psi = Statevector(qc)

psi.draw("latex")
psi.draw("city")
psi.draw("hinton")
```

#### Noisy Simulation

https://docs.meetiqm.com/iqm-client/user_guide_qiskit.html#noisy-simulation-of-quantum-circuit-execution

IQM では QC のアーキテクチャを模した Fake backend が提供されており、それを用いたシミュレーションを行うことができる。mock を使う場合は、ランダムな結果しか得られない。
提供されている Fake backend の種類は[ここ](https://iqm-finland.github.io/qiskit-on-iqm/api/iqm.qiskit_iqm.fake_backends.iqm_fake_backend.IQMFakeBackend.html)で確認することができる。(2025年10月時点で提供されているのは、adonis, aphrodite, apollo, deneb, garnet)
この中で star architecture は deneb のみのため、sirius の simulation を行いたい場合は deneb を使用する？

> [!warning]
> Fake backend は量子ビットの寿命などを設定できるが、T1 測定などを行っても指数関数分布が見えるわけではないので、delay などはシミュレーションできない？


### Readout Error Mitigation
Garnet machine などは readout error が大きいため、Readout Error Mitigation (REM) を行うことで結果がかなり改善できる。
REM は各 Qubit を別々に励起したときに、他の Qubit が励起される割合などを示す confusion matrix を取得し、その逆行列を REM を行いたい結果に対してかけることで補正する。
Qiskit にはこれを行うための package が用意されており、LocalReadoutError と CorrelatedReadoutError の 2種類の補正方法がある。
前者は、量子ビットが全て基底状態もしくは励起状態の場合に観測される状態を用いて confusion matrix を作成するため、走らせる回路数は 2 つのみ。
後者は、すべての状態ベクトルを生成する回路を準備することで confusion matrix を作成するため、精度は良くなるが、回路数は bit 数によって増加する。
上記の module は IQM backend ではそのまま動かないため、以下のように少し修正する必要がある。

```python
from qiskit_experiments.library import LocalReadoutError, CorrelatedReadoutError

qubits = [0,1,2]
backend = ns_arxiv.fakebackend

exp = LocalReadoutError(qubits, backend=backend)
for c in exp.circuits():
	print(c.metadata) # ここに {'state_label': '000'} のような情報が入っている
	print(c)

exp.analysis.set_options(plot=True)
exp.set_run_options(shots=1000)

# analysis does not run for IQM fake & real backend since circuit metadata is not correctly propagated
# backend_run must be set to true to retrieve data from IQM backend
result = exp.run(analysis=None, backend_run=True).block_for_results()

expdata = ExperimentData(experiment=exp, backend=exp.backend)
for i, circ in enumerate(exp.circuits()):
	expdata.add_data({
		"counts": result.data()[i]['counts'],
		"metadata": circ.metadata, # ここで state_label を明示的に補完
		"shots": 1000,
})

# Rerun analysis with metadata (state_label)
analysis_result = exp.analysis.run(expdata).block_for_results()
display(analysis_result.figure(0))
```

### Quantum State Tomography

測定結果から city plot などを作成するには以下のように StateTomography 用の回路を作成する必要がある。
https://qiskit-community.github.io/qiskit-experiments/stubs/qiskit_experiments.library.tomography.StateTomography.html

### Benchmark

IQM では QC の性能を測定するための benchmark script が[ここ](https://github.com/iqm-finland/sdk/tree/main/iqm_benchmarks/src/iqm/benchmarks)に色々と用意されている。
例えば、量子ビットの性能を測定するための T1, T2 echo 測定を[ここ](https://iqm-finland.github.io/iqm-benchmarks/examples/example_coherence.html)に書いてあるように行うことができる。

### Calibration ID と Qubit mapping の取得


### API で Qubit や calibration data を取得する方法

https://resonance.meetiqm.com/docs/api-reference?api=server&qc=emerald が 2025年10月時点で使用可能な API の instruction。
HTTP を使用しており、Qubit の lifetime や共振周波数などの情報を取得することができる。
Terminal から json file の情報を取得する場合のコマンドは、

```bash
curl -H "Authorization: Bearer <token>" https://resonance.meetiqm.com/api/v1/quantum-computers/emerald/artifacts/chip-design-records
```

python script を使用する場合は、以下のように書く

```python
url = ( f"https://resonance.meetiqm.com/api/v1/calibration-sets/{system}/{cal_set}/metrics")
headers = {"Accept": "application/json", "Authorization": f"Bearer {token}"}
r = requests.get(url, headers=headers)
calibration = r.json()

# dut field の取得
for iq in calibration['observations']:
	dut_field = iq['dut_field']
	
	# characterization.model.QB1.t1_time
	# characterization.model.QB1.t2_time
	# characterization.model.QB1.t2_echo_time
	# metrics.rb.clifford.xy.QB1.fidelity
	# metrics.rb.clifford.xy.QB1.fidelity:par=d1
	# metrics.rb.prx.drag_crf.QB1.fidelity:par=d1
	# metrics.ssro.measure.constant.QB1.fidelity
```

> [!info] API
> API とは Application Programming Interface の略称で、異なるソフトウェアやアプリケーションが相互にデータや機能を利用するための「窓口」となる仕組みを示している。

### transpile_to_IQM
- https://docs.iqm.tech/iqm-client/api/iqm.qiskit_iqm.iqm_naive_move_pass.transpile_to_IQM.html

> [!NOTE]
> Customized transpilation to IQM backends.
> Works with both the Crystal and Star architectures.
> 
> Note: When transpiling a circuit with MOVE gates, you might need to set `optimization_level` lower. If `optimization_level` is set too high, the transpiler might add single qubit gates onto the resonator, which is not supported by the IQM Star architectures. If this in undesired, it is best to have the transpiler add the MOVE gates automatically, rather than manually adding them to the circuit.

### その他
#### Histogram の作成

```python
from qiskit.visualization import plot_distribution, plot_histogram

legend = ['Mitigated Probabilities', 'Unmitigated Probabilities']

# plot_histogram は counts をそのまま表示し、plot_distribution は 1 に規格化される

plot_distribution(
	[mitigated_probs, unmitigated_probs], # counts の dictionary
	legend=legend, 
	sort="value_desc", 
	bar_labels=False
)
```



## IQM PulLa (Pulse level access)

- Tutorial
	- https://www.iqmacademy.com/tutorials/
	- https://docs.meetiqm.com/iqm-pulla/examples.html

> [!warning] Pulla を使用するときの注意点
> - Mock environment に投げることはできないので、実際に走らせるしかない
> - 

### Calibration ID の取得方法

```
p = Pulla(url)
p.fetch_default_calibration_set()[1]
```

### Gate implementation

- https://docs.meetiqm.com/iqm-pulla/Custom%20Gates%20and%20Implementations.html#
#### Gate について

- IQM Pulla ではゲート操作の実装方法の変更や新たなゲート操作を定義することができる
- 使用可能なゲート操作は以下のように確認することができる
```python
p = Pulla(url)

compiler = p.get_standard_compiler()
compiler.print_all_implementations_trees()
```
- 各 gate には calibration data (duration, frequency, width, amplitude) が設定されており、以下から実際の値を確認することができる。
```python
print( compiler.get_calibration_set_values() )
```
gate の calibration data の key は以下のように設定されている。

```
gates.<gate名>.<implementation>.<qubit>.<parameter名>
# gates.prx.drag_crf_sx.QB14.amplitude_i': 0.16482464499820992
```

#### Default implementation の切り替え

Default gate には複数の implementation が設定されている場合がある。
例えば `prx` gate には `drag_gaussian`, `drag_crf`, `drag_crf_sx`, `drag_gaussian_sx` と 4 つの implementation が設定されており。
```python
compiler.set_default_implementation('cz', 'slepian')
compiler.set_default_implementation_for_loci('cz', 'tgss', [('QB1', 'QB2')])
```

#### 既存のゲート操作をコピーして、パラメータのみ書き換える

以下では既存の `prx_12` pulse をそのままコピーして、calibration data のみを変更した `prx_23` pulse を定義する。
まずは `prx_12` pulse がどのように定義されているか確認する。
Anaconda を使用している場合、default gate の実装は `/opt/anaconda3/envs/iqm-pulla/lib/python3.11/site-packages/iqm/pulse/gates/default_gates.py` から確認することができる。

```python
_implementation_library: dict[str, dict[str, type[GateImplementation]]] = {
	...
	"prx": {
		"drag_gaussian": PRX_DRAGGaussian,
		"drag_crf": PRX_DRAGCosineRiseFall,
		"drag_crf_sx": PRX_DRAGCosineRiseFallSX,
		"drag_gaussian_sx": PRX_DRAGGaussianSX,
		},
	"prx_12": {
		"modulated_drag_crf": PRX_ModulatedDRAGCosineRiseFall,
	},
}
```
上記より、`prx_12` は `PRX_ModulatedDRAGCosineRiseFall` で定義されていることが分かる。
`prx` は複数の implementation があり、それぞれ別の class で定義されている。
`PRX_ModulatedDRAGCosineRiseFall` などの class は `/opt/anaconda3/envs/iqm-pulla/lib/python3.11/site-packages/iqm/pulse/gates/prx.py` で定義されており、import することができる。
以下では `prx_12` pulse の implementation をそのままコピーして、`prx_23` pulse を新たに作成し、`amplitude_i` と `frequency` のみを変更している。
```python
from iqm.pulse.gate_implementation import CompositeGate
from iqm.pulse.gates.prx import PRX_ModulatedDRAGCosineRiseFall
from iqm.pulse.quantum_ops import QuantumOp
import numpy as np
from math import sqrt

compiler.add_implementation('prx_23', 'modulated_drag_crf', PRX_ModulatedDRAGCosineRiseFall, quantum_op=QuantumOp('prx_23', params={'angle': (float,), 'phase': (float,)}))

  

for qubit in qubits:
	cal_dict = compiler.get_calibration_set_values()
	common_key = f"gates.prx_12.modulated_drag_crf.{qubit}."
	custom_cal_data = {
		"duration" : cal_dict[ common_key + "duration" ],
		"amplitude_i" : cal_dict[ common_key + "amplitude_i" ] * sqrt(3./2.), # Not sure why this number works...
		"amplitude_q" : cal_dict[ common_key + "amplitude_q" ],
		"frequency" : cal_dict[ common_key + "frequency" ] * 2,
		"full_width" : cal_dict[ common_key + "full_width" ],
		"center_offset" : cal_dict[ common_key + "center_offset" ]
}

	# Amend default calibration configuration
	compiler.amend_calibration_for_gate_implementation('prx_23', 'modulated_drag_crf', (qubit,), custom_cal_data)
```

### 量子ビットの flux voltage や AWG drive frequency などの取得方法

- https://docs.meetiqm.com/iqm-pulla/Configuration%20and%20Usage.html#basics

### Flux bias や measurement constant を書き換える方法 (IQ 平面上の結果を得る方法)

- https://docs.meetiqm.com/iqm-pulla/Configuration%20and%20Usage.html#complex-readout