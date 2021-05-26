# **Freeze将棋拡張 案**

* 2020-11-29：初版
* 2020-12-02：改定
* 2020-12-22：改定
* 2021-02-02：改定
* 2021-02-16：改定
* 2021-03-03：改定
* (2021-05-25：改定)
  * 「被フリーズ」のZxx を完全にオプションとしました
    * 但し、GUI(自己対局時含)の指し手をAI側で評価関数でソノ指し手は悪手では無い(被フリーズでの指し手)と判断できる可能性があるので(評価値での問題)削除は致しませんでした。
  * フリーズ回数の省略を無くしました
  * 「FREEZENUM」コマンドを保留と致しました
    * 詳細は下記参照を;^^
  * `FREEZE` コマンド に、オプション `IGNORE` を追加致しました

---

## 基本 ;^^

* Freeze将棋に関連する部分「以外」は極力本将棋に準ずるように努めます。
* 「F or f」は既にsfenである(6筋)ので　Freeze将棋関連は「Z or z」をで;^^
* 基本、最後に付加して、 無ければ考慮せずに本将棋で

---

## sfen拡張

基本形

  `lnsgkgsnl/1r5b1/ppppppppp/9/9/9/PPPPPPPPP/1B5R1/LNSGKGSNL b - 1`

### フリーズ残り回数指定 `zxxZxx`

* zxx は、は後手の残り回数、Zxx は先手の残り回数
  * zxxのみ  Zxxのみ は不可。0回は xx が 0 とします
  * 全く無ければ 先後共に0回。 つまり、考慮しない(本将棋)
* 例：lnsgkg…sSNL b - 1  z3Z3
  * コノ局面で 後手３回、先手３回

### フリーズ指定 `zy` `Zy`

* フリーズした `zy`
  * yは駒種 :歩 と と金、 飛車と龍等の区別あり(成り駒は'+'を付加)
    * 例：nsgkg……sSNL b - 1 zG z2Z3
      * コノ局面で、フリーズ金 先手３回、先手２回
        * つまり、AIは金での指し手はできず、フリーズ返しできない
  * 全く無ければ考慮しない(本将棋)

* フリーズされた `Zy` (オプション)
    * yは駒種 :歩 と と、 飛車と龍等の区別のあり(成り駒は'+'を付加)
      * 例：lnsgkg……sSNL w - 1 ZG z3Z2
        * コノ局面は、フリーズ金されていて 先手2回、後手2回
          * つまり、フリーズ金からの指し手で、AIはフリーズが可能
    * 全く無ければ考慮しない(本将棋)

---

## USIコマンド拡張

追加コマンド
* AI から GUI へ `FREEZE [ALLOW_FREEZE | CAN_FREEZE | OK | NONE | IGNORE]`

保留中
* ~~~~GUI から AI へ `FREEZENUM [zNZN]`~~~~)

### 各コマンド

#### `FREEZE`

オプション

* `NONE`
  * Freeze将棋 を一切受け入れない(デフォルト)
    * Freeze将棋の指し手は、AI側でエラーとしても良いとします
* `IGNORE`
  * AIは、Freeze将棋の指し手を受け入れるが、無視をする。フリーズの手も指さず、考慮もしない
    * フリーズは無視。普通に指し手を返す(当然、フリーズ打ち歩詰めは不可;^^)
* `ALLOW_FREEZE`
  * AIは、Freeze将棋の指し手を受け入れるが、AIからはフリーズの指し手を含めない
* `CAN_FREEZE`
  * AIは、Freeze将棋の指し手を指すが、相手からのフリーズの指し手を受け入れない
* `OK`
  * `ALLOW_FREEZE` かつ `CAN_FREEZE`。Freeze将棋を受け入れ、かつ、Freeze将棋の指し手を指す

##### `FREEZE` コマンド 詳細

option コマンドの応答内へ含める意図です。

当然ですが、AIからの応答に含まれていなければ Freeze将棋 には未対応です;^^
(`FREEZE` 応答がが無い場合、GUIは `NONE` にフォールバックするとします)

`ALLOW_FREEZE`、`CAN_FREEZE`、`IGNORE`、`NONE` は デバグ目的でもあります
(`NONE` と `IGNORE` は完全にデバグ用途です；^^)。

また、AIが、指定された評価関数や定石等を読み込んだ時に、Freeze将棋に対応していないとの判断時に返答するようにとの
拡張の意図もあったりなかったり;^^ 。

(思考中に評価関数を変更するようなAIは無いと思うのですが(私が知らないだけかもですが)、
Freeze将棋でない時にはFreeze将棋の指し手を探索しないのは意味ある動作と思うので
(Freeze将棋の指し手探索はけこ重いのです><)オプション機能として入れておきます;^^)

--

#### `FREEZENUM`

保留中。

通常のGUIとのやり取りでは下記の通りに毎回GUIから回数を送信するので、
自己対局時に最大回数を指定の意図に変更(usiとしては拡張コマンド)。
か、完全に削除の予定;^^。当面は機械学習は予定していないの><

<del>

* `zNZM`
  * N は後手のフリーズ回数。M は先手のフリーズ回数
  * zxxのみ  Zxxのみ は不可。0回は xx が 0 とします
  * `FREEZENUM` を送信していなければ 先後共に0回 (本将棋)

##### `FREEZENUM` コマンド 詳細

GUIがready 受信後にnewgame を送る前 に送信する意図です。

Freeze将棋 上は3回がルールだけど任意回と致します(例えば、下手0回、上手10回とか；^^)。

回数指定が省略されていて、無い場合(`FREEZENUM` 単体で送信)は、3回づつとします

上記、FREEZE 応答が無くて送信しても、Freeze将棋対応で無いAIの時は単に無視されるだけと思う;^^
(~~~~また、GUIでFreeze将棋に細かく対応していても、下記、指し手でエラーになると思う~~~~)

</del>

---

## 指し手表現

 基本形その１

* `position startpos`
* `position sfen xxxxxxxxxx`

 基本形その２

* `position startpos moves xx`
* `position sfen xxxxxxxxxx moves xx`

<!-- * 基本形その２ position sfen xxxxxxxxxx moves xx zG ・・・ z3Z3 -->

### フリーズ残り回数 `zxxZxx`

* zxx は、は後手の残り回数、Zxx は先手の残り回数
  * zxxのみ  Zxxのみ は不可。0回は xx が 0 とします
  * 全く無ければ 先後共に0回。 つまり、考慮しない(本将棋)
  * 例
    * position startpos z3Z3
    * position startpos moves xx ・・・ z0Z0

## 駒 移動

基本形

* `7g7f`

例

* `7g7fzG`
  * `7g7f` 移動で金をフリーズ
* `7g7fZG`
  * 金がフリーズの局面で、指し手 `7g7f`
    * (オプション)

成り駒の指定は '+' を付加
* `7g7fzB+`
  * `7g7f` 移動で馬をフリーズ
* `7g7fZB+`
  * 馬がフリーズの局面で、指し手 `7g7f`
    * (オプション)

### info で表示するか bestmoveで示すか pv等で示すか

#### info

* info depth 1 seldepth 1 score cp 6055 nodes 137 nps 10538 time 13 pv ・・・5c6czG
  * 読みで 5c6c移動で金をフリーズ
* info depth 1 seldepth 1 score cp 6055 nodes 137 nps 10538 time 13 pv ・・・ 5c6czG・・・
  * 読みでの途中で 5c6c移動で金をフリーズ
* info depth 1 seldepth 1 score cp 6055 nodes 137 nps 10538 time 13 pv ・・・ xxxzG 5c6cZG・・・
  * 読みでの途中で 金がフリーズの局面で思考し 5c6c移動
    * (オプション)

#### bestmove

* bestmove 5c6czG
  * 金をフリーズ で 5c6c移動
* bestmove 5c6czG ponder xxxx
  * 金をフリーズ で 5c6c移動 で XXXを先読み
* bestmove 5c6c ponder xxxxzG
  * 5c6c移動 で 先読みで xxxx移動で 金をフリーズ
* bestmove 5c6czG ponder xxxxzG
  * 金をフリーズ で 5c6c移動、先読みで xxxx移動で 金をフリーズ

---
* bestmove 5c6cZG
  * 金がフリーズの局面で、指し手 5c6c移動 (オプション)
* bestmove 5c6cZG ponder xxxx
  * 金がフリーズの局面で、指し手 5c6c移動 で XXXを先読み (オプション)
* bestmove 5c6cZG ponder xxxxzG
  * 金がフリーズの局面で、指し手 5c6c移動 先読みで xxxx移動で 金をフリーズ (オプション)

<del>

* bestmove 5c6czG ponder xxxxzG
  * 有り得ない(はず)。先読みフリーズ返し;^^

</del>
