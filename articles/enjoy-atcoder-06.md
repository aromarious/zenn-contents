---
title: "中学生にAtCoderでプログラミングを教える記録 6回目 〜 Pythonのlist（配列）と for 文を教える"
emoji: "🐣"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["AtCoder", "Python", "プログラミング教育", "中学生プログラミング教育"]
published: true
---
# この記事の内容
ソフトウェアエンジニアが中学生の子にプログラミングを教えていく様子を記録する。この記事は**6回目**。一度提出した問題を、`for`を使って解いてみる。

[**【前回の記事】**](https://zenn.dev/aromarious/articles/enjoy-atcoder-05)
@[card](https://zenn.dev/aromarious/articles/enjoy-atcoder-05)
[過去問(ABC081A - Placing Marbles)](https://atcoder.jp/contests/abc081/tasks/abc081_a) を解いた回。`list`, `map`などを使う解き方。

# 今回の達成目標
- `Python`の配列（`list`）を理解する
- 配列を`for`文で処理する

B問題に進むにあたり、繰り返しによる全探索を知っておく必要がある。このあたりで`for`文の使い方を教えたい。

今まで、データ型や配列について明確に教えたわけではない。「こう書けばこう動く」もないまま文法を教えても、理解しづらいと思ったからだ。

これまで問題を解く中で、文字列、整数、配列（リスト）などを扱ってきた。私が

「空白で区切ってバラバラにして、順番に並べて配列にして、この変数に入れる」
「これは文字列だからたし算できないね。数に変換しよう」

などと言うのを聞いて、なんとなく

（並べたやつは配列って言うんだ。並べたものをまとめて1つの変数に入れられるんだ）
（文字の`'1'`と数の`1`は別なんだな）

と思っている。そのレベルの理解はある。

これを正面から説明し、リストの各要素を`for`文で順に処理する方法で前回と同じ問題を解く。

# 教材

このページを一緒に見ながら、リストについて一通り話をした。今から言うことは、書き方の細かいところを覚えたりしなくていい、こういうことができるんだな、というのを「へー」って見ててください。というスタンス。

https://utokyo-ipp.github.io/2/2-2.html

# 説明したこと一覧
目の前で動かしてみせながら、一通り教えた。覚えてなくていい。そういうことができるんだなあ、というのを見ているだけでいい。
- リストリテラルの書き方 `[0, 10, 20]`
- リストの要素は文字列でもよい `['apple', 'banana', 'cherry']`
- 数字と文字列が混じってもいい `[10, 'apple', 20, 'banana', 30]`
- 何も入ってないリストっていうのもある `[]`
- リストから1つ取り出すにはインデックスをつかってこう書く（最初の要素は`0`番目） `list[2]`
- 「スライス」といって範囲を取ってくることもできる `list[1:3]` 1番目〜3番目未満を取ってくる
- インデックスやスライスを使って代入することもできる `list[0] = 'A'`
- リストの要素にリストを入れることもできる `[1, 2, [3, 4, 5]]`
- リストの要素数を数える `len(list)`
- リスト中の最大値を取ってくる `max(list)`
- リスト中の最小値を取ってくる `min(list)`
- リストの要素を全部合計した結果 `sum(list)`
- 複数のリストを結合する `[0, 1, 2] + ['a', 'b', 'c'] → [0, 1, 2, 'a', 'b', 'c']`
- リストを繰り返して新しいリストを作る `[0, 1, 2] * 3 → [0, 1, 2, 0, 1, 2, 0, 1, 2]`
- リストは中身が別領域に置いてあって、変数にはその場所が書いてある
- リストの入ってる変数を他の変数に代入して、もとのリストをいじると、他の変数も変わったように見える（同じものを見ているから）
- 別のリストとして扱いたければ `list.copy()`で複製を取って、それを代入する
- リストにこの要素が入っているか確かめる `10 in list → 10がlist内にあればTrue`
- この要素がリストの何番目に入っているか取ってくる `numbers.index(10) →10がlist内の2要素目にあれば2が返ってくる`
- リストの要素を小さいのから準に並べる `numbers.sort()` リストの中身を直接並べ替える。関数の返り値は`None`
- 逆順に並べる `numbers.sort(reverse = True)` これも中身を並べ替える。返り値は同じく`None`
- このように、リストの中身を直接いじることを「破壊的操作」と言う
- リストの要素を並べ替えた結果を返す関数 `sorted(numbers)` もとのリストは変えず、並べ替えた配列を返してくれる
- 逆順に並べ替えたものを返す `sorted(numbers, reverse = True)`
- この2つはもとのリストを温存し、結果のリストは新しく生成する。「非破壊的」な関数。
- 要素を追加する時は `list.append(element)`を使う。 `list.append(10)` とすると `[1, 2, 3]→[1, 2, 3, 10]`になる
- リストを結合したい時は `list.extend(list2)`を使う。`list.extend([11, 12, 13])`とすると`[1, 2, 3]→[1, 2, 3, 11, 12, 13]`になる
- `list.append([11, 12, 13])`で配列を渡すと`[1, 2, 3]→[1, 2, 3, [11, 12, 13]]`となるので注意。
- 以前やった`list(文字列)`, `文字列.split()`の復習
- `for`文はこう書いて、このように動きます（次のセクション） 

# `for`文で処理する
私「今まで出てきた関数は、配列をブチ込んで、合計とかの結果を持ってくるやつだったじゃん？」
子「そうだね」
私「でもさ、配列にデータが入ってて、自分なりの処理がしたい時もあるじゃん」
子「ないなあ」
私「今んとこないだけで、そのうち処理したくなるんだから『そうだね』って言うところなんだよ」
子「『そうだね』」
私「棒読み」
子「わかったわかった。処理したい時、あります。ありますので話を進めてください」
私「処理したい時あるんだ。天才じゃない？」
子「いいから進めて」
私「はい。すみませんでした。たとえば数の配列があって、それぞれ2乗して全部足したい時とか出てくる」
子「そんな計算することある？」
私「普通にある。統計の話になるけど、エッ、その話していい？」
子「2乗して足したい時がありますという前提で進めてください」
私「ちぇ。そういう時にちょうどいい関数がないんで、自分でひとつひとつ処理する。`SwiftPlayground`でも繰り返しをやったと思うけど、`Python`にも同じのがあります」
子「ああそういえばそういうのあったね！」

```swift
var numbers = [1,2,3,4,5]
for num in numbers {
    print(num)
}
```

私「`Swift`ではこうだったよね。`Python`ではこうやって書きます」

```python
list = [0, 1, 2]
for value in list:
  print(value)
```

私「こう書くと、`list`の中にあるものを順番に1個ずつとりだして、`value`に入れて、処理するのを繰り返す」
子「`Swift`みたいに`{}`で囲まないんだ」
私「そうなんだよ！ 今その話するとこだった。お見通しだな」
子「囲まなかったらまとまりがわからなくない？」
私「そうなんだよ。困るよね」
子「始まりは`for`でいいかもしれないけど、終わりは？」
私「`Python`ではインデント（字下げ）で示すことになってるんだって。大雑把だよね」
子「ああこの右にズレてるとこは`for`の中身ですって意味になるんだ」
私「その通りです」

子「それで？ この`value`って何？」
私「この`value`は`for`の中だけで使う変数で、`list`から1こ取ってきて、`value`に入れて、中身の処理をやって、終わったら次の要素をまた`value`に入れるってことになってる。動かしてみよう」

```python
list = [0, 1, 2]
print(list)
for value in list:
  print(value)
```
```:結果
[0, 1, 2]
0
1
2
```

子「ああなるほど、順番に取ってきて繰り返すってそういうふうにやるのかー」
私「飲み込みが早い！ じゃあ昨日の問題を`for`文使って書き直してみようか」
子「やってみる」

[**【次の記事】**](https://zenn.dev/aromarious/articles/enjoy-atcoder-07) 今習った`for`文を使って、前回解いた過去問  [ABC081A - Placing Marbles](https://atcoder.jp/contests/abc081/tasks/abc081_a) をもう一度解く。
@[card](https://zenn.dev/aromarious/articles/enjoy-atcoder-07)
