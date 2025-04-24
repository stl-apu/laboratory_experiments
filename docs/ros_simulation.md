# シミュレーション
シミュレーターの中でヴァーチャルロボットを動作させます。本実験では単純なシミュレーター「turtlesim」を用い、ヴァーチャルな亀ロボットキーボードで動かします。  


## 準備
DockerサービスとDockerコンテナーが起動していること、そしてX Window Systemが使える状態になっていることを確認してください。

コンテナーの中に入り、シミュレーション用のROSパッケージをインストールします。
```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ sudo apt update
$ sudo apt install ros-humble-turtlesim -y
```

ROSパッケージ「turtlesim」が実行可能であることを確認しておきます。
```
$ ros2 pkg executables turtlesim
```

1度ターミナルを閉じても良いですし、引き続き、演習を実行しても大丈夫です。


## 基礎
全部で3つのターミナルを用います。ターミナルを3つ用意しても良いですし、1つのターミナルに3つのタブを作成しても良いです。なお、ROSノードは「Ctrl＋c」で停止できますので、覚えておいてください。

### 1つ目：シミュレーターの起動
シミュレーターを起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ ros2 run turtlesim turtlesim_node
```

### 2つ目：コントローラーの起動
亀ロボットを動かすため、キーボード入力を速度情報に変換するROSノードを起動します。このターミナルをアクティブにした状態で矢印キー（↑や→など）を入力すると、亀ロボットを動かせます。
```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ ros2 run turtlesim turtle_teleop_key
```

### 3つ目：ビューワーの起動
ノードの一覧を確認してみます。2つのノード（/turtlesimと/teleop_turtle）が存在することを確認できると思います。
```
$ ros2 node list
```

ノード同士の繋がりを確認してみます。2つのノードがトピック「/turtle1/cmd_vel」で繋がっていることを確認できると思います。
```
$ rqt_graph
```

「ロボットを動作させる時」と「talkerとlistnerを会話させる時」が全く同じ仕組みで動いていることを理解してください。ここで重要なのは、ノード「/teleop_turtle」は速度情報を出版しているだけ、ノード「/turtlesim」は速度情報を購読しているだけで、どのノードが出版・購読しているのかは全く関係ないところです。つまり、速度情報「/turtle1/cmd_vel」を出すのが、キーボードでも、マウスでも、ジョイスティックでも、画像処理の結果でも、音声処理の結果でも、何でも大丈夫です。同様に、キーボードだけで、ヴァーチャルロボットも、リアルロボットも、自動車も、ドローンも、潜水艦も、何でも操作することができます。これがROSの利点になります。

もう1つ、ROSの利点を説明します。「○○を操作したいなぁ」と思った時にROSノード「teleop_turtle」を追加し、必要なくなったら削除する。あるいは「○○を確認したいなぁ」と思った時にROSノード「rqt_graph」を追加し、必要なくなったら削除する。このように、必要な機能を必要な時に簡単に追加・削除できます。ROS対応のハードウェアを用いることで、「ロボットアームを1本追加したいなぁ」「2本足から車輪へ移動機構を変更したいなぁ」「このセンサーは要らないなぁ」といった感じに、ロボットを気軽に改良できます。

## 発展
課題のために、研究開発に役立つROSツールを紹介しておきます。余裕のある人は試してみてください。

### 時系列データの確認
ROSツール「rqt_plot」でロボットに対する制御入力の時系列変化を確認できます。
```
$ ros2 run rqt_plot rqt_plot
あるいは
$ rqt_plot
```

ボックス「Topic」にトピック名と変数名を入力し、ボタン「+」で表示したいTopicをグラフに追加します。
```
《記法》
トピック名/変数名
《実例》
/turtle1/cmd_vel/linear/x
や
/turtle1/cmd_vel/angular/z
```

キーボード入力（速度情報）に合わせてグラフが更新されると思います。Figure options（グラフみたいなアイコン）でy軸の範囲を変更すると見やすくなります。参考として、前進する時の速度は2、後進する時の速度は-2、回転する時の速度は0です。

### 様々なデータの確認・変更
rqt_plotなどの元になっているrqtはROSのGUIツールで、カスタマイズして使用ことができます。様々な情報を同時に確認することができますし、情報を確認しながら値を変更することもできます。例として、Robot Steeringを用いてトピックを出版してみます。

まず、rqtを起動します。
```
$ rqt
```

次に、［Plugins］→［Robot Tools］から［Robot Steering］を追加します。Robot Toolsが見つからない人は、rqtをオプション付きで実行してみてください。ボックスに「/turtle1/cmd_vel」と入力し、スライダーで値を変化させてみます。値が変化すると、自動的にトピックが出版され、ロボットが動くと思います。
```
$ rqt --force-discover
```


### マウスを用いた操作
ロボットをマウス（タッチパッドやトラックパッドも含む）で操作することもできます。シミュレーターを一旦終了し、必要なソフトウェアをインストールしてから試しましょう。

ROSパッケージ「teleop_tools」をcloneします。
```
$ cd ~/colcon_ws/src/
$ git clone git@github.com:ros-teleop/teleop_tools.git
```

humble版のブランチを切り替えます。
```
$ cd teleop_tools
$ git checkout humble-devel
```

ビルドします。
```
$ cd ~/colcon_ws && colcon build
$ source ~/colcon_ws/install/setup.bash
```

シミュレーターなどを再実行し、マウスで操作するためのROSノードを起動します。マウスのドラッグにより並進移動量と回転移動量を出版することができますが、そのままでは「/mouse_vel」というトピック名を出版するので、ロボットは動きません。オプションremapを用いて「/mouse_vel」を「/turtle1/cmd_vel」というトピック名で出版するように変更します。
```
$ ros2 run mouse_teleop mouse_teleop --remap mouse_vel:=/turtle1/cmd_vel
``` 

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
