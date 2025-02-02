# カスタムドライバを使った自動テスト

Electron アプリの自動テストを作成するには、アプリケーションを「ドライブ」する方法が必要になります。 [Spectron](https://electronjs.org/spectron) は、[WebDriver](http://webdriver.io/) を介してユーザの操作をエミュレートできる、よく使用される解決方法です。 ただし、Node に組み込まれている IPC 越し標準入出力を使用して独自のカスタムドライバを書くことも可能です。 カスタムドライバの利点は、Spectron よりもオーバーヘッドが少なくて済むことです。また、カスタムメソッドをあなたのテストスイートに公開できます。

カスタムドライバを作成するには、Node.js の [child_process](https://nodejs.org/api/child_process.html) API を使用します。 テストスイートは、以下のように Electron プロセスを spawn してから、簡単なメッセージングプロトコルを確立します。

```js
var childProcess = require('child_process')
var electronPath = require('electron')

// プロセスをスポーンする
var env = { /* ... */ }
var stdio = ['inherit', 'inherit', 'inherit', 'ipc']
var appProcess = childProcess.spawn(electronPath, ['./app'], { stdio, env })

// アプリからの IPC メッセージをリッスンする
appProcess.on('message', (msg) => {
  // ...
})

// アプリへ IPC メッセージを送る
appProcess.send({ my: 'message' })
```

Electron アプリケーション内からは、Node.js の [process](https://nodejs.org/api/process.html) API を使用して、メッセージをリッスンして返信を送信できます。

```js
// テストスイートからの IPC メッセージをリッスンする
process.on('message', (msg) => {
  // ...
})

// テストスイートへ IPC メッセージを送る
process.send({ my: 'message' })
```

これで、`appProcess` オブジェクトを使用してテストスイートから Electron アプリケーションに通信できます。

利便性のために、より高度な機能を提供するドライバオブジェクトで `appProcess` をラップすることをお勧めします。 以下は、これをどのようにするかの例です。

```js
class TestDriver {
  constructor ({ path, args, env }) {
    this.rpcCalls = []

    // 子プロセスを開始する
    env.APP_TEST_DRIVER = 1 // アプリにメッセージのリッスンが必要だと知らせる
    this.process = childProcess.spawn(path, args, { stdio: ['inherit', 'inherit', 'inherit', 'ipc'], env })

    // rpc レスポンスをハンドルする
    this.process.on('message', (message) => {
      // ハンドラを削除
      var rpcCall = this.rpcCalls[message.msgId]
      if (!rpcCall) return
      this.rpcCalls[message.msgId] = null
      // reject/resolve
      if (message.reject) rpcCall.reject(message.reject)
      else rpcCall.resolve(message.resolve)
    })

    // ready を待つ
    this.isReady = this.rpc('isReady').catch((err) => {
      console.error('Application failed to start', err)
      this.stop()
      process.exit(1)
    })
  }

  // 単純な RPC コール
  // 用法: driver.rpc('method', 1, 2, 3).then(...)
  async rpc (cmd, ...args) {
    // rpc リクエストを送信する
    var msgId = this.rpcCalls.length
    this.process.send({ msgId, cmd, args })
    return new Promise((resolve, reject) => this.rpcCalls.push({ resolve, reject }))
  }

  stop () {
    this.process.kill()
  }
}
```

このアプリでは、RPC 呼び出しのために簡単なハンドラを作成する必要があります。

```js
if (process.env.APP_TEST_DRIVER) {
  process.on('message', onMessage)
}

async function onMessage ({ msgId, cmd, args }) {
  var method = METHODS[cmd]
  if (!method) method = () => new Error('Invalid method: ' + cmd)
  try {
    var resolve = await method(...args)
    process.send({ msgId, resolve })
  } catch (err) {
    var reject = {
      message: err.message,
      stack: err.stack,
      name: err.name
    }
    process.send({ msgId, reject })
  }
}

const METHODS = {
  isReady () {
    // 必要であれば何かセットアップする
    return true
  }
  // RPC 可能なメソッドをここに定義する
}
```

すると、テストスイート内では、以下のようにテストドライバを使用できます。

```js
var test = require('ava')
var electronPath = require('electron')

var app = new TestDriver({
  path: electronPath,
  args: ['./app'],
  env: {
    NODE_ENV: 'test'
  }
})
test.before(async t => {
  await app.isReady
})
test.after.always('cleanup', async t => {
  await app.stop()
})
```
