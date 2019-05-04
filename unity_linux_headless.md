# この文書の目的

Unity(ゲームエンジン)のHeadlessモードで実行ファイルをビルドし起動した際の挙動を調査することを目的とする。

## Headlessモードとは
Unityにはビルド設定にHeadlessモードというのがある。
このモードでビルドすると、描画が取り除かれたバージョンの実行ファイルがビルドされる。

## ユーザー入力について

Unityはコマンドラインからも起動できる。このコマンドラインから起動するモードで引数に-batchmodeをつけるとHeadlessモードと同等の動作になる。

```
-batchmode	ゲームを “headless” モードで実行します。ゲームは何も表示せず、ユーザー入力を受付ません。これはネットワークゲーム でのサーバー実行に最も便利です。
```

マニュアルには上記のように書かれている。そのため、ゲーム中にUIを選択すると次に進む部分等があるならば、その部分を無視するか書き換えなければならない。

## どのようにしてHeadlessモードであると判定するか
参考URL:https://qiita.com/su10/items/a56762ce3fe1b529e0bd

1. SystemInfo.graohicsDeviceType
2. SystemInfo.graphicsDeviceID

このどちらかで判定する。



## その他参考URL

* Microsoft Azure DSVMでUnity ML-Agentsを使用した強化学習を行う http://kentablog.cluscore.com/2018/08/microsoft-azure-dsvmunity-ml-agents.html
* How to run Unity on Amazon Cloud or without Monitor https://towardsdatascience.com/how-to-run-unity-on-amazon-cloud-or-without-monitor-3c10ce022639
