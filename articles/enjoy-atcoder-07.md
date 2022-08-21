---
title: "中学生にAtCoderでプログラミングを教える記録 7回目 〜 ABC081A - Placing Marbles を for文を使って解く"
emoji: "🐣"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["AtCoder", "Python", "プログラミング教育", "中学生プログラミング教育"]
published: true
---
# この記事の内容
ソフトウェアエンジニアが中学生の子にプログラミングを教えていく様子を記録する。この記事は**7回目**。一度提出した問題を、`for`を使って解いてみる。

[**【前回の記事】**](https://zenn.dev/aromarious/articles/enjoy-atcoder-06)
@[card](https://zenn.dev/aromarious/articles/enjoy-atcoder-06)
配列（リスト）と`for`文のお勉強回。`for`文はこんな風に書きます、というところまでやった。
```python
list = [0, 1, 2]
for value in list:
  print(value)
```
```:結果
0
1
2
```

# 今回の達成目標
- `for`文を使って [(ABC081A - Placing Marbles](https://atcoder.jp/contests/abc081/tasks/abc081_a)をもう一度解く。

# Placing Marbles 問題概要
[ABC081A - Placing Marbles](https://atcoder.jp/contests/abc081/tasks/abc081_a) の問題を再掲する。

> 3つのマスにビー玉が置かれている。その様子を次のように数字で表す。`0`はビー玉なし、`1`はビー玉あり。
> ```:入力例
> 010
> ```
> これが1行入力されるので、マスにビー玉が何個置かれているかを出力する。
> ```:出力例
> 1
> ```

# これを `for` 文で解いてみよう
前回のコードは配列を配列のまま取り扱って処理した。

```python:前回のコード
a, b, c = map(int,list(input()))
print(a+b+c)
```
実はこの書き方は`for`文より難しい。子はこれを理解して書いているわけではなく、私が`map(int, 配列)`を書いて「こう動きます」と教えたので、イディオムとして使っているにすぎない。これを`for`文で繰り返す方法で書き直す。

子「`list(input())`が1文字ずつの配列だったよね。ここからやればいいかな」

```python
list = list(input())
print(list)
```
```:結果
['0', '1', '0']
```
私「いいね。配列になってたら繰り返せるもんね」
子「じゃあ、まず繰り返して1個ずつ出力してみる」
```python
list = list(input())
print(list)
for value in list:
  print(value)
```
```:結果
['0', '1', '0']
0
1
0
```
私「いい感じじゃん」
子「これを1個ずつもってきて、たしていったら結果が出るよね？」

```python
list = list(input())
print(list)
for value in list:
  print(value)
  goukei = goukei + value
  print(goukei)
```
```:結果
例外が発生しました: NameError
name 'goukei' is not defined
```
子「ンン？」
私「いいねいいね！ すごい。書いてることは正解だよ。正解なんだけど、コンピュータって融通きかないんだよなあ〜ｗ」
子「え、何をおこられたの？ これは」
私「この子の言ってるのは、『`goukei`っていう変数使おうとしてるけど、変数を作る前に使おうとしてるじゃん、やめてよね』って言ってる」
子「なんで…代入したら作ったことになるんじゃないの？」
私「代入ってね、まずイコールの右側を計算して、その結果を代入するんだよ。この代入の式をそういう目で見たらどうかな？」
子「ああなるほど…確かに繰り返しの最初の回で、`goukei`に代入する前に、`goukei`を使ってるわ」

子「でもここで決まった数字を代入したら合計できないんだけど…」
私「`for`が始まる前に変数を用意してみたらいいんじゃないかな」
子「なるほど？」

```python
list = list(input())
print(list)
goukei = 0 # goukei変数を作っておく
for value in list:
  print(value)
  goukei = goukei + value
  print(goukei)
```
```:結果
例外が発生しました: TypeError
unsupported operand type(s) for +: 'int' and 'str'
```
子「もー、いちいち怒る。怒ってる内容はさっきと違うけど」
私「わかる。この人は怒りっぽい。たぶん友達いないんだよ。可哀想」
子「そういう問題じゃないと思う。友達いても怒るやつは怒る」
私「確かに。え…なんか…学校で苦労してる？ お疲れ様」
子「うん、まあね。そんでこの人は今度は何がイヤだったの？」
私「`'int'`は数、`'str'`は文字列のことで、『数と文字列は`+`でつなげません』って言うてる」
子「ああ…これ1文字の文字列の配列だから、`value`は1文字の文字列が入ってるんだ。『たし算したかったら、数に直してからにしてよね』ってことか」
私「そうみたい」
子「文字列を数に直すのどうするんだっけ。`map`で`int`使ったけど、あれを使うの？」
私「そうそう。あの時は『この関数使ってね』って感じで関数名を書いたけど、今回は直接呼び出すから、`int(文字列)`って書いたら、文字列を数に直せるよ」
子「やってみる」

```python
list = list(input())
print(list)
goukei = 0 # goukei変数を作っておく
for value in list:
  print(value)
  goukei = goukei + int(value)
  print(goukei)
```
子「こうすればいいのかなあ。動かしてみる」

```:結果
['0', '1', '0']
0
0
1
1
0
1
```
子「動いた！ 怒らなかった！」
私「怒られずに動くとホッとするよねｗ」
子「ちょっとデバッグ出力がわかりにくいな…何の数字か書くといいかな。それから、結果出力は本当は`for`終わった後に出力したほうがいいよね？」
私「おっしゃる通りです」
子「`for`終わったあとの処理は、インデントなしで書いたらいいのかな」
私「おっしゃる通りです」
```python
list = list(input())
print(list)
goukei = 0 # goukei変数を作っておく
for value in list:
  print('input': value)
  goukei = goukei + int(value)
  print('goukei:', goukei)
print(goukei)
```
```:結果
['0', '1', '0']
input 0
goukei 0
input 1
goukei 1
input 0
goukei 1
1
```
子「動いた！ 他の入力も試してみる」
（`101`, `111`, `000` などを試す）
子「動いた動いた。じゃあデバッグ出力をコメントアウトして、結果だけ出力させて、テストを通すね」
```python
list = list(input())
# print(list)
goukei = 0 # goukei変数を作っておく
for value in list:
#  print('input': value)
  goukei = goukei + int(value)
#  print('goukei:', goukei)
print(goukei)
```
```:oj test の結果
[SUCCESS] test success: 3 cases
```

子「これってさ、1マスにビー玉3ことか9こ置いてもよくない？」
私「やってみｗ」

```:入力
139
```
```:出力
13
```

私「1マスにビー玉どんだけ詰め込んどん笑」
子「マス増やしてもいいよね？」
私「やってやってｗ」

```:入力
99999999999
```
```:出力
99
```
子「ちゃんと計算してる笑」
私「ビー玉たくさんすぎん！？ 人の分のビー玉まで奪って詰め込んでそう」
子「じゃあ提出するわ」

![](/images/enjoy-atcoder-07/result_abc081a_another.png =250x)

子「いえーい」
私「やったねー」

**【次の記事】** 次何しようかなあ。A問題で`for`使って解けそうなやつを探してこよう。（予定）
<!-- [**【次の記事】**](https://zenn.dev/aromarious/articles/enjoy-atcoder-08) -->
<!-- @[card](https://zenn.dev/aromarious/articles/enjoy-atcoder-08) -->
