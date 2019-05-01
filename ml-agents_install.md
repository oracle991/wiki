# この文書の目的

ml-agentsを導入した時の作業ログであり、後で作業手順がわからなくなった時に使う。


## ソフトウェアのバージョン

ml-agents toolkit : beta v0.8.1
Anaconda : Anaconda3-5.1.0-Windows-x86_64
Python : v3.6.8
Unity : 2017.4.26f1
Nvidia CUDA toolkit : v9.0


# 実行

## 必要なソフトウェアのインストール

基本的には以下のURLに基づいて進めていく。
https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Installation-Windows.md

Anacondaは以下のURLからインストールする。Python3.6にしか対応してないので注意。
https://repo.continuum.io/archive/Anaconda3-5.1.0-Windows-x86_64.exe

インストールが終わったらパスを通す。
%USER_PROFILE%のところは適宜読み替えて設定すること。%USER_PROFILE%のまま環境変数に突っ込んでも反映されない。(1敗)
```
%UserProfile%\Anaconda3\Scripts
%UserProfile%\Anaconda3\Scripts\conda.exe
%UserProfile%\Anaconda3
%UserProfile%\Anaconda3\python.exe
```

## conda環境の構築

新しく環境を作る。

```
conda create -n ml-agents python=3.6
activate ml-agents
```

次にtensorflowのインストールを行う。

```
pip install tensorflow==1.7.1
```

## ml-agentsのパッケージを落としてくる

```
git clone https://github.com/Unity-Technologies/ml-agents.git
```

このタイミングでmlagentsのインストールをさせる意味は謎。

```
pip install mlagents
```


## tensorflow-gpuのインストール

Nvidia CUDA toolkitとNvidia cuDNN libraryをインストール。

環境変数CUDA_HOMEを追加。Cuda toolkitをインストールした場所を指定。

PATHに以下の2つを追加。

```
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\lib\x64
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v9.0\extras\CUPTI\libx64
```

tensorflow-gpuをインストール。

```
pip uninstall tensorflow
pip install tensorflow-gpu==1.7.1
```

以下のコードをpythonインタプリタか何かで入力して動作確認。
正常にインストールされているならば、自分のGPUの名前が出てくるはず。


## サンプルの実行

以下のURLに沿って進めていく。
https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Basic-Guide.md

さっきgitで落としてきたmlagentsのリポジトリから、UnitySDKフォルダをUnityのプロジェクトとして開く。
Unity2017.4.10f1が元のバージョンなので、それ以外のバージョンだとバージョン以降のダイアログが出る。

Edit->Project Settings->Playerより、Other Settingsの中にあるScript Runtime VersionをExperimental(.NET 4.6 Equivalent)にする。

## すでに訓練されたモデルを動かす

多分再生ボタン押せば一応動くはずだ。
公式チュートリアルの手順は「ここはこういう機能だよ」というツアーになっている気がするが、今説明することではないような気がする。
ついでにメモ:ここで言うPlatformは動かせる板のPrefabを指す。

必要なやつだけ下に書く。


* Assets/ML-Agents/Examples/3DBall/Prefabs/Game/Platformを開くとBall3DAgentというスクリプトが引っ付いている。そこのスクリプトの設定項目にあるBrainのところにAssets/ML-Agents/Examples/3DBall/Brainsにある3DBallPlayerをドラックアンドドロップ。
* Assets/ML-Agents/Examples/3DBall/Brains/3DBallLearningを開くとModelという設定項目がある。そこにAssets/ML-Agents/Examples/3DBall/TFModels\3DBallLearningをドラックアンドドロップ。
* Assets/ML-Agents/Examples/3DBall/Brains/3DBallLearningにあるInference Deviceはどっちでもいい。GPU使うつもりならGPUにしておこう。
* Ctrl+Sを押してからPlay。

## 訓練

* ヒエラルキからBall3DAcademyを選択。アタッチされているBall3dAcademyスクリプトのControlにチェックを入れる。ここにチェックを入れるとbrainがpythonからの制御を受け入れることになる。
* ターミナルでml-agentsのgitリポジトリを落としてきたところまで移動。Anaconda環境の場合はactivateを忘れずに。そこで以下のコマンドを実行。

```
mlagents-learn config/trainer_config.yaml --run-id=firstRun --train
```

* "Start training by pressing the Play button in the Unity Editor" という表示が出たらUnityエディタのプレイボタンを押す。
* しばらく訓練する。
* モデルがml-agents\models\firstRun-0\3DBallLearning.nnに保存されている。
* 適当にリネームして(リネームしなくてもよい。その場合上書きになる)ml-agents\UnitySDK\Assets\ML-Agents\Examples\3DBall\TFModelsに持ってくる。
* Brainに持ってきたnnファイルを指定。
* Ball3DAcademyのControlのチェックを外して実行してみる。ボールが落ちなければ多分成功。


