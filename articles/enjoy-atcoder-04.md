---
title: "中学生にAtCoderでプログラミングを教える記録 4回目 〜 初めての提出（後半） ABC086A - Product"
emoji: "🐣"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["AtCoder", "Python", "プログラミング教育", "中学生プログラミング教育"]
published: true
---
# この記事の内容
ソフトウェアエンジニアが中学生の子にプログラミングを教えていく様子を記録する。この記事は**4回目**。子本人が自分のマシンを操作し、初めて解答をひとつ提出する。前半では、文をひとつ書いては動かすことを覚えた。続いて、後半では設問に沿ったコードを書いて提出までを書く。

[**【前回の記事】**](https://zenn.dev/aromarious/articles/enjoy-atcoder-03)
前フリが長かったが、3回目、いよいよ自分で初めてコードを書きはじめる回。
@[card](https://zenn.dev/aromarious/articles/enjoy-atcoder-03)

# 3行で、と思ったけど1行で
1. スモールステップでひとつひとつ進め、提出までやる。

# 前回のあらすじ
 ABC 086 A - Product を題材に書き始めて、スモールステップを踏んできた。

https://atcoder.jp/contests/abc086/tasks/abc086_a

入力を取得するところから順に、ひとつ書いてはデバッグ実行して動作確認しながら作ることを旨とし、このように進めてきた。

1. まず問題をよく読む。
- 📌 **楽しくコードを書く方法**を伝授 ←大事
2. 入力を1行取得するコードを書き、実行してみよう。
3. 取得した入力に関係ない適当な文字列を出力するコードを追加して、実行してみよう。
4. 入力を1行取得して、取得したものを出力するように修正して、実行してみよう。

# 後半戦、問題に沿ってコードを書き、提出するまで
今回の記事では次のように進める。

5. 空白で区切って配列にするコードを書き、実行してみよう。  `a = input().split()`
6. 文字列の配列の各要素を整数に変換して代入する修正をほどこし、実行してみよう。 `a, b = map(int, input().split())`
7. 判定のための演算を試しながら作っていこう。
   1. `a`と`b`の積… `a*b`
   2. 偶数か奇数か判定するには2で割った余りを計算する… `a*b % 2` （2で割った余りを計算する）
   3. 余りが0かどうかで判定… `print(a*b % 2 == 0)`→True/False
8. これを使って `if` 文を書き、実行してみよう。
   1. `if a*b % 2 == 0: print('Even') else: print('Odd')`

## 5. 空白で区切って配列にするコードを書き、実行してみよう。 `a = input().split()`
私「入力はゲットしてきたけど、これは1つの長い文字列だから、掛け算はできないんだよね」
子「空白で区切って2つに分けて、数に直すんだよね。こないだ教えてくれたやつ」
私「メッチャ覚えるやん。覚えんでええ言うたのに」
子「いまおれ怒られてるの？ 覚えてたのに？」
私「言うたな。覚えてるんやったら書いてみ」
子「分けるのはこうでしょ。`a = input().split()` 」
私「ウソ…マジで覚えてる…」
子「ハイハイわかったわかった。動かすよ」
```python: solve_abc086_a.py
#!/usr/bin/env python
a = input().split()
print(a)
```
```:結果
["3", "4"]
```
私「素晴らしい！ ちなみにこれはどういうことになってるか教えてもらっていいですか」
子「急に敬語。入力された文字列を、空白で区切ってバラバラにして、配列にした」
私「もう教えることないから帰っていい？」
子「どこへ帰るつもりですか」

## 6. 文字列の配列の各要素を整数に変換して代入する修正をほどこし、実行してみよう。 `a, b = map(int, input().split())`
子「これを数にしないといけないんだよね。なんだっけ。あのアレを使うんだよ」
私「覚えてないこともあるんだ」
子「うるさいな。もう帰ってもらっていいですか」
私「`map`を使うやつね」
子「そうそうそれだよ」
```python: solve_abc086_a.py
#!/usr/bin/env python
a = map(int, input().split())
print(a)
```
```:結果
<map object at 0x104f2bee0>
```
子「あれ？」
私「説明はできんけど、`map`の結果は配列じゃないねん」
子「説明してよ笑」
私「大きくなったらね」
子「ごまかしたな。子供扱いしたな」
私「ごめんてｗ マジで今説明したら意味もなく難しなるだけやし後悔するで」
子「しゃあないな。でもこれは見覚えあるけど。どう使うんだっけ」
私「そのまま `a`と`b`に代入してみて」
子「ああ、こうだっけ」
```python: solve_abc086_a.py
#!/usr/bin/env python
a, b = map(int, input().split())
print(a, b)
```
```:結果
3 4
```
子「そうそうこれこれ」

## 7. 判定のための演算を試しながら作っていこう。
   1. **`a`と`b`の積… `a*b`**

私「これ掛け算して結果表示できる？」
子「掛け算て何使うの？」
私「`*`これ」
子「ポムポムプリンを思い出すなあ」
私「やめてあげて」
```python: solve_abc086_a.py
#!/usr/bin/env python
a, b = map(int, input().split())
print(a, b)
print(a*b)
```
```:結果
3 4
12
```
子「できた」
私「教えてないのにもう`print`デバッグみたいなことするやん」
子「動かして結果を確認するのが大事なんでしょ？」
私「仰せの通りでございます」

   2. **偶数か奇数か判定するには2で割った余りを計算する… `a*b % 2` （2で割った余りを計算する）**

子「偶数か奇数か判定してくれるコマンドとかないの？」
私「ないなあ。算数で偶数か奇数か判断してって言われたらどうする？」
子「2で割って割り切れるかどうか見る？」
私「だよね。商と余りとかやったよね。あれを計算してくれるやつがある。`//`と`%`。2で割った余りは`x % 2`で出る」
子「もう、今言おうと思ったのにー。書いていい？」
私「どうぞお書きください」
```python: solve_abc086_a.py
#!/usr/bin/env python
a, b = map(int, input().split())
print(a, b)
print(a*b)
print(a*b%2)
```
```:結果
3 4
12
0
```
子「割り切れた。ということは偶数」

## 8. これを使って `if` 文を書き、実行してみよう。
私「これを使って`if`文書けるかな」
子「だんだんスモールステップの幅が大きくなってきてない？」
```python: solve_abc086_a.py
#!/usr/bin/env python
a, b = map(int, input().split())
print(a, b)
print(a*b)
print(a*b%2)
if a*b % 2 == 0:
  print('Even')
else:
  print('Odd')
```
```:結果
3 4
12
0
Even
```
子「できた！ 奇数にしてみる」
```:結果
3 5
15
1
Odd
```
私「できたじゃーん。デバッグ出力が残ってるから消すといいよ」
子「わかった」
```python: solve_abc086_a.py
#!/usr/bin/env python
a, b = map(int, input().split())
if a*b % 2 == 0:
  print('Even')
else:
  print('Odd')
```
子「スッキリした。あの、サンプル入力を試せるやつをやってみたい」
私「ここでこれ押して `oj test` 選んで」

```:結果
(.venv) a% oj test -c ./solve_abc086_a.py -d ./tests   
[INFO] online-judge-tools 11.5.1 (+ online-judge-api-client 10.10.0)
[INFO] 2 cases found

[INFO] sample-1
[INFO] time: 0.017030 sec
[SUCCESS] AC

[INFO] sample-2
[INFO] time: 0.028136 sec
[SUCCESS] AC

[INFO] slowest: 0.028136 sec  (for sample-2)
[INFO] max memory: 6.784000 MB  (for sample-2)
[SUCCESS] test success: 2 cases
(.venv) a% 
```
子「できたー！」


私「これ提出してみる？」
子「する！！」
私「これ押して`acc submit`選んで」
子「おお…`AC`になった！」

![](/images/enjoy-atcoder-04/result_abc086a.png)

私「おめでとう！ 初めての提出、大成功じゃん！」
子「ヤッター！」

[**【次の記事】**](https://zenn.dev/aromarious/articles/enjoy-atcoder-05) 次のA問題、[ABC081A - Placing Marbles](https://atcoder.jp/contests/abs/tasks/abc081_a) を解く。
@[card](https://zenn.dev/aromarious/articles/enjoy-atcoder-05)
