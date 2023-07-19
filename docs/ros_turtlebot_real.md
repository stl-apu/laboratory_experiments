# TurtleBot（real robot）
TurtleBot 3の実機を用いたデモを4限目の開始時に行います。

## デモの準備
実験用のルーターの電源を入れ、リモートPCをルーターに接続させます。ちなみに、TurtleBotは自動的にルーターに接続するように設定してあります。

## TurtleBotの起動
TurtleBotにアダプターかバッテリーを挿し、TurtleBot内のOpenCRの電源スイッチをスライドします。

TurtleBot上のRIDARが回転し始め、起動音が鳴ります。

TurtleBotが起動したら、リモートPCからTurtleBot内のRasbperry Piに入ります。
```
$ ssh ubuntu@10.0.1.11
```

環境変数（TURTLEBOT3_MODELやROS_DOMAIN_ID）が設定されていることを確認します。
```
$ cat ~/.bashrc
```

TurtleBotを始動するためのROSパッケージを実行します。
```
$ ros2 launch turtlebot3_bringup robot.launch.py
```

## 実機の操作
ヴァーチャルロボットと同じようにリアルロボットを操作することができます。別のターミナルで操作してみます。
```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

ヴァーチャルとリアルを同時に動かすこともできます。別のターミナルを開いてシミュレーターを起動します。
```
$ ros2 launch turtlebot3_gazebo empty_world.launch.py
```

試しに点群データを確認してみます。別のターミナルを開いてRVizで確認してみます。
```
$ rviz2
```

## TurtleBotの停止
リモートPCからシャットダウンします。
```
$ sudo shutdown -h now
```

Raspberry Piの緑色LEDが消えたら、OpenCRの電源をオフにします。

TurtleBotからアダプターやバッテリーを抜いて終了です。

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
