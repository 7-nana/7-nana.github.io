---
layout: post
comments: true
title: Mastodonをv2.5.0にするためv2.3.3からv2.4.0に上げたらエラー画面になった話
date: 2018-09-27 00:00:00
image: https://7-nana.github.io/images/img_mastodonv250-update001.png
categories: Mastodon
tags:
- Update
- Error
---

先日、私が運営しているMastodonインスタンス「[マストどす](https://mastodos.com/about)」を最新のv2.5.0にすべく、まずはv2.3.3からv2.4.0にアップデートしたところエラー画面（We’re sorry, but something went wrong on our end.）になり、クライアントアプリからは使えるのにWebブラウザからだと使えない、摩訶不思議な状態に陥りました。

※当インスタンスはさくらのクラウドのスタートアップスクリプトで作成したものです

## 結論からお話すると

（最終的にエラー画面すら出なくなり）502エラーになっている状態でv2.4.0からv2.4.1にアップデートしたら直りました。その後（v2.4.2とv2.4.3を飛ばして）v2.4.4→v2.5.0とアップデートすることができました。

<iframe src="https://taruntarun.net/@mayaeh/100763899665858403/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://taruntarun.net/embed.js" async="async"></script>

私はv2.4.4のリリースノートを確認せずに、「いまv2.3.3だから、v2.4.0から順にアップデートすれば安心だ」と思い込んで今回のトラブルに見舞われました。アップデートが追いつかないまま本家のバージョンがいくつか上がった場合は、 **リリースノートを最新から順に一読すべきだった** と反省しています。

今回のことで、沢山の方々から応援やアドバイスをいただきました。また、多くのブログ記事にも助けられました。ありがとうございます。

解決まで延べ3日と時間を要してしまいましたが、教わったことを忘れないためにも、ここに書き記しておきます。

## 解決までの道のり

### nginxのエラーログを確認

/var/log/nginx/error.log

> [error] 1014#0: *5302 connect() failed (111: Connection refused) while connecting to upstream, client: XXX.XXX.XXX.XXX, server: mastodos.com, request: "GET /api/v1/streaming/?stream=user&access_token=ホニャララ HTTP/1.1", upstream: "http://[::1]:4000/api/v1/streaming/?stream=user&access_token=ホニャララ", host: "mastodos.com"

### アプリからは使えていた

<iframe src="https://omochi.xyz/@mimoo/100764153597158158/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://omochi.xyz/embed.js" async="async"></script>

mstdn.jpのアカウントを使って状況をトゥートしていると、他の運営者の方から「アプリからなら使用できているみたい」と教えていただき、確認してみるとユーザーの皆さんは普段通りワイワイとやり取りを楽しまれていました。

ひとまず安心し、事情を報告できたものの、一体どこをどう対処すれば良いのかわかりません。v2.4.0のアップデートではPostgreSQLを9.6に、Rubyを2.5.1にアップデートする必要がありますが、終始エラーなどは出ていませんでした。v2.3.3時点で改造していた場所は無く、ヴァニラのままです。

どこに問題があるのか見当がつかないと、相談のしようもありません。自分なりに調べながら、多くの方々からも助言をいただきつつ、以下のことを試しました。

### ディスク全体の容量を確認

`# df -h`

### どのディレクトリに一番容量を使っているか確認

`# du -sh /*`

### nginx.confのproxy_passを書き換えてみる

proxy_pass http://localhost:4000;  
↓  
proxy_pass http://127.0.0.1:4000;  
※今は戻しています

### nginxを再起動

`# systemctl restart nginx`

### pumaの確認

`# ps ax`  
「puma: cluster worker」を見つける

### Mastodonのサービス群を再起動

`# systemctl restart mastodon-web`  
`# systemctl restart mastodon-sidekiq`  
`# systemctl restart mastodon-streaming`

### Mastodonのサービス群が起動しているか確認

`# systemctl status mastodon-web`  
`# systemctl status mastodon-sidekiq`  
`# systemctl status mastodon-streaming`

### Redisが起動しているか確認

`# systemctl status redis`

### Redisが停止していたので起動

`# systemctl start redis`  
`# systemctl enable redis`

### サーバを再起動

`# reboot`

### mastodon-webのログを確認する

`# journalctl -xf -u mastodon-web.service`  
「status=500」がないか確認。

恐らくここで見落としがあったんじゃないかと思っています。

### Ruby Gem を再インストール

`# bundle exec gem uninstall -aIx`  
`# bundle install`

### アセットをゼロから作り直す

/home/mastodon/live  
`$ RAILS_ENV=production bundle exec rails assets:clobber`  
`$ RAILS_ENV=production bundle exec rails assets:precomplie`

### Node.jsをv8.X系までアップデートする

Mastodon v2.4.0でこの作業は必要ないはずなんですが、node_modulesでエラーが出ていたので下記のページを参考にNode.jsをアップデートしました。  
[Mastodon地域インスタンス「箕面どん」をv.2.5.0にアップデートしました – tonetalk](https://tonetalk.to/?p=1562)

この時点でエラー画面すら出なくなり、502エラーになってしまいました。上記のコマンドを再度試したりもしましたが原因がわからず八方塞がりになり、思い切ってv2.4.1へのアップデートを試してみると、無事Webブラウザでアクセスできるようになりました。

![v2.5.0になったマストどす](https://7-nana.github.io/images/img_mastodonv250-update001.png)

## 参考記事

* [Releases · tootsuite/mastodon](https://github.com/tootsuite/mastodon/releases)
* [Mastodon instances](https://instances.social/mastodos.com)
* [ゼロからはじめるMastodon インスタンス運用編｜さくらのナレッジ](https://knowledge.sakura.ad.jp/8683/)
* [Mastodon 保守メモ - Qiita](https://qiita.com/kumasun/items/bf4997f181f893130041)
* [さくらクラウドのスクリプトで作ったmastodonのバックアップとDocker環境へのリストア - Qiita](https://qiita.com/ABK28/items/2128f15840223349b024)
* [「さくらのクラウド」スタートアップスクリプトによるMastodonを、v2.3.3からv2.4.0rc3にバージョンアップした時につまずいた話。｜西村 治久《ソーシャルな隠居》｜note](https://note.mu/west2538/n/n687a96031f04)
* [素人がMastodonインスタンス運用でハマったトラブルシューティング10選+α v1.2.2 → 中略 → v2.4.5 → v2.5.0rc1｜西村 治久《ソーシャルな隠居》｜note](https://note.mu/west2538/n/ne52c57340555)
* [Mastodon地域インスタンス「箕面どん」をv.2.4.0にアップデートしました – tonetalk](https://tonetalk.to/?p=1524)
* [インスタンスでの障害相談 - 設置・運用 - Mastodon日本語メタフォーラム](https://discourse.mstdn.jp/t/topic/156)
* [マストドンのシンプルなアップデート手順。主にさくらのクラウドのスタートアップスクリプトを利用してインストールした場合を想定しています。](https://gist.github.com/anon5r/bd96ee4127d6b66ad4150287f5a4ed99)

## 今回の笑ったで賞

<iframe src="https://mastodos.com/@mashigure/100767035854994092/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://mastodos.com/embed.js" async="async"></script>

<iframe src="https://don.inux39.me/@inux39/100769057036570362/embed" class="mastodon-embed" style="max-width: 100%; border: 0" width="400"></iframe><script src="https://don.inux39.me/embed.js" async="async"></script>
