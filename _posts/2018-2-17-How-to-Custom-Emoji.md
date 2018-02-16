---
layout: post
comments: true
title: Mastodon用にオリジナルのカスタム絵文字を作る
date: 2018-02-17 00:00:00
image: https://7-nana.github.io/images/img_howtocustomemoji_00009.png
categories: Mastodon
tags:
- Photoshop
- Illustrator
---

分散SNS「[Mastodon](https://joinmastodon.org/ "ソーシャルネットワーキングを、あなたの手の中に - Mastodonプロジェクト")」では、バージョン2.0.0からカスタム絵文字が使えるようになりました（絵文字を追加できるのはインスタンス管理者のみ）。

<iframe src="https://mastodos.com/@7_nana/99535641093246226/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mastodos.com/embed.js" async="async"></script>

先日、久しぶりに私のMastodonインスタンス「[マストどす](https://mastodos.com/about "mastodos.com - マストどす")」にカスタム絵文字を追加していて、なんとなーく作業フローが固まってきたので、書いておこうと思います。

この作業には、PhotoshopとIllustratorを使用します。

## 絵文字にしたい画像を用意する

使用する素材は、写真・絵、どちらでも構いません。なるべく鮮明なもので、サイズは400px以上あれば十分だと思います。

Mastodonのカスタム絵文字は、他のインスタンスからインポートすることが可能です。そのため、自分で撮影したものか、[写真AC](https://www.photo-ac.com/ "写真素材なら「写真AC」無料（フリー）ダウンロードOK")など商用利用が可能でクレジット記載が不要な無料素材を使用すると安心です。

絵は、スキャナーがなければデジカメやスマートフォンで撮影したものでも問題ありません。小さく表示しても視認できるよう、なるべく単純化して描きましょう。塗り１色の絵文字を作る場合は、白い紙に黒1色で描いておくと良いです。

## Photoshopでの作業

用意した画像をPhotoshopで開きます。モチーフそのものが色鮮やかでない場合、Photoshopメニューバーの「イメージ」＜「色調補正」＜「色相・彩度」から彩度を20％〜30％上げたり、

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00002.png)

全体の色調が沈んでいたり青みや黄みが強い場合は、「イメージ」＜「色調補正」＜「レベル補正」から整えます。絵なら、白い紙の部分が真っ白に見えるようにしておくと良いでしょう。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00003.png)

白背景にモチーフがある場合は、ここで画像を別名保存し、Illustratorでの作業に移ります。しかし図のように、絵文字にしたくない部分があったり、白背景に白いモチーフがある場合は、必要ない部分をマスクします。

ペンツールで絵文字にしたい部分をパスでざっくり囲み、

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00004.png)

パスパネルに追加された「作業用パス」を選択した状態で、オプションメニュー「パスを保存」を選択し「OK」、再度オプションメニューから「クリッピングパス」を選択して平滑度に「1」を入力し「OK」をクリックします。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00005.png)

別名保存でPhotoshop EPSフォーマットを選択し、.epsファイルを作成します。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00006.png)

## Illustratorでの作業

200x200pxのアートボード（RGB）を新規作成し、Illustratorのメニューバー「ファイル」＜「配置」で先ほど作成した画像ファイルを選択します。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00007.png)

リンクファイルメニューにある「画像トレース」のドロップダウンから「写真（低解像度）」を選択し、続いて「拡張」をクリックします。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00008.png)

画像が編集可能なパスグループに変換されました。パスの形を変えたり、パスの色を変更したり、不要なパスを削除したりして整えます。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00009.png)

アートボードに収まるよう、縮小します。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00010.png)

下に矩形パスを置いて、塗りをMastodonのタイムラインの背景色（R:40 G:44 B:55）にし、

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00011.png)

表示倍率を下げて視認性を確認します。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00012.png)

問題なければ矩形パスを非表示にし、「ファイル」＜「書き出し」＜「Web用に保存」

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00013.png)

プリセットは「PNG-24」を選択、「透明部分」にチェックを入れて保存します。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00014.png)

## 自分のMastodonインスタンスに登録する

保存したPNG画像を、MastodonのAdmin用管理画面から登録し、

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00015.png)

ローカルの絵文字一覧を表示し、最終確認をして完了です。

![Alt属性の内容](https://7-nana.github.io/images/img_howtocustomemoji_00016.png)

## おわりに

カスタム絵文字は、Mastodonの自由度を示す上で最も分かりやすい機能だと思います。管理者でない方も、ご自身でカスタム絵文字を作成して、管理者にリクエストしてみても良いかもしれません。
