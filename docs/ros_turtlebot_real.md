# TurtleBot（real robot）
TurtleBot 3の実機を用いたデモを4限目の開始時に行います。

## デモの準備
実験用のルーターの電源を入れ、リモートPCをルーターに接続させます。ちなみに、TurtleBotは自動的にルーターに接続するように設定してあります。

## TurtleBotの起動
TurtleBotにアダプターかバッテリーを挿し、TurtleBot内のOpenCRの電源スイッチをスライドします。

Rasbperry Piの緑色LEDがともります。そして、TurtleBot上のLIDARが回転し始め、起動音が鳴ります。

1分ほど待ち、TurtleBotが起動したら、リモートPCからTurtleBot内のRasbperry Piに入ります。
```
$ ssh ubuntu@10.0.1.11
```

環境変数（ROS_DOMAIN_IDやTURTLEBOT3_MODEL）が設定されていることを確認します。
```
$ cat ~/.bashrc
```

TurtleBotを始動するためのROSパッケージをRasbperry Piで実行します。
```
$ ros2 launch turtlebot3_bringup robot.launch.py
```

## 実機の操作
ヴァーチャルロボットと同じようにリアルロボットを操作することができます。リモートPCで別のターミナルを開いて操作してみます。
```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

ヴァーチャルとリアルを同時に動かすこともできます。別のターミナルを開いてシミュレーター（Gazebo）を起動します。
```
$ ros2 launch turtlebot3_gazebo empty_world.launch.py
```

点群データも確認してみます。別のターミナルを開いて可視化ツール（RViz）で確認してみます。Fixed Frameは「base_link」や「base_scan」に設定する。
```
$ rviz2
```

## TurtleBotの停止
全てのターミナルを停止し、リモートPCからシャットダウンします。自動的にSSH通信が切れます。
```
$ sudo shutdown -h now
```

Raspberry Piの緑色LEDが消えたら、OpenCRの電源をオフにします。

TurtleBotからアダプターやバッテリーを抜いて終了です。

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
