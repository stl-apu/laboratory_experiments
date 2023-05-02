# シミュレーション
- シミュレーターの中でロボットを動作させます。
- 単純なシミュレーターturtlesimをキーボードで動かします。  

## 準備
- コンテナーの中に入り、シミュレーション用のROSパッケージをインストールする。
```
$ source /ros_entrypoint.sh
$ sudo apt update
$ sudo apt install ros-foxy-turtlesim -y
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




- 重要なポイントは、/teleop_turtleノードは速度情報を出版しているだけ、/turtlesimノードは速度情報を購読しているだけで、どのノードが配信・購読しているかは全く関係ないところです。つまり、速度情報（/turtle1/cmd_vel）を出すのが、キーボードでも、マウスでも、ジョイスティックでも、画像処理の結果でも、音声処理の結果でも、何でも大丈夫です。同様に、キーボードだけで、バーチャルなロボットも、リアルなロボットも、自動車も、ドローンも、何でも操作することができます。
- これがROSの利点です！


## 発展

### 4つ目：GUIツールで確認
- ツール「rqt_plot」でロボットに対する制御入力を確認できます。
  ```
  $ rosrun rqt_plot rqt_plot
  や
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

### 5つ目：GUIツールで操作
- rqt_plotなどの元になっているrqtはROSのGUIツールで、カスタマイズして使用ことができます。
- 様々な情報を同時に確認することができますし、情報を確認しながら値を変更することもできます。
- 例えば、Robot Steeringを用いてトピックを出版することができます。
- まず、rqtを起動します。
  ```
  $ rqt
  ```
- 次に、［Plugins］→［Robot Tools］からでRobot Steeringを追加します。
- ボックスに/turtle1/cmd_velと入力し、スライダーで値を変化させます。値が変化すると、自動的に出版されます。

### 6つ目：マウスで操作
- マウスで操作することもできます。
- シミュレーターを一旦終了し、必要なソフトウェアをインストールします。
- ROSパッケージ「teleop_tools」をcloneします。
  ```
  $ cd ~/catkin_ws/src
  $ git clone git@github.com:ros-teleop/teleop_tools.git
  ```
- ROS2版が標準となっているので、ROS1版に切り替えます。
  ```
  $ cd teleop_tools
  $ git checkout kinetic-devel
  ```
- ビルドします。
  ```
  $ cd ~/catkin_ws
  $ catkin_make
  $ source ~/catkin_ws/devel/setup.bash
  ```
- ROSマスター（1つ目）やシミュレーター（2つ目）を再度実行し、マウスで操作するためのROSノードを起動します。
  ```
  $ rosrun mouse_teleop mouse_teleop.py mouse_vel:=/turtle1/cmd_vel
  ```
- ドラッグにより並進移動量と回転移動量を出版することができます。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
