# TurtleBot（virtual robot）
- リモートPC側で実行します。
- Gazeboを用いたシミュレーションです。

## Dockerコンテナーの作成
- GUIに対応したROS用のDockerコンテナーを作成します。
```
$ docker container run -p 6080:80 --shm-size=512m tiryoh/ros2-desktop-vnc:foxy
```

- ウェブブラウザーを開き、下記のURLでアクセスしてみます。
    - http://localhost:6080

- ターミナルは［左下のボタン］→［System Tools］→［LXTerminal］で開けます。
- コピー＆ペーストは左端のcontrol barのClipboardを使用します。
    - Clipboardにペースト（Ctrl＋v）し、その内容をターミナルにペースト（Ctrl＋Shift＋v）できます。


# シミュレーションの準備
- 必要となる関連パッケージを予めインストールしておきます。
```
$ sudo apt-get update
$ sudo apt-get upgrade -y
$ sudo apt install ros-foxy-gazebo-* ros-foxy-cartographer ros-foxy-cartographer-ros ros-foxy-navigation2 ros-foxy-nav2-bringup -y
```
    - Gazebo（Gazebo11）
    - Cartographer
    - Navigation2



- TurtleBot3用のパッケージをインストールします。
```
$ source ~/.bashrc
$ sudo apt install ros-foxy-dynamixel-sdk ros-foxy-turtlebot3-msgs ros-foxy-turtlebot3 -y
```

- 1つだけROSの環境変数を設定します。
```
$ export ROS_DOMAIN_ID=30
```
    - 毎回、設定するのが面倒なら、.bashrcに書き込んでおきましょう。
```
$ echo 'export ROS_DOMAIN_ID=30' >> ~/.bashrc
$ source ~/.bashrc（←設定した直後のみ）
```

- TurtleBot3のシミュレーション用のパッケージをインストールします。
```
$ mkdir -p ~/colcon_ws/src
$ cd ~/colcon_ws/src/
$ git clone -b foxy-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
$ cd ~/colcon_ws
$ colcon build --symlink-install
```

- TurtleBot3はburgerとwaffleがあるので、ロボットのモデルとしてburgerと設定しておきます。
```
$ export TURTLEBOT3_MODEL=burger
```

- 動作確認の意味で、empty_world（何も無い世界）を起動してみます。
```
$ ros2 launch turtlebot3_gazebo empty_world.launch.py
```
    - 何も無い世界でも5分くらい掛かります。
    - マウスの左ボタンを押しながらドラッグすると、視点を平行移動できます。
    - マウスの左ボタンとキーボードのShiftボタンを押しながらドラッグすると、視点を回転移動できます。
    - マウスの左ボタンを押しながらドラッグすると、視点を近づけたり遠ざけたりできます。
    - 「Ctrl＋c」で終了できます。

- キーボードで操作することができます。
```
$ ros2 run turtlebot3_teleop teleop_keyboard
```
    - w：前進
    - x：後進
    - s：停止（※スペースキーでもOK！）
    - a：左回転
    - d：右回転

- 次にTurtleBot3 Worldを試してみましょう。
```
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
や
```

- Gazeboの物理演算エンジンにより、仮想的に画像を取得したり、障害物との衝突判定を行ったり、リアルロボット（実機）に近いシミュレーションを実施することができます。
- センサーから取得される仮想的なデータは可視化ツール「RViz」で確認することができます。
```
$ rviz2
```

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
