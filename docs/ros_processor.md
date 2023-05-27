# データ処理（点群処理）
ROSシステム上でデータを処理する例を示します。

ロボットが人間を支援するために必要不可欠な点群処理を例示します。

TurtleBot3は複数のトピックを用いて動作していますが、トピック「scan」はLaser Range Scanner（LIDARやLaser Distance Sensorなどとも呼ばれる距離センサー）で取得される点群データです。

物体までの距離を意味する点の集まりを処理することで、ロボットは物体に沿って移動したり障害物を避けながら移動したりできるようになります。

## 点群に関するトピックの理解
トピック「scan」のメッセージ型はsensor_msgs/msg/LaserScanです。

下記のコマンドで、どのようなデータで構成されているかが分かります。

```
$ ros2 interface show sensor_msgs/msg/LaserScan
```

rangesが特定の角度ごとの物体までの距離を持つリストということが分かるので、このデータに基づいてロボットを制御することを考えます。つまり、トピック「scan」を購読し、トピック「cmd_vel」を出版することを考えます。

## サンプルプログラムの実行
サンプルプログラムをsrcディレクトリーにcloneします。
 
 ```
$ mkdir -p ~/colcon_ws/src
$ cd ~/colcon_ws/src/
$ git clone git@github.com:stl-apu/laboratory_experiments_2023.git
```

ビルドします。

```
$ cd ~/colcon_ws/
$ colcon build
$ source ~/colcon_ws/install/setup.bash
```

ターミナルを2つ用いて実行します。

```
《1つ目》
$ ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
《2つ目》
$ ros2 run sample_package sample_processor_node
```

障害物を避けつつ、基本的に左回りで動きます。

## サンプルプログラムの理解
sample_processor_node.pyを確認してみます。

```
$ cat ~/colcon_ws/src/laboratory_experiments_2023/sample_package/sample_package/sample_processor_node.py
```

メッセージ型「sensor_msgs/msg/LaserScan」を使用するためには、Pythonのプログラムでモジュールsensor_msgsを読み込む必要があります。ついで、geometry_msgsも読み込んでおきます。

```
from sensor_msgs.msg import LaserScan
from geometry_msgs.msg import Twist
```

関数「create_subscription」でトピック「scan」を購読し、トピックを受け取るごとにcallback関数を実行するように指定します。

callback関数の中ではfor文やif文を用いて最も近い物体までの距離やその角度を求め、ロボットを制御するためにトピック「cmd_vel」を出版しています。このサンプルでは、基本的には直進し、正面に障害物がある場合は左に大きく回転する。また、一定範囲内に障害物がある場合は障害物の反対側に回転するように記述してあります。

if文の条件を変更することで、より知的に移動することができます。

## 設定ファイルの更新

ビルドするためにはPythonファイルだけでなく、package.xmlを更新する必要があります。第3週の課題に取り組む際は注意しましょう。

```
<exec_depend>sensor_msgs</exec_depend>
<exec_depend>geometry_msgs</exec_depend>
```

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
