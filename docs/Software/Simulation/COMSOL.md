
## 参考資料
---
- Tracking Eigen modes in Parameter Sweeps : https://www.comsol.jp/support/learning-center/article/tracking-eigenmodes-in-parameter-sweeps-89591
- 開いた構造の eigen mode simulation をするときにどうすれば良いか : https://www.comsol.com/blogs/mode-analysis-for-electromagnetic-waveguides-in-comsol

## Wave port の作成
---
- 円形導波菅で wave port を作成する例 : https://www.comsol.com/model/download/1094901/models.rf.polarized_circular_ports.pdf
- Physics -> Port -> Port を作成した boundary を選択
- 作成した Port を選択した状態で Attributes -> Circular Port Reference Axis で 2 点選択して、極性を選ぶ

## S-parameter の評価
---
- S21 の評価を行う際には、Lumped port を定義して片方の "Wave excitation at this port" を ON にして Adaptive frequency sweep を走らせることで S11 と S21 を確認することができる
- S22 なども評価したい場合、`Electromagnetic Waves, Frequency Domain` の port sweep setting から `Use manual port sweep` にチェックを入れて、


## 同軸キャビティのような開放系で Eigenfrequency analysis などを行いたい場合
---
- 全体を Block で囲んで中身を air にする
- そのままだと本来境界がないところも PEC として定義されてしまうため、Matched Boundary Condition として定義することで、モードが立たないようにする
- https://www.comsol.com/blogs/using-perfectly-matched-layers-and-scattering-boundary-conditions-for-wave-electromagnetics-problems

## GDS design を COMSOL に読み込みたい場合
---
- <u>Qiskit-metal 用に GDS で metal & pocket design を作成している前提</u>
- klayout で GDS を開き、Save as で DXF 形式で保存
- AutoCAD で DXF file を開く。command prompt に CONVTOSURFACE と打ち込み、全ての object を選択し、polyline -> surface に変換する
- COMSOL で DXF file を Import (このとき unit が inch になっているので、μm に変換できるように scale する)
- Ground 用に workplane -> rectangle を作成し、pocket に対応する design を subtract する

## Geometry に色をつける方法
---




## Domain ごとに mesh の大きさを変えたい場合
---
- Geometry の中に JJ などの細かい構造があるときに、default の mesh を切ると全体が細かくなってしまう
- そこで user defined mesh を作成することで、どこを細かく切りたいかを指定することができる
- https://www.comsol.com/blogs/best-practices-for-meshing-domains-with-different-size-settings




## Trouble shooting
---
- Kill したい場合
	- Windows Power Shell を開いて ```taskkill /F /im comsol.exe```
- Mesh が切れない(Error が出る)
	- Model の中に非常に小さい構造がある
	- Error が出たあとの Mesh の構造を確認した時に、非常に細かい Mesh が設定されていないかチェック
		- 例 : Launch pad と Lumped port 用の sheet の大きさが少しずれていないか
- 複数の component があるときに simulation が進まない
	- simulation をしようとしている component とは別の component からエラーが出ている？
- Simulation が終わらない : https://www.comsol.jp/support/knowledgebase/1260
	- Progress を確認して Convergence がどうなっているかを確認する(<u>Simulation を走らせてる際には、convergence plot が作成されるため、iteration ごとに error が減っているかどうかも確認する</u>)
		- うまく converge できている場合は以下のような plot になる
		- ![[Screenshot 2026-04-15 at 17.25.44.png]]
	- Convergence が設定した値 (0.01 ?) に達していない場合![[Screenshot 2026-01-22 at 12.05.46.png]]
	- 解決方法
		- Solver が変わっている可能性がある
			- Solution に星マークが付いているときは変わっていることを示しているので、右クリックから "Reset Solver to Default" を選択してリセットする
			- ![[Pasted image 20260122121909.png]]
		- Mesh size が荒すぎる可能性があるため、Geometry の変更や Mesh size の調整で対応
		- Default の solver でダメな場合、他の solver を試してみる
			- [COMSOL で使用可能な solver の list](https://doc.comsol.com/6.3/doc/com.comsol.help.comsol/comsol_ref_solver.36.146.html)
			- Default では BiCGStab になっているはずなので、GMRES, FGMRES などを試してみる
		- Mesh を作り直したらうまく行ったことがある？