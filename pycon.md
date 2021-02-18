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

<!--![bg left:40% 80%](https://marp.app/assets/marp.svg)-->

# **Discord ヘビーユーザによる**
## **discord.py 解説**

---

## おまえ誰よ

- 塚田 貴史
- Discord 歴 4 年(2016 年から)
  - (歴が長いだけ....とはいわせない!!)
- お仕事・趣味で Python に触れている
- **重度**(**重度**)のゲーマー
- github/takapdayon

---

## アジェンダ

<!--TODO きれいにする-->

- Discord について
- discord.py
  - 何ができる!?
    - 最小限のコード紹介
    - トリガー紹介
  - discord.py の中身をちらっと覗き見
- まとめ

---

## LT で持って帰ってもらいたいもの

- bot の開発は簡単だよ!
  - 余計な申請とかいらないよ!
- bot を導入したサーバが豪華になるよ
- 世の bot(slack とか)と同じように開発できるよ

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

## Discord

元々はゲーマー向けコミュニケーションツールとしてデビュー
今では、大学・企業・ゲーム以外のコミュニティでも幅広く活躍!!!

- Python.jp
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

## 豪華にしたくないですか?

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

## 始め方

- discord bot を作成する
- pip で discord.py を入れる

これだけです!

---

## 1: discord bot を作成する

discord.py で紹介されています
https://discordpy.readthedocs.io/en/latest/discord.html

見てわかる通り、やることは凄く少ないです

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

<!-- Scoped style -->
<style scoped>
section {
  font-size:25px;
}
</style>

# 最小限のコード

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

    if message.content.startswith('$hello'):
        await message.channel.send('Hello!')

client.run('Botトークン')
```

---

## 動かしてみよう

先ほどのコードを実行してみたいと思います

```sh
$ python Main.py
We have logged in as test-slack
```

これだけで、discord.py 側で、よしなにしてくれます

---

## 試す

bot を導入した Discord サーバで$hello と投稿してみましょう
Bot が Hello!と返してくれれば成功です

![height:350](./assets/cap.png)

---

## 他にちょっと関数を紹介

- **on_member_join**: ユーザが join したことを検知する

  - 使い道
    - join したユーザに対して Bot から DM で説明を送る
    - join した情報を外部 API に投げて連携する

- **on_member_update**: ユーザのステータス変更を検知する
  - 使い道
    - ゲームアクティビティを取得し、各ゲーム時間を算出する
    - (これ作りたい!)

---

## 何をトリガーにできるのか

一覧は discord.py の API で紹介されています
https://discordpy.readthedocs.io/ja/latest/api.html
![height:400](./assets/api.png)

---

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

- bot 開発は簡単に行える(ホントに敷居が低いです!)
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

---

## <!--

### discord.py の中身をちらっと覗き見

最小限のコードで async/await で定義している事からわかるように
asyncio で動いています

```
client = discord.Client()
```

全ての元になる Client クラスをインスタンス化してます

```
@client.event
```

先ほどインスタンス化した client の event 凸れーたです

---

## @client.event

```python
    def event(self, coro):
        if not asyncio.iscoroutinefunction(coro):
            raise TypeError('event registered must be a coroutine function')

        setattr(self, coro.__name__, coro)
        log.debug('%s has successfully been registered as an event', coro.__name__)
        return coro
```

関数が**コルーチン**で定義されているかをチェックし
よければインスタンス変数として関数を保持する感じです

---

## client.run('your token here')

```python
def run(self, *args, **kwargs):
    #~省略~
    async def runner():
        try:
            await self.start(*args, **kwargs)
        finally:
            if not self.is_closed():
                await self.close()
```

start の中で login と connect の処理が走ります
やってることとしては、discord 側との認証を確保しています
connect で discord 側からデータを受け取るクラスをインスタンス化しています

---

## client.run('your token here')

```python
def run(self, *args, **kwargs):
    #~省略~
    def stop_loop_on_completion(f):
        loop.stop()

    future = asyncio.ensure_future(runner(), loop=loop)
    future.add_done_callback(stop_loop_on_completion)
    try:
        loop.run_forever()
    except KeyboardInterrupt:
        log.info('Received signal to terminate bot and event loop.')
    finally:
        future.remove_done_callback(stop_loop_on_completion)
        log.info('Cleaning up tasks.')
        _cleanup_loop(loop)
```

---

## こんなの作りました

自分が過去に作ったものを荒く紹介

- ゲームのチーム分け

  - ゲームの API からキャラクターを取得して、それぞれに割り振る

- グローバルチャット

  - 他のサーバのみんなと...チャットができちゃう!?

- slack に join 通知を送る
  - join したら、お知らせするようにしました b

-->
