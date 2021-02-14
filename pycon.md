---
theme: gaia
_class: lead
paginate: true
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
そんなサーバ...

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

```sh
$ pip install discord.py
# 音声系を使う場合
$ pip install discord.py[voice]
```

これだけでBOT開発を始められます!

(※別途discord botは作成する必要があります)
(url)

---
## 何ができる!?
+ 
+ 
+ 
+ 


---
## 作ってみた
簡単なbotを早速作ってみました
discordの状態を取得し、外部APIに連携するBOTです
これがあれば、ホワイトボード当のstatusをdiscordから設定できます

```
コード
```

---
<!-- Scoped style -->
<style scoped>
section {
  font-size:25px;
}
</style>

# 最小限のコード
公式からサンプルで出されている最小限コードです
>https://discordpy.readthedocs.io/en/latest/quickstart.html


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
## まとめ
+ 世に出回っているBOTは全部が全部安全ではない
+ discord.pyを使えば簡単にAPIを扱える
+ 録音の高レベルAPIがない!!!
  + JS側はある
  + 強い方commitを是非

<!--

---

# How to write slides

Split pages by horizontal ruler (`---`). It's very simple! :satisfied:

```markdown
# Slide 1

foobar

---

# Slide 2

foobar
```

-->