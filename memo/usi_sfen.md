# **Freeze将棋拡張 案**

* 2020-11-29：初版
* 2020-12-02：改定
* 2020-12-22：改定
* 2021-02-02：改定
* 2021-02-16：改定
* 2021-03-03：改定

---

* [**Freeze将棋拡張 案**](#freeze将棋拡張-案)
  * [基本 ;^^](#基本-)
  * [sfen拡張](#sfen拡張)
    * [フリーズ残り回数指定 `zxxZxx`](#フリーズ残り回数指定-zxxzxx)
    * [フリーズ指定 `zy Zy`](#フリーズ指定-zy-zy)
  * [USIコマンド拡張](#usiコマンド拡張)
    * [各コマンド](#各コマンド)
      * [`FREEZE`](#freeze)
        * [`FREEZE` コマンド 詳細](#freeze-コマンド-詳細)
      * [`FREEZENUM`](#freezenum)
        * [`FREEZENUM` コマンド 詳細](#freezenum-コマンド-詳細)
  * [指し手表現](#指し手表現)
    * [フリーズ残り回数 `zxxZxx`](#フリーズ残り回数-zxxzxx)
  * [駒 移動](#駒-移動)
    * [info で表示するか bestmoveで示すか pv等で示すか](#info-で表示するか-bestmoveで示すか-pv等で示すか)
      * [info](#info)
      * [bestmove](#bestmove)

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
  * xxが無ければ0回 (zxのみ  Zxのみ も有効 。但し、xxが 0 もOK とします)
    * ↑ 保留中；^^
  * 全く無ければ 先後共に0回。 つまり、考慮しない(本将棋)
* 例：lnsgkg…sSNL b - 1  z3Z3
  * コノ局面で 先手２回、先手３回

### フリーズ指定 `zy Zy`

* フリーズした `zy`
* yは駒種 :歩 と と金、 飛車と龍等の区別あり
* l例：nsgkg……sSNL b - 1 zG z2Z3
  * コノ局面で、フリーズ金 先手２回、先手３回
* 全く無ければ考慮しない(本将棋)

* フリーズされた `Zy`
* yは駒種 :歩 と と、 飛車と龍等の区別のあり
  * 例：lnsgkg……sSNL w - 1 ZG z3Z2
    * コノ局面は、フリーズ金されていて 先手3回先手2回
* 全く無ければ考慮しない(本将棋)

---

## USIコマンド拡張

追加コマンド

* AI から GUI へ `FREEZE [ALLOW_FREEZE | CAN_FREEZE | OK | NONE]`
* GUI から AI へ `FREEZENUM [zNZN]`

### 各コマンド

#### `FREEZE`

オプション

* `NONE`
  * フリーズ将棋 を受け入れない
* `ALLOW_FREEZE`
  * AIは、フリーズ将棋の指し手を受け入れる (が、AIからはフリーズの指し手に含めない)
* `CAN_FREEZE`
  * AIは、フリーズ将棋の指し手を指す (が、相手からのフリーズの指し手を受け入れない)
* `OK`
  * `ALLOW_FREEZE` かつ `CAN_FREEZE` (フリーズ将棋を受け入れ、かつ、フリーズ将棋の指し手を指す)

##### `FREEZE` コマンド 詳細

option コマンドの応答内へ含める意図です。
当然ですが、AIからの応答に含まれていなければ Freeze将棋 には未対応です;^^

`ALLOW_FREEZE`、`CAN_FREEZE`、`NONE` は デバグ目的でもあります。

また、AIが、指定された評価関数や定石等を読み込んだ時に、Freeze将棋に対応していないとの判断時に返答するようにとの
拡張の意図もあったりなかったり;^^

#### `FREEZENUM`

オプション

* `zNZM`
  * N は後手のフリーズ回数。M は先手のフリーズ回数
  * Nのみ  Mのみ も有効 。但し、MやMが 0 もOK とします
    * ↑ 保留中；^^
  * `FREEZENUM` を送信していなければ 先後共に0回 (本将棋)

##### `FREEZENUM` コマンド 詳細

GUIがready 受信後にnewgame を送る前 に送信する意図です。

Freeze将棋 上は3回がルールだけど任意回と致します(例えば、下手0回、上手10回とか；^^)。

回数指定が省略されて無い場合(`FREEZENUM` 単体で送信)は、3回づつとします

上記、FREEZE 応答が無くて送信しても、Freeze将棋対応で無いAIの時は単に無視されるだけと思う;^^
(また、GUIでFreeze将棋に細かく対応していても、下記、指し手でエラーになると思う)

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
  * xxが無ければ0回 (zxのみ  Zxのみ も有効 。但し、xxが 0 もOK とします)
    * ↑ 保留中；^^
  * 全く無ければ 先後共に0回。 つまり、考慮しない(本将棋)
  * 例
    * position startpos z3Z3
    * position startpos moves xx ・・・ z3Z3

## 駒 移動

基本形

* `7g7f`

例

* 7g7fzG
  * 7g7f 移動で金をフリーズ
* 7g7fZG
  * 金がフリーズの局面で、指し手7g7f
    * (オプション 要るかなぁ;^^)

### info で表示するか bestmoveで示すか pv等で示すか

#### info

* info depth 1 seldepth 1 score cp 6055 nodes 137 nps 10538 time 13 pv ・・・5c6czG
  * 読みで 5c6c移動で金をフリーズ
* info depth 1 seldepth 1 score cp 6055 nodes 137 nps 10538 time 13 pv ・・・ 5c6czG・・・
  * 読みでの途中で 5c6c移動で金をフリーズ
* info depth 1 seldepth 1 score cp 6055 nodes 137 nps 10538 time 13 pv ・・・ 5c6cZG・・・
  * 読みでの途中で 金がフリーズの局面で思考し 5c6c移動
    * (オプション 要るかなぁ;^^)

#### bestmove

* bestmove 5c6czG
  * 金をフリーズ で 5c6c移動
* bestmove 5c6czG ponder xxxx
  * 金をフリーズ で 5c6c移動 で XXXを先読み
* bestmove 5c6c ponder xxxxzG
  * 5c6c移動 で 先読みで xxxx移動で 金をフリーズ
* bestmove 5c6cZG
  * 金がフリーズの局面で、指し手 5c6c移動
    * (オプション 要るかなぁ;^^)
