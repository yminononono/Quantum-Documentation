# spread sheet

## Apps Script

複数のプルダウンの状態に応じて、セルの色やテキストを変えたい場合には ```Apps Script``` で関数を書くとやりやすい。(背景色の変更だけなら条件付き書式で簡単に設定できる)

![](<Screenshot 2024-12-11 at 11.06.26.png>){ width="80%" }

```Apps Script``` を開き、以下のような関数を生成すると、spread sheet を開いたときにプルダウンの状態から 2 列目のセルの色やテキストを変更する。

```cpp
function onOpen(e) {

    var startRow = 10; // 処理を開始する行

    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

    // 処理を行う列の範囲
    var endRow = sheet.getLastRow(); // 最終行
    var col_Usage = 3; // C列 (条件の基準)
    var col_Photo = 12; // L列
    var col_EB = 14; // N列
    var col_Shadow = 16; // P列
    var col_Result_start = 2; // R列 (結果を表示する列)

    var green = "#B7E1CD";
    var yellow = "#FCE8B2";
    var red = "#F4C7C3";



    for (var row = startRow; row <= endRow; row++) {
      var col_Result_end = 2; // R列 (結果を表示する列)
      var value_Usage  = sheet.getRange(row, col_Usage).getValue();
      var value_Photo  = sheet.getRange(row, col_Photo).getValue();
      var value_EB     = sheet.getRange(row, col_EB).getValue();
      var value_Shadow = sheet.getRange(row, col_Shadow).getValue();

      var result = ""; // 結果を格納する変数
      var color = ""; // 背景色

      // 条件を評価
      // Tc測定
      if ( value_Usage === "Tc測定" ){

        if( value_Photo == "" ){
          result = "";
          color = "";
          col_Result_end = 30;
        }
        else if( value_Photo !== "-"
        ) {
          result = "完了";
          color = green; // 緑色
          col_Result_end = 13;
        } else {
          result = "ベタ膜";
          color = red; // 赤色
          col_Result_end = 11;
        }
      }
      else if (value_Usage === "未定"){
          result = "ベタ膜";
          color = red; // 赤色 
          col_Result_end = 11;     
      }
      else {
          result = "";
          color = "";
          col_Result_end = 20;  
      }

      //Logger.log(col_Result_end);
      // 結果を R列に書き込む
      for(var col = col_Result_start; col <= 30; col++){
        var resultCell = sheet.getRange(row, col);
        // reset color
        resultCell.setBackground("");
        if(col == col_Result_start) resultCell.setValue(result);
        if(col <= col_Result_end)   resultCell.setBackground(color);
      }
    }

}

```

```Apps Script``` には関数名で指定できるトリガー関数が用意されている。

|    関数名    |                               トリガー条件                               |
| ------------ | ------------------------------------------------------------------------ |
| onOpen       | スプレッドシートを開いた時                                               |
| onEdit       | セルの値を入力や変更の編集が行われたとき                                 |
| onChange     | セルの編集に加えて、レイアウトの変更（行の追加や削除など）が行われたとき |
| onFormSubmit | スプレッドシートに紐づけられているフォームがユーザーによって送信された時 |

以上の関数名を指定しない場合には、自分でトリガー条件を設定する必要がある。
```トリガー``` -> ```トリガーを追加``` でトリガーしたい関数名を指定し、条件をプルダウンから指定する。

!!!warning
    onEdit を使用すると、undo がうまく機能しなくなるので、onOpen の方がおすすめ。
    おそらく undo を行うと、それ自体が編集になるため、再び関数がトリガーされてしまう。


## 連動するプルダウンの作成

あるプルダウンの選択肢に応じて、他のプルダウンの選択肢も変更したい場合がある。
やり方は [ここの説明](https://gosuke-blog.com/other-sheet-pull-down-list/#index_id0) が非常に参考になった。