
## Field Calculator
---
電場分布の空間積分などを行いたい時は Field Calculator を使用する。
様々な量の計算方法が[ここ](https://ansyshelp.ansys.com/public/Views/Secured/Electronics/v252/en/Subsystems/HFSS/Content/ReportsandPostProc/FieldsCalculatorRecipes.htm) にまとめられている。

### 起動方法
起動方法は、```Field Overlays``` -> ```Calculator``` で Field Calculator が表示される。
![[Screenshot 2026-09-10 at 13.26.25.png|498]]

### 例 : $E_x$ の空間積分

1. Input -> Quantity -> E を選択
2. Vector -> Scal? -> ScalarX を選択。($E_x$ に変換)
3. General -> Complex -> Real を選択。($Re[E_x]$ に変換)
4. Input -> Geometry -> Volume -> 空間積分する geometry を選択。
5. Scalar -> Integral を選択
6. ここで、Library -> Add を押しておくと、作成した量を変数として定義しておくことができる。
7. さらにあとで読み込めるように Library -> Save To で保存しておくこともできる。
8. Output -> Eval で作成した式の量を計算することができる。

![[Screenshot 2026-09-10 at 14.38.28.png|494]]


> [!note] Stack 内の表示について
>- **`Scl` (Scalar):** Single real number or scalar magnitude.
>- **`CSc` (Complex Scalar):** Scalar value that includes both real and imaginary components.
>- **`Vec` (Vector):** Real vector quantity with directional components (x, y, z).
>- **`CVc` (Complex Vector):** Complex vector quantity like electric or magnetic phasor fields


## Tips
---

### Object の表示を切り替えたい時

Object の表示を切り替えたいときは、```Draw``` -> ```Hide/Show overlaid visualization in the active view``` で object のメニューが表示されるので、右側のチェックボックスを押すと表示の切り替えが簡単にできる
![[Screenshot 2026-09-10 at 13.00.27.png]]
## Optimetrics
---
- Save Fields & Mesh を ON にしていた場合、variable ごとに field や mesh を確認することができる
- Optimetrics -> 特定の optimetrics -> 右クリック -> View Analysis Result で見たい変数に切り替えると、field plot などがアップデートされる

## Customer support
---
- サポートに対してお金を払っている場合には、おそらく[Ansys Customer Support Space](https://customer.ansys.com/)からサポートを受けられる
	- その際、customer number の入力が必要
- サポートを受けられない場合、[Ansys learning forum](https://innovationspace.ansys.com/forum/forums/forum/discuss-simulation/electronics-2/) に質問を投げると誰かが答えてくれるかもしれない