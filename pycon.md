---
theme: gaia
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

# **Discordヘビーユーザによる**
## **discord.py解説**

---

## おまえ誰よ

+ 塚田 貴史
+ discord歴4年(2016年から)
  + (歴が長いだけ....とはいわせない!!)
+ お仕事・趣味でpythonに触れている
+ **重度**(**重度**)のゲーマー
+ github/takapdayon


---

## アジェンダ
+ discordについて
+ discord.py
  + 何ができる!?
    + 世のbot紹介
    + こんなBOTどう紹介
  + discord.pyの中身をちらっと覗き見
+ まとめ

---
## LTで持って帰ってもらいたいもの
+ 開発すぐ始められるよ
  + 余計な手続きがほとんどない
+ サーバ豪華になるよ
+ 世のbot(slackとか)と同じようにできるよ


---
<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## discord 使ってますか?

---

![bg fit 50%](./assets/logo.svg)

---
## discord
元々はゲーマー向けコミュニケーションツールとしてデビュー
今では、大学・企業・ゲーム以外のコミュニティでも幅広く活躍!!!
+ pycon
+ etc...

そして、なんとサーバ費は驚きのZERO!(ブーストはあります)
そんなdiscordサーバ...

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
discord APIをラップしてpythonから使えるようにしたライブラリ
中でdiscordとの認証等ごにょってくれているためとてもありがたい




---
## 始め方
+ discord botを作成する
  +
  +
  +
+ pipでdiscord.pyを入れる

これだけです!

---
## 1: discord botを作成する
discord.pyで紹介されています
(https://discordpy.readthedocs.io/en/latest/discord.html)

見てわかる通り、やることは凄く少ないです

![bg right:40% 90%](./assets/create.gif)


---
## 2: pipでdiscord.pyを入れる

おなじみパッケージ管理ツールpipを使います
```sh
$ pip install discord.py

# 音声系を使う場合
$ pip install discord.py[voice]
```

これだけでBOT開発を始められます!

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
> https://discordpy.readthedocs.io/en/latest/quickstart.html

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

client.run('your token here')
```

---
## 動かしてみよう
先ほどのコードを実行してみたいと思います
```sh
$ python Main.py
We have logged in as test-slack
```
これだけで、discord.py側で、よしなにしてくれます

---
## 試す
botを導入したdiscordサーバで$helloと打ってみましょう
BotがHello!と返してくれれば成功です

![150%](./assets/cap.png)

うまくいきましたね！


---
## 他にちょっと紹介
+ **on_member_join**: ユーザがjoinしたことを検知する
  + joinしたユーザに対してBotからDMで説明を送る
  + joinした情報を外部APIに投げて連携する

+ **on_member_update**: ユーザのステータス変更を検知する
  + ゲームアクティビティを取得し、各ゲーム時間を算出する
    + (これ作りたい!)


---
## 何をトリガーにできるのか
一覧はdiscord.pyのAPIで紹介されています
> https://discordpy.readthedocs.io/ja/latest/api.html


---
## ちらっと中身解説

```python
client = discord.Client()

@client.event
async def on_message(message):
    if message.author == client.user:
        return
```

+ @client.event
  + 対象の関数が**コルーチン関数**か判定し、コルーチンの場合はclientにインスタンス変数として保持させます
+ if message.author == client.user:
  + メッセージを発信した対象がbot自身か判定(無限ループ防止)


---
## ちらっと中身解説

```python
client.run('your token here')
```

+ client.run()
  + 中でasyncioの低レベルAPI()

---
## まとめ
+ bot開発は簡単に行える(ホントに敷居が低いです!)
+ 世に出回っているBOTは安全ではない場合もある

---

<!-- _class: title -->
<br>
<br>
<br>
<br>
<br>

## あなただけのサーバに進化させましょう!

---

<!--
---
### discord.pyの中身をちらっと覗き見
最小限のコードでasync/awaitで定義している事からわかるように
asyncioで動いています

```
client = discord.Client()
```
全ての元になるClientクラスをインスタンス化してます

```
@client.event
```
先ほどインスタンス化したclientのevent凸れーたです

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
startの中でloginとconnectの処理が走ります
やってることとしては、discord側との認証を確保しています
connectでdiscord側からデータを受け取るクラスをインスタンス化しています

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

+ ゲームのチーム分け
  + ゲームのAPIからキャラクターを取得して、それぞれに割り振る

+ グローバルチャット
  + 他のサーバのみんなと...チャットができちゃう!?

+ slackにjoin通知を送る
  + joinしたら、お知らせするようにしましたb

-->