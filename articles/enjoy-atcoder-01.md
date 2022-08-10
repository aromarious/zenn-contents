---
title: "AtCoderでプログラミングを教える記録 1回目 背景と環境概要"
emoji: "🐣"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["AtCoder", "Python", "プログラミング教育"]
published: false
---
# この記事の内容

ソフトウェアエンジニアが中学生の子にプログラミングを教えていく様子を記録する。この記事は1回目。これまでやってきたことと、今回AtCoderにチャレンジするための環境について概要を記す。

# 3行で
1. 10歳頃にScratchとSwiftPlayground、HumanResourceMachineでプログラミングに触れる
2. 現在13歳。自由に作るより、問題を解くほうが好きとのこと。AtCoderをやってみては、ということに。
3. MacBookAirにVSCode等の環境を構築した。

# 子が小学生時代に学んだこと
Scratchの猫やSwiftPlaygroundのバイト君を動かしながら、プログラミングの基礎概念を習得した。

コードを書いて、それを実行すると、コンピュータはそこに書かれた通りに順番に動作することを理解しており、変数、四則演算、条件分岐、繰り返し、関数呼び出し、関数定義などの基本概念を習得している。

SwitchのHumanResourceMachineというゲームを時々プレイしている。アセンブラのような言語体系と、使いづらいエディタを駆使して、複雑な処理を書いている。これを書けるのなら、今の普通の構造化言語は書けるに決まっていると思い、教えるタイミングを虎視眈々と狙っていた。

https://apps.apple.com/jp/app/human-resource-machine/id1005098334

私はZ80や370アセンブラ（IBMのメインフレームのアセンブラ）の経験があるが、このゲームでは子に勝てない。書きづらくて泣きが入る。もっと書きやすいモダンな言語で書かせてほしい。

# 子の特性と現在の状況
プログラミングを学ぶには作りたいものを作るのが一番速いとよく言われるが、子はそのような自由課題に戸惑いを見せる。課題を解いていくほうが好きなのだそうだ。

また、小学生時代は画面にあふれるアルファベットが読みづらく暗号のように思えて、なかなかやる気にならなかったが、中学生になって英語をやり始めたので読みやすくなってきたと言う。

ではこのタイミングでAtCoderに挑戦してみるか。夏休みを使ってやってみることになった。

# 環境構築の方針
AtCoderはブラウザだけでもチャレンジできるが、初心者が試行錯誤しながら学んでいくには手がかりが少なく学習しづらい。文法エラーくらいは実行前に指摘してもらいたいし、デバッグ実行もしたい。この際、`VSCode`でいろいろ試しながら理解を深められるよう、環境を作ることにした。

# 用意した環境
AtCoderやGitHubのアカウントの登録、操作に必要な基本コマンド（`homebrew`や`git`など）のインストールなどはいちいち書かない。
1. Python3.8 と仮想環境
2. `online-judge-tools` と `atcoder-cli`
3. `Visual Studio Code`

## 1. Python3.8 と仮想環境
`Python`か`JavaScript`かどちらがいいか選ばせたところ、「`Python`にしようかな」とのことで、インストールする。
- `python`のバージョン管理として`pyenv`をインストール
- `pyenv`を使って`3.8.13`をインストール<br>AtCoderでは`Python3`として`3.8.2`がし使用されているため、`3.8`系の最新を使うことにした。その他`3.10.5`もインストール。
- AtCoder用のフォルダを作り、そこで仮想環境を作っておく<br>`mkdir AtCoder; cd AtCoder; python -m venv .venv`

## 2. `online-judge-tools` と `atcoder-cli`
https://qiita.com/Adaachill/items/3d4ddad56c5c2cc372cd
この記事を参考にして、コマンドラインでAtCoderから問題やテスト入力などの取得、書いたコードの提出ができるようにした。
:::details 設定
```:atcoder-cli の設定
~/Library/Preferences/atcoder-cli-nodejs
├── config.json
├── python3/
│   ├── solve.py*
│   └── template.json
└── session.json
```
```json:config.json
{
        "oj-path": "/Users/username/.pyenv/shims/oj",
        "default-contest-dirname-format": "{ContestID}",
        "default-task-dirname-format": "{tasklabel}",
        "default-test-dirname-format": "tests",
        "default-task-choice": "inquire",
        "default-template": "python3"
}
```
```python:python3/solve.py ← chmod +x しておく
#!/usr/bin/env python
```
```json:python3/template.json
{
  "task": {
    "program": [["solve.py", "solve_{TaskID}.py"]],
  }
}
```
この設定をすると、たとえば abc 086 コンテストのA問題を取得した時にこうなる。
- フォルダ名…コンテストID
- 解答ファイル名…コンテストID+タスクID
![](/images/enjoy-atcoder-01/contest-task.png)
:::

## 3. `Visual Studio Code`
必要な拡張機能をインストールし、設定をして、実行のハードルを下げておく。
- `Python`に必要な拡張機能
- `Command Runner` …`VSCode`からコマンドを発行できる拡張機能

https://marketplace.visualstudio.com/items?itemName=edonet.vscode-command-runner
:::details 設定
### Command Runner 設定
```json:Command Runner 設定
"command-runner.commands": {
  "oj test": "cd ${fileDirname} && oj test -c ./${fileBasename} -d ./tests",
  "acc submit": "cd ${fileDirname} && acc submit ./${fileBasename}",
  "acc task": "cd ${fileDirname} && acc task"
},
```
この設定をすると、上記コマンドをタイプしなくても、一覧から選んで実行できるようになる。
![](/images/enjoy-atcoder-01/command-runner-config.png)

### デバッグ実行の構成
```json:launch.json デバッグ実行の設定。AtCoderから取得したテスト入力3種類
  {
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: 現在のファイル＋入力1",
      "type": "python",
      "request": "launch",
      "cwd": "${fileDirname}",
      "program": "${file}",
      "console": "integratedTerminal",
      "args": ["<", "tests/sample-1.in"],
      "justMyCode": true
    },
    {
      "name": "Python: 現在のファイル＋入力2",
      "type": "python",
      "request": "launch",
      "cwd": "${fileDirname}",
      "program": "${file}",
      "console": "integratedTerminal",
      "args": ["<", "tests/sample-2.in"],
      "justMyCode": true
    },
    {
      "name": "Python: 現在のファイル＋入力3",
      "type": "python",
      "request": "launch",
      "cwd": "${fileDirname}",
      "program": "${file}",
      "console": "integratedTerminal",
      "args": ["<", "tests/sample-3.in"],
      "justMyCode": true
    }
  ]
}
```
この設定をすると、入力ファイルを選んでデバッグ実行できる。
![](/images/enjoy-atcoder-01/debugconfig.png)
:::

