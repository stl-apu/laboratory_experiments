# シミュレーション
- シミュレーターの中でロボットを動作させます。
- 単純なシミュレーターturtlesimをキーボードで動かします。  

## 準備
- コンテナーの中に入り、シミュレーション用のROSパッケージをインストールします。
```
$ source /ros_entrypoint.sh
$ sudo apt update
$ sudo apt install ros-foxy-turtlesim -y
```

- ROSパッケージが実行可能であることを確認します。
```
$ ros2 pkg executables turtlesim
```

## 基礎
- 全部で3つのターミナルを用います。ターミナルを3つ用意しても良いですし、1つのターミナルに3つのタブを作成しても良いです。
- ROSノードは「Ctrlキー＋cキー」で停止できます。

### 1つ目：シミュレーターの起動
- シミュレーターを起動します。
```
$ source /ros_entrypoint.sh
$ ros2 run turtlesim turtlesim_node
```

### 2つ目：コントローラーの起動
- ロボットを動かすため、キーボード入力を速度情報に変換するROSノードを起動します。
```
$ source /ros_entrypoint.sh
$ ros2 run turtlesim turtle_teleop_key
```

- このターミナルをアクティブにした状態で矢印キー（↑キーや→キーなど）を入力すると、ロボットを動かせます。

### 3つ目：ビューワーの起動
- ノードの一覧を確認してみます。
```
$ ros2 node list
```
    - 2つのノード（turtlesimとteleop_turtle）が存在することを確認できると思います。

- ノード同士の繋がりを確認してみます。
```
$ rqt_graph
```
    - 2つのノードがトピック/turtle1/cmd_velで繋がっていることを確認できると思います。
    - 「ロボットを動作させる時」と「talkerとlistnerで会話する時」が全く同じ仕組みで動いていることを理解してください。

- ここで重要な点は、/teleop_turtleノードは速度情報を出版しているだけ、/turtlesimノードは速度情報を購読しているだけで、どのノードが出版・購読しているかは全く関係ないところです。つまり、速度情報（/turtle1/cmd_vel）を出すのが、キーボードでも、マウスでも、ジョイスティックでも、画像処理の結果でも、音声処理の結果でも、何でも大丈夫です。同様に、キーボードだけで、バーチャルなロボットも、リアルなロボットも、自動車も、ドローンも、何でも操作することができます。これがROSの利点です！
- 「○○を操作したいなぁ」と思った時にteleop_turtleを起動し、必要なくなったら停止する。あるいは「○○を確認したいなぁ」と思った時にrqt_graphを起動し、必要なくなったら停止する。このように必要な機能を簡単に追加・削除できることもROSの利点の1つです。「腕を1本追加したいなぁ」「2本足から車輪に変更したいなぁ」「このセンサーは要らないなぁ」といった感じでロボットを改良できるという意味です。

## 発展

### 時系列データの確認
- GUIツール「rqt_plot」でロボットに対する制御入力の時系列変化を確認できます。
```
$ ros2 run rqt_plot rqt_plot
あるいは
$ rqt_plot
```

- ボックス「Topic」にトピック名と変数名を入力します。
```
《記法》
トピック名/変数名
《実例》
/turtle1/cmd_vel/linear/x
や
/turtle1/cmd_vel/angular/z
```
- ボタン「+」で表示したいTopicをグラフに追加します。
- キーボード入力に合わせてグラフが更新されます。
    - Figure options（グラフみたいなアイコン）でy軸の範囲を変更すると見やすくなります。

### 様々なデータの確認・変更
- rqt_plotなどの元になっているrqtはROSのGUIツールで、カスタマイズして使用ことができます。
- 様々な情報を同時に確認することができますし、情報を確認しながら値を変更することもできます。

- 例として、Robot Steeringを用いてトピックを出版してみます。
- まず、rqtを起動します。
```
$ rqt
```
- 次に、［Plugins］→［Robot Tools］からでRobot Steeringを追加します。
- ボックスに/turtle1/cmd_velと入力し、スライダーで値を変化させます。値が変化すると、自動的にトピックが出版されます。

### マウスを用いた操作
- ロボットをマウスで操作することもできます。
- シミュレーターを一旦終了し、必要なソフトウェアをインストールします。

- ROSパッケージ「teleop_tools」をcloneします。
```
$ cd ~/colcon_ws/src
$ git clone git@github.com:ros-teleop/teleop_tools.git
```

- foxy版のブランチを切り替えます。
```
$ cd teleop_tools
$ git checkout foxy-devel
```

- ビルドします。
```
$ cd ~/colcon_ws && colcon build
$ source ~/colcon_ws/install/setup.bash
```

- シミュレーターなどを再度実行し、マウスで操作するためのROSノードを起動します。
```
$ ros2 run mouse_teleop mouse_teleop
↓
$ ros2 run mouse_teleop mouse_teleop --remap mouse_vel:=/turtle1/cmd_vel
```
    - マウスのドラッグにより並進移動量と回転移動量を出版することができます。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
