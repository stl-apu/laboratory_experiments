# データ処理（点群処理）
ROSシステム上でデータを処理してみます。ロボットが人間を支援するために必要不可欠な点群処理の例を示します。

TurtleBot3は複数のトピックを用いて動作していますが、トピック「/scan」はlaser range scanner（laser range finderやLIDARなどとも呼ばれる測域センサー）で取得される点群データです。物体までの距離を意味する点の集まりを処理することで、ロボットは物体に沿って移動したり障害物を避けながら移動したりできるようになります。

ロボット分野では3次元点群（xyz空間）を取り扱うことが多いのですが、トピック「/scan」は2次元点群（xy平面）になります。3次元より2次元の方が分かりやすいので、入門編と考えてください。

## 点群に関するトピックの理解
トピック「/scan」のメッセージ型は「sensor_msgs/msg/LaserScan」です。下記のコマンドで、どのようなデータで構成されているかを確認することができます。rangesが特定の角度ごとの物体までの距離を持つリストということが分かりますので、このデータに基づいてロボットを制御することを考えてみます。つまり、トピック「/scan」を購読し、トピック「/cmd_vel」を出版することを考えます。
```
$ ros2 interface show sensor_msgs/msg/LaserScan
```

## サンプルプログラムの実行
GUIに対応したROS用のDockerコンテナーを用いて演習を行います。

サンプルプログラムをsrcディレクトリーにcloneします。cloneする前に、SSHで接続できるように、GitHubの鍵認証を設定します。第1週の内容を復習してください。
```
鍵認証を設定した後
↓
$ mkdir -p ~/colcon_ws/src/ && cd $_
$ git clone -b develop git@github.com:stl-apu/laboratory_experiments_2025.git
```

ビルドしてみます。
```
$ cd ~/colcon_ws/
$ colcon build
```

ビルドに成功したら、ビルドしたROSパッケージを使えるように設定します。毎回、読み込んで設定するのは面倒なので、設定ファイル「~/.bashrc」に書き込んでしまうことをオススメします。
```
$ source ~/colcon_ws/install/setup.bash
```

ターミナルを2つ用いて実行します。ロボットが障害物を避けつつ、基本的に左回りで動くと思います。
```
《1つ目》
$ ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
《2つ目》
$ ros2 run sample_package sample_processor_node
```

## サンプルプログラムの理解
sample_processor_node.pyの中身を確認してみます。
```
$ cat ~/colcon_ws/src/laboratory_experiments_2025/sample_package/sample_package/sample_processor_node.py
```

メッセージ型「sensor_msgs/msg/LaserScan」を使用するためには、Pythonのプログラムでモジュールsensor_msgsを読み込む必要があります。ついでに、トピック「/cmd_vel」を出版するために、モジュールgeometry_msgsのメッセージ型「TwistStamped」も読み込んでおきます。
```
from sensor_msgs.msg import LaserScan
from geometry_msgs.msg import TwistStamped
```

関数「create_subscription」でトピック「/scan」を購読し、トピックを受け取るごとにcallback関数を実行するように指定します。

callback関数の中ではfor文やif文を用いて最も近い物体までの距離や角度を求め、ロボットを制御するためにトピック「cmd_vel」を出版しています。

このサンプルでは、基本的に直進し、正面に障害物がある場合は左に大きく回転する。また、一定範囲内に障害物がある場合は障害物の反対側に回転するように記述してあります。このような単純な制御ルールでも空間を移動することができます。そして、制御ルールを変更することで、より知的に移動することができるようになります。

## 参考：設定ファイルの修正
ビルドするためにはPythonファイルだけでなく、設定ファイル（package.xmlなど）も更新する必要があります。第3週の課題に取り組む際は注意しましょう。
```
<exec_depend>sensor_msgs</exec_depend>
<exec_depend>geometry_msgs</exec_depend>
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
