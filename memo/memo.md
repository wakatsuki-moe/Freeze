# 変更方針☆大前提

***※ 既存から逸脱が最小限になる様に※***

## AI

* YaneuraOu をベースにする
  * USER_ENGINE 使えない><
    * しょがなく、ゴリゴリ書いたw
      * うさぴょん 様を参考に(lessaKai)
        * USI拡張してコマンドラインではできるように\^^
  * コンパイルはできるようになた
  * でも、さて…どうしようw;^^
  * YaneuraOu コード内に「freeze」が既にあるので、使用しない;^^
    * zFreeze とか？w
* フリーズで無ければ本将棋で思考する…様に…したい;^^

* 出来うる限り現状の評価函数/定跡 から派生させる

  * 基本、Freeze将棋はパッチ風に追加する
* 評価函数は既存ベース？
  * 何にを基本に？
    * 理解出来るもので;^^
    * 動作出来るもので;^^
    * KKPT等(キメラ可)
    * NNUE(キメラ不可)
      * ※無しも視野に;^^
* 定跡は既存ベース？
  * 何を基本に？

    * 理解出来るもので;^^
    * 動作出来るもので;^^
      * ※無しも視野に;^^

## GUI

* Myshogi をベースにする

  * GUIとで統合で公開していただいているから
  * C#かぁ・・・w
  * フリーズの有無はオプションで指定?

## AI関連

### sfen USI プロトコル

* ※将棋所 と Shogigui は、そもそもFreeze将棋に対応できないのだから無視;^^
  * うん

* 盤面送信 と 指し手送信 の両方 が必要？
  * 盤面にはフリーズ済みの意が必要？
    * 先後手の残フリーズ回数の送信が要る？
  * 「F or f」は既にsfenである(6筋)ので　「Z or z」にしようかなぁ
    * Zとｚにする！^^
  * フリーズはその局面でなく、次の局面で効力発揮。う～ん><
  * でも、指し手としてはその局面。う～ん><
  * ※!!どうあっても 標準からは外れる!!※

    * 指し手に付随なので指し手側にする?
* 千日手と連続王手の千日手
  * 綾様の御返事で気にしなくて良くなった
  * 打ち歩詰め
    * どしよかｗ><

## GUI関連

* 棋譜入力

  * ※ 将棋神だと、そもそもコメント編集機能が無い
  * (2020-10-22)両方許す。でないと、将棋所とshougiGUIでの入力が読めない;^^

    * 指し手に　11金(Z歩)　と コメントの　Z歩　の両方を許す?
    * 指し手に　(Z歩)11金　と コメントの　Z歩　の両方を許す(フリーズ済み　の意)?
  * 指し手に　11金(Z歩) と (Z歩)11金 の両方表記。でないとフリーズした指し手での分岐で困りそう;^^

    * 指し手に　11金(Z歩)　と コメントの　Z歩　の両方を許す?
    * 指し手に　(Z歩)11金　と コメントの　Z歩　の両方を許す(フリーズ済み　の意)?
  * 盤面保存のときに困りそう;^^

    * 先後手の残フリーズ回数が要る？
* 棋譜表示

  * ※[将棋神 だと、そもそもコメントは読み込むが表示していない]
  * (2020-10-22)指し手に　11金(Z歩) と (Z歩)11金 の両方表記。でないとフリーズした指し手での分岐で困りそう;^^

    * 指し手に　11金(Z歩)　と コメントの　Z歩　の両方を許す?
    * 指し手に　(Z歩)11金　と コメントの　Z歩　の両方を許す(フリーズ済み　の意)?
  * 盤面保存のときに困りそう;^^
  * 先後手の残フリーズ回数が要る？
* 棋譜出力

  * ※[将棋神 だと、コメントは保存時に書き込みをしていない><]
  * ※「つまり読み込んで編集して保存すると消える！]

    * どうしよかｗ

----

## 番外編

*

----

## 保留/その他;^^

* フリーズ詰め将棋
* 評価函数
* 定跡