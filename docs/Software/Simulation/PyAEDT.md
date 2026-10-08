
## 参考資料
---
- PyAEDT Client Library Cheat Sheet : https://developer.ansys.com/blog/pyaedt-client-library-cheat-sheet
- Setup を作る際に設定可能な dictionary まとめ : https://github.com/ansys/pyaedt/blob/2e2f96c9e9437d20e7a5fa8d36c079fb8fc16b0f/src/ansys/aedt/core/modules/setup_templates.py#L121
- PostProcessing のexample script : https://github.com/ansys/pyaedt/blob/2e2f96c9e9437d20e7a5fa8d36c079fb8fc16b0f/tests/system/visualization/test_12_1_PostProcessing.py#L102

## Object
---
### 特定の面(face)を取得して、wave port を割り当てたい

```python
# faces = port_in_object.faces
# port_in_face = max(faces, key=lambda f: f.center[1])
port_in_face = port_in_object.top_face_y

hfss.wave_port(assignment=port_in_face, name = "Port_in", modes = 1)
```


## Eigenmode
---
### Solved Frequency の取得
```python
expressions=["Mode(1)","Mode(2)","Mode(3)"]
report = hfss.post.reports_by_category.eigenmode(expressions=expressions)
solution_data = report.get_solution_data()
solution_data.export_data_to_csv(output="test.csv")
for mode in expressions:
	print(mode)
	freq = solution_data.data_real(mode)[0]
	Q = abs(0.5 * freq / solution_data.data_imag(mode)[0])
	print("Frequency : ",freq*1e-9, " [GHz]")
	print("Q-value   : ",Q)
```
expressions がわからない場合は
```python
report = hfss.post.reports_by_category.eigenmode()
solution_data = report.get_solution_data()
print( solution_data.expressions )
```

## Report
---
- Eigen mode の固有振動数や Driven Modal による S-parameter の結果を取得する方法

```python
report_config = dict(
	#expressions=["db(S12)"], 
	expressions=["db(S(Port_out,Port_in))","db(S(Port_out1,Port_in))"], 
	plot_name="S-parameter", 
	variations={
		"Freq": ["All"],
	}
)

# 例えば chip_inductance を変更しながら、parametric sweep を行った場合
report_config["variations"]["$chip_inductance"] = ["All"]
report = hfss.post.create_report( **report_config )

## Solution data の取得と出力
solution_data = report.get_solution_data()
solution_data.export_data_to_csv(output="CoaxCavity_Modal.csv")

# Get frequency list
freq_list = [ i.value for i in solution_data.variation_values(variation="Freq") ]

# 色々な sweep での S21 を取得したい場合
mode_S_array = np.zeros(shape=(len(solution_data.variations), len(freq_list)))
for id in range(len(solution_data.variations)):
	solution_data.set_active_variation(id)
	mode_S_array[id] = solution_data.data_real()

plt.figure(figsize=(10,5))
plt.subplot(111, xlabel = "Frequency [GHz]", ylabel = "S21")
for id in range(len(solution_data.variations)):
	plt.plot(freq_list, mode_S_array[id], marker = ".")
```

## Common
---

### Optimetrics の option (Save Fields and Mesh) を ON にする方法
- https://github.com/ansys/pyaedt/discussions/4663
```python
hfss.set_oo_property_value(aedt_object=hfss.ooptimetrics, object_name="Parametric _Setup_Name", prop_name='SaveFields', value='True')
```

さらに他の option を追加したい場合、以下のようにして property を調べることが可能
```python
sweep.available_properties

['IsEnabled',
 'ProdOptiSetupDataV2',
 'ProdOptiSetupDataV2/SaveFields',
 'ProdOptiSetupDataV2/CopyMesh',
 'ProdOptiSetupDataV2/SolveWithCopiedMeshOnly',
 'StartingPoint',
 'Sim. Setups',
 'Sweeps',
 'Sweeps/SweepDefinition',
 'Sweep Operations',
 'Goals']
```

> [!warning]
ただし、SaveFields 以外の変数がなぜか set_oo_property_value で書き換えられない...
 一応以下のように直接書き換えることはできる。
 さらに CopyMesh を書き換える場合、SaveFields も dictionary から直接書き換える必要があるらしい
> ```python
>sweep.props["ProdOptiSetupDataV2"]["SaveFields"] = True
 >sweep.props["ProdOptiSetupDataV2"]["CopyMesh"] = True
>    ``` 


### Sweep の追加
- Sweep で追加可能な sweep type について : https://pyepr-docs.readthedocs.io/en/latest/_modules/pyEPR/ansys.html


### Object の property の変更

```python
vacuum_object.name = "boundary"
vacuum_object = hfss.modeler.get_object_from_name("boundary")
hfss.assign_material("boundary", "vacuum")
hfss.modeler.move_face([vacuum_object.top_face_z], offset=10) # offset in mm
```

### Object の Clone

```python
vacuum_object = box_object.clone()
```

### HFSS で表示される E-field を調整する方法
https://aedt.docs.pyansys.com/version/stable/API/_autosummary/ansys.aedt.core.hfss.Hfss.edit_sources.html
```python
sources = {"1": "0", "2": "1", "3": "0"}
hfss.edit_sources(sources, eigenmode_stored_energy=True)
```

### Plot の scale の変更
```python
## min & max が欲しい plot を表示する
plane_plot = hfss.post.create_fieldplot_cutplane("Global:YZ", "Mag_E", plot_name = "plane_Mag")
sources = {"1": "0", "2": "1", "3": "0"}
hfss.edit_sources(sources, eigenmode_stored_energy=True)

min_value = hfss.post.get_scalar_field_value("Mag_E","Minimum")
max_value = hfss.post.get_scalar_field_value("Mag_E","Maximum")
print(max_value, min_value)

plane_plot.change_plot_scale(maximum_value=max_value, minimum_value=1, is_log=True)
## 以下の方法でも同様に plot scale を変更することができる
# hfss.post.change_field_plot_scale(plot_name="plane_Mag1", maximum_value=max_value, minimum_value=1, is_log=True )
```

## Trouble Shooting
---
### Sheet のMesh が切れない
- 以下のように sheet に対して mesh を細かく切りたい場合、inside_selection option を False にしないと object の内部の mesh を細かく切ろうとしてしまう
```python
hfss.mesh.assign_length_mesh(["cap1", "cap2"], inside_selection=False, maximum_length="20um", name="mesh_cap")
hfss.mesh.assign_length_mesh(["port_out"], inside_selection=False, maximum_length="5um", name="mesh_JJ") # maximum 7um for JJ in qiskit-metal
```