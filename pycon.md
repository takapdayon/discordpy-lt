---
theme: style
_class: lead
paginate: false
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
marp: true
style: |
  section.title * {
      text-align: center;
  }
---

# **Discord ヘビーユーザによる**
## **discord.py 解説**

---

## おまえ誰よ
<!--
まず、おまえだれよ、ということで
・Discord歴は4年です(歴だけ長いとはいわせないけど歴だけ長いです)
こちらの資料githubにあげますので、もしまたみたいよって方いましたらそちらから御願いします
-->

- 塚田 貴史
- Discord 歴 4 年(2016 年から)
  - (歴が長いだけ....とはいわせない!!)
- お仕事・趣味で Python に触れている
- **重度**(**重度**)のゲーマー
- github/takapdayon

---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## LT で持って帰ってもらいたいもの

---
<!--
bot開発は簡単だよ!
ってことだけを持ち帰っていただければと思います
-->
<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## :v:bot開発は簡単だよ!:v:

---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## 突然ですが!


---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## Discord 使ってますか?

---

![bg fit 50%](./assets/logo.svg)

---
<!--
少しだけdiscord紹介させてください
ゲームの企業等がユーザにフィードバックをもらう場としても使われています
-->
## Discord

元々はゲーマー向けコミュニケーションツールとしてデビュー
今では、大学・企業・ゲーム以外のコミュニティでも幅広く活躍!!!

- Python.jp
- discord.py
- etc...

そして、なんとサーバ費は驚きの ZERO!(ブーストはあります)
そんな Discord サーバ...

---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## 豪華に...したくないですか?

---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## 便利にしたく...ないですか?

---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## その願い、**discord.py**で叶うかもしれません!

---

## discord.py

Discord API をラップして Python から使えるようにしたライブラリ
中で Discord との認証等ごにょってくれているためとてもありがたい

---
<!--
始め方は以下２つだけです

-->
## 始め方

- discord bot を作成する
- pip で discord.py を入れる

これだけです!

---
<!--

-->
## 1: discord bot を作成する

discord.py で紹介されています
https://discordpy.readthedocs.io/en/latest/discord.html

見てわかる通り、やることは凄く少ないです

1. botの名前を決めて作成
2. botの**トークン**を入手
3. botをDiscordサーバに招待する

![bg right:40% 90%](./assets/create.gif)

---

## 2: pip で discord.py を入れる

おなじみパッケージ管理ツール pip を使います

```sh
$ pip install discord.py

# 音声系を使う場合
$ pip install discord.py[voice]
```

---


<style scoped>
section {
  font-size:25px;
}
</style>

<!--
後ほどざっくりコードは紹介しますのでここでは、一つだけ解説させてください
-->
# Hello World!

公式からサンプルで出されている最小限コードです
https://discordpy.readthedocs.io/en/latest/quickstart.html

```python
import discord

client = discord.Client()

@client.event
async def on_ready():
    print('We have logged in as {0.user}'.format(client))

@client.event
async def on_message(message):
    if message.author == client.user:
        return

    if message.content.startswith('$hello'):
        await message.channel.send('Hello!')

client.run('Botトークン')
```

---
<style scoped>
section {
  font-size:25px;
}
.hljs-comment {
  color: #FF644E;
}
</style>

<!--
コメントアウトでこことなっている部分のif文なのですが
ここで、チャットのメッセージの内容に$helloが含まれているかを判定し
awaitで、対象のサーバに対してsendの中身を返しています
ここでは'hello'を返していますね!
-->
# Hello World!

<!-- TODO ハイライトを見やすくする -->

公式からサンプルで出されている最小限コードです
https://discordpy.readthedocs.io/en/latest/quickstart.html

```python
import discord

client = discord.Client()

@client.event
async def on_ready():
    print('We have logged in as {0.user}'.format(client))

@client.event
async def on_message(message):
    if message.author == client.user:
        return

    # ここ!
    if message.content.startswith('$hello'):
        await message.channel.send('Hello!')

# ここ!
client.run('Botトークン')
```

---
<!--
では、実際に動かしてみましょう
なんとpythonファイルを実行するのと同じように実行するだけで
discord.py君がよしなにやってくれて動かしてくれます
-->
## 動かしてみよう

先ほどのコードを実行してみたいと思います

```sh
$ python Main.py
We have logged in as test-slack
```

これだけで、discord.py 側で、よしなにしてくれます

---
<!--
では、先ほどのファイルを動かした後に
botを導入したサーバのチャットで、$helloと投稿してみましょう
botからHelloと返ってきたら成功です
なんとこれだけでチャットを返してくれるbotが出来上がってしまいました!
-->
## 試す

bot を導入した Discord サーバで$hello と投稿してみましょう
Bot が Hello!と返してくれれば成功です

![height:350](./assets/cap.png)

---
<!--
今回は、on_messageでチャットが投稿されたことを検知しましたが、ほかにもさまざま
なアクションをトリガーできます。一覧は.pyに乗ってますので、興味ありましたら
覗いてみてください
-->
## 何をトリガーにできるのか

一覧は discord.py の API で紹介されています
https://discordpy.readthedocs.io/ja/latest/api.html
![height:400](./assets/api.png)

---
<!--
ちらっと中身解説しますが、ちょっとお時間ないので、多少省きながら行きます
@client.eventのデコレータで、コルーチンの関数かを判定し、そうだった場合に、clientに変数として関数ごともたせます
-->
## ちらっと中身解説

```python
client = discord.Client()

@client.event
async def on_message(message):
    if message.author == client.user:
        return
```

- @client.event
  - 対象の関数が**コルーチン関数**か判定し、コルーチンの場合は client にインスタンス変数として保持させます

---
<!--
on_messageで、discord側から常にアクションが届いているのですが、そのアクションがチャットの投稿だった時に動く関数を定義しています
message.authorで、botが投稿したアクションにbotが反応しないようにします
-->
## ちらっと中身解説

```python
client = discord.Client()

@client.event
async def on_message(message):
    if message.author == client.user:
        return
```

- async def on_message(message):
  - discord.py側で実行する関数です。messageの中にチャットしたサーバ等の情報が入ってます

- if message.author == client.user:
  - メッセージを発信した対象が bot 自身か判定(無限ループ防止)

---
## ちらっと中身解説

```python
client.run('Botトークン')
```

- client.run()
  - 中でstart関数(login関数とconnect関数)がTaskとして登録され、run_forever()で永続化されて動いています。
  asyncio の低レベルAPI()がゴリゴリ動いているので、興味ある方は見てみると面白いかもしれません!

---

## まとめ

- bot 開発は簡単に行える(ホントに敷居が低いです:v:)
- 世に出回っている BOT は安全ではない場合もあります
  - slackbot とか google エクステンションと同じですが...

---

<!-- _class: title -->
<br>
<br>
<br>
<br>

## You are the server customizer Champion!
## (ご清聴ありがとうございました!)