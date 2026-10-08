
## 量子回路の状態確率を確認する方法

```python

# Define a F_gate
def F_gate(circ,q,i,j,n,k) :
	theta = np.arccos(np.sqrt(1/(n-k+1)))
	circ.ry(-theta,q[j])
	circ.cz(q[i],q[j])
	circ.ry(theta,q[j])
	# circ.barrier(q[i])

def w_state(n):
	qr = QuantumRegister(n, 'q')
	qc = QuantumCircuit(qr)

	qc.x(qr[n-1]) #start is |100...0>

	for i in range(n-1):
		F_gate(qc,qr,n - i - 1, n - i - 2, n, i + 1 )
	for i in range(n-1):
		qc.cx(qr[n-i-2],qr[n-i-1])

	return qc

from qiskit.quantum_info import Statevector
qc = w_state(3)
sv = Statevector.from_instruction(qc)
sv.probabilities_dict()
```

以下のように、出力される状態と確率が dictionary として保存されている

```
{
np.str_('001'): np.float64(0.3333333333333333), np.str_('010'): np.float64(0.3333333333333333), np.str_('100'): np.float64(0.3333333333333333)
}
```



## Gate

- cx : CNOT
- ccx : toffoli gate

## measure

量子ビットの測定では、measure や measure_all という関数を用いることができる。
measure を使用する際は、第一引数に量子ビット、第二引数に結果を書き込む古典ビットを設定する。
measure_all は古典ビットおよび測定前に barrier が自動的に追加される。

## SABRE method

- https://quantum.cloud.ibm.com/docs/en/tutorials/transpilation-optimizations-with-sabre
- https://quantum.cloud.ibm.com/docs/en/guides/transpiler-stages

Transpile では量子回路を実際のハードウェアに実装できるような回路に変換する。
特に離れた量子ビット間の二量子ビットゲートを打つには、大量の SWAP ゲートが必要となるがコストが高くなる。
そこで SABRE method という手法を用いることで SWAP gate を最小化するような量子回路を作成する。

> [!NOTE]
> Transpilation converts quantum circuits into forms compatible with specific quantum hardware. Two key stages are choosing a **qubit layout** (mapping logical qubits to physical qubits) and **gate routing** (inserting SWAP gates so multi-qubit gates respect device connectivity).
> 
> **SABRE** (_SWAP-Based Bidirectional heuristic search algorithm_) optimizes both layout and routing. It is especially effective for large-scale circuits (100+ qubits) on devices with complex coupling maps, like IBM® Heron processors. SABRE minimizes SWAP gates and reduces circuit depth, improving execution fidelity.
> 
> The _Layout_ stage selects the hardware qubits to be used, while the _Routing_ stage inserts the appropriate amount of SWAP gates in order to execute the circuits using the selected layout.