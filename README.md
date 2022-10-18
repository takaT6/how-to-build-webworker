## howto-build-webworker
webpackやVite, Vue-cliで"Web Worker"のファイルも含めてビルドしたいときの対処法

## ローカルで動かす場合
main.ts
```
const worker = new Worker("./worker.ts");
worker.onmessage = (event: MessageEvent) => {
  console.log(event.data);
};
```

worker.ts
```
const _worker: Worker = self as any;

// mainスレッドへ送信
_worker.postMessage("hoge");
```

この書き方だど、ビルドツールが"worker.ts"をビルドしてくれない

## ビルドツールに"worker.ts"を含めるようにする方法
main.ts
```
import MyWorker from "./worker?worker";
const worker = new MyWorker();
worker.onmessage = (event: MessageEvent) => {
  console.log(event.data);
};
```

worker.ts
```
const _worker: Worker = self as any;

// mainスレッドへ送信
_worker.postMessage("hoge");
```

## まとめ
下記のソースコードを入れれば、ビルドツールがビルドして出力してくれる
```
import MyWorker from "./worker?worker";
const worker = new MyWorker();
```

