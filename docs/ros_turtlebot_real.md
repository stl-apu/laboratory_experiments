# TurtleBot（Real robot）
TurtleBot3の実機を用いたデモを4限目の開始時に行います。

## デモの準備
実験用のルーターの電源を入れ、リモートPCをルーターに接続させます。ちなみに、TurtleBot自身は自動的にルーターに接続するよう、予め設定してあります。

## TurtleBotの起動
TurtleBotにアダプター（バッテリー）を接続し、TurtleBot内のOpenCRの電源スイッチをスライドします。

Rasbperry Piの緑色LEDが一時的に点灯します。そして、起動音が鳴り、TurtleBot上のLIDARが回転し始めます。

1分ほど待ち、TurtleBotが起動したら、リモートPCからTurtleBot内のRasbperry PiにSSH接続で入ります。なお、IPアドレスは固定してあります。
```
$ ssh ubuntu@10.0.1.11
```

まず、環境変数（ROS_DOMAIN_IDやTURTLEBOT3_MODEL）が設定されていることを確認します。
```
$ cat ~/.bashrc
```

TurtleBotを始動するためのROSパッケージをRasbperry Pi上で実行します。
```
$ ros2 launch turtlebot3_bringup robot.launch.py
```

## 実機の操作
ヴァーチャルロボットと同じようにリアルロボットを操作することができます。

ターミナルを開き、リモートPCの環境変数も確認しておきます。
```
$ cat ~/.bashrc
```

リモートPCでROSトピックを受け取れていることを確認します。
```
$ ros2 topic list
```

キーボードで操作してみます。
```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

ヴァーチャルとリアルを同時に動かすこともできます。別のターミナルを開き、シミュレーター（Gazebo）を起動します。
```
$ ros2 launch turtlebot3_gazebo empty_world.launch.py
```

点群データも確認してみます。別のターミナルを開いて可視化ツール（RViz）で確認できます。Fixed Frameは「base_link」や「base_scan」に設定してみます。そして、ボタン「Add」でトピック「/scan」を追加してみます（点群が表示されない場合はReliability PolicyをBest Effortに切り替えてみます。）。
```
$ rviz2
```

## TurtleBotの停止
全てのターミナルを停止し、リモートPCからシャットダウンします。自動的にSSH通信が切れます。
```
$ sudo shutdown -h now
```

Raspberry Piの緑色LEDが消えたら、OpenCRの電源をオフにします。

TurtleBotからアダプター（バッテリー）を抜いて終了です。

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
