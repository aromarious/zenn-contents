---
title: "Mac で locate の代替として mdfind を使うことにした話"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [mac, cli, spotlight, gnu, homebrew]
published: true
---
# この記事でわかること
Macデフォルトの`locate`コマンドが役に立たない場合、`mdfind`を使いましょう。

# 3行で
1. 新しいMacでデフォルトの`locate`コマンドを使おうとしたが、目的のファイルが表示されない。
2. GNU findutils をインストールしたが似たような症状。`updatedb`の整備をやりかけたが手間がかかる。
3. Macデフォルトの`mdfind`コマンドで目的が果たせることが判明。これを使うことにした。手間ナシ爆速。

# 記事内容
結論としては「`mdfind`コマンドで用が足ります」以上のことはありません。それさえわかればオッケーという人はここから読む必要はないです。

- `locate`コマンドの基本動作
- Macデフォルトの`locate`コマンド
- GNU findutils の `locate`コマンド
- `mdfind`は何がうれしいか

を書きます。

# 記事前提
- ハードウェア…MacbookPro 2021, Apple M1 Pro
- 自機保守基準…システムはできるだけデフォルトで使いたい（保守コストを上げたくない）

# `locate`コマンドの基本動作
`locate`コマンドはシステム内のファイルをファイル名で検索するコマンドです。

実行時にファイルシステムを全検索しているわけではなく、あらかじめ用意されたファイルシステム内の全ファイル名のデータベースを検索します。それによって高速な検索を可能にしています。

MacOSにも`locate`コマンドがあって、あのファイルどこにあったっけ、という時に使えます。

と、思ったのです。でもそうはいかなかった。

# Macデフォルトの`locate`コマンド
仕事をしていて、あのファイルどこだったかな、と`locate`で検索したのですが、うまく動きません。

- 検索結果が正常に返ってくる場合もある。システムのファイルは検索できているっぽい
- 外付けディスクにあるファイルも検索できている
- 自分のホームディレクトリにあるはずのファイルが検索結果に出てこない

このような症状で、データベースが一部欠けているようだということがわかります。何が欠けているか正確に調査特定するのは難しいですが、つまりはデータベース作成がうまくいっていないのでしょう。以下をチェックしました。

- データベース `/var/db/locate.database` のタイムスタンプ
- コマンド `/usr/libexec/locate.updatedb` の仕様
- データベースシステムバックグラウンドプロセスを制御する`launchctl`の設定、動作状況のチェック

正常に動いているようです。ネットを検索すると、似た症状が散見され、理由として挙がっていたのは

- データベースファイルの owner が `nobody` になっている。`nobody`権限で実行されていて、その権限で読めるものしかデータベースに入れられないのでは
- シンボリックリンクは辿らないことになっている。Macのエイリアスなら辿る

あたりでした。

データベース作成プロセスの起動設定はシステム領域になっており、そこに手を入れるのはできればやりたくありません。また、シンボリックリンクは気軽にあちこちで使っていて、それらを全てエイリアスに変更するのは現実的ではありません。別の方法を当たることにしました。

# GNU findutils の`locate`コマンド

いずれにせよ、新しいMacにはGNUのツールをインストールするのが常なんです。仕事でGNU系のコマンドに慣れているので、Macデフォルトのコマンドはオプションが違ったりして使いづらいんですよね。

GNU findutils をインストールして、そちらの `locate` を使うことにしました。

:::details この時にインストールしたコマンドライン系パッケージ一覧
- binutils
- coreutils
- diffutils
- findutils ← `locate`, `updatedb`, `find`コマンドなどが入っている
- gawk
- gnu-sed
- gnu-tar
- tree
:::

ですが、こちらもうまく動かない。データベースが欠ける。`find`コマンドでファイル名一覧を作っているようなのですが、`sudo`でroot権限でデータベースを作成しても全ファイルを持ってこないので、ファイルシステムをまたげないとか、シンボリックリンクを辿れないとか、そのあたりのようです。

シンボリックリンクを辿るオプションをつけて手元のコマンドラインで実行してみると、うまく作れた様子です。

ですが、このデータベースを`locate`コマンドで検索すると、ものすごく！遅かった。使い物にならないレベル。ファイル数が多いためと思われますが、これはよっぽどの時しか使わないだろうなというレスポンスタイムでした。`launchctl`の設定ファイルを準備する気にもなりませんでした。

# `mdfind`は何が嬉しいか

いろいろ調べているうちに、`mdfind`を使えばいいじゃん、という記述がありました。`mdfind`？ なにそれ？

> `mdfind`はSpotlight の CLI 

そんなものがあるのか。知らなかった。試しにいくつか検索してみたところ、データベースは正常に作れているようでちゃんと出てくる。しかも速い。**これを使います。決定。**

## Pros
- Spotlightは普段から使ってるんだから、データベースが1系統で済むならそれに越したことはない
- 圧倒的に速い
- すでに動いているのでシステムに手を入れる必要なし
- 設定したければシステム環境設定のGUIで可能、わかりやすい

## Cons
- `locate`とは違うコマンドなので、当然仕様が違う
- ふだんSpotlight検索をオフにしている人にはメリットはあまりない

`locate` の代替として使う場合は `-name <filename>`オプションを使うとよいでしょう。

```sh:ファイル名com.apple.locate.plistのファイルのありかを知りたい
$ mdfind -name com.apple.locate.plist
```

Spotlightと同じ仕様なので、オプションなしで起動するとファイルの内容までヒットします。そうしたい場合は `-name`オプションなしで。

## メタデータについて少し
ファイルの内容と言いましたが、正確には「**メタデータ**」を見ているようです（`mdfind`の`md`は meta data）。Finderで「情報を見る」で表示されたりするあれです。

`mdls`コマンドで表示できます。

```sh:ホームディレクトリのメタデータ
$ mdls ~
```

```sh:さっきのplistファイルのメタデータ
$ mdfind -name com.apple.locate.plist | xargs mdls
```

MacOSは普段からシステムバックグラウンドでファイルシステム内の全ファイルを監視しており、メタデータを収集してインデックスを作る作業をしています。収集したメタデータは各ファイルボリュームの直下に置かれるようです。今使っているシステムでは

- `/System/Volumes/Data/.Spotlight-V100` 内蔵ディスク
- `/Volumes/ExtremePro/.Spotlight-V100`  外付ディスク

こうなっています。（メタデータインデックス自身のメタデータはメタデータインデックスに入っておらず、`mdfind`では出てこない仕様のようです）

`mdfind`の引数にはメタデータクエリを書けます。`locate`からだいぶ遠くなってしまったので、参考リンクを挙げてこの記事を終わりにします。

- Spotlightはどう動いているか [How Does Spotlight Work?](https://developer.apple.com/library/archive/documentation/Carbon/Conceptual/MetadataIntro/Concepts/HowDoesItWork.html)
- ファイルメタデータクエリの構文 [File Metadata Query Expression Syntax](https://developer.apple.com/library/archive/documentation/Carbon/Conceptual/SpotlightQuery/Concepts/QueryFormat.html)
- Spotlightメタデータアトリビュート [Spotlight Metadata Attributes](https://developer.apple.com/library/archive/documentation/CoreServices/Reference/MetadataAttributesRef/Reference/CommonAttrs.html)

いずれもApple公式の開発者向けドキュメント（英文）です。