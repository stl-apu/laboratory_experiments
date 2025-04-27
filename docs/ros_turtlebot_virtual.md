# TurtleBot（virtual robot）
TurtleBot本体（実機）でなく、リモートPC上（シミュレーター）で実行します。Gazebo（ガゼボ）というソフトウェアを用いたシミュレーションです。基本的に、ROS 1では旧Gazebo（Gazebo Classic）を使用し、ROS 2では新Gazebo（Gazebo Ignition）を使用します。また、通常はROS 2 Humbleをオススメしますが、Gazeboを用いる場合はROS 2 Jazzyをオススメします。

## Dockerコンテナーの作成
GUIに対応したROS用のDockerコンテナーを作成します。コンテナー中でなく、PC上で実行してください。
```
$ docker container run -p 6080:80 --name ros-gui --shm-size=512m tiryoh/ros2-desktop-vnc:jazzy
```

PC上でウェブブラウザーを開き、URL「[http://localhost:6080/](http://localhost:6080/)」でアクセスします。ボタン「接続」を押すと、デスクトップが表示されます。

ターミナルは、デスクトップにあるTerminatorなどを使います。

コピー＆ペーストは左端のcontrol barのClipboardを使用します。Clipboardにペースト（Ctrl＋v）し、その内容をターミナルにペースト（Ctrl＋Shift＋v）できます。

<span style="color: #CC0066;">コンテナーを停止する時は、念のためログアウトしてから停止しましょう。</span>コンテナーを再起動した際、「サーバーへの接続に失敗しました」というエラーが発生する可能性があるので…。発生してしまった場合は、1度、停止→再起動してみてください。それでもダメなら、コンテナーを削除し、再度、コンテナーを作成してください。

## シミュレーションの準備
とりあえず、アップデートを確認し、更新しておきます。
```
$ sudo apt update
$ sudo apt upgrade -y
```

まず、新Gazebo（Gazebo Ignition）をインストールします。
```
$ sudo apt install ros-${ROS_DISTRO}-ros-gz -y
```

Gazegoが起動するかを確認しておきます。ウィンドウが1つ開けばOKです。✕ボタンで閉じておきます。
```
$ ros2 launch ros_gz_sim gz_sim.launch.py
```


TurtleBot 3用のパッケージをインストールします。
```
$ source /opt/ros/jazzy/setup.bash
$ mkdir -p ~/turtlebot3_ws/src
$ cd ~/turtlebot3_ws/src/
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/DynamixelSDK.git
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3.git
$ cd ~/turtlebot3_ws
$ colcon build --symlink-install
$ echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
$ source ~/.bashrc
```

TurtleBot 3のシミュレーション用のパッケージをインストールします。
```
$ cd ~/turtlebot3_ws/src/
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
$ cd ~/turtlebot3_ws && colcon build --symlink-install
```

ROSの環境変数を設定します。ROS_DOMAIN_IDは0～65535の中で好きな数字を選んで設定してください（標準値は30です）。また、TurtleBot 3はburgerとwaffleがあるので、ロボットのモデルとしてburgerと設定しておきます。
```
$ export ROS_DOMAIN_ID=30
$ export TURTLEBOT3_MODEL=burger
```

毎回、設定するのが面倒なら、設定ファイル「.bashrc」に書き込んでおきましょう。
```
$ echo 'export ROS_DOMAIN_ID=30' >> ~/.bashrc
$ echo 'export TURTLEBOT3_MODEL=burger' >> ~/.bashrc
$ source ~/.bashrc
```

## シミュレーションの実行
まず、動作確認の意味で、empty_world（何も無い世界）を起動してみます。何も無い世界でも、起動するまでに5分くらい掛かります。
```
$ ros2 launch turtlebot3_gazebo empty_world.launch.py
```

Gazeboの操作方法は、
- マウスの左ボタンを押しながらドラッグすると、視点を並進移動できます。
- マウスの右ボタンを押しながらドラッグすると、視点を近づけたり遠ざけたりできます。スクロールホイールの回転でも変更できます。
- マウスの左ボタンとキーボードのShiftボタンを押しながらドラッグすると、視点を回転移動できます。スクロールホイールのドラッグでも変更できます。
- 「Ctrl＋c」で終了できます。

TurtleSimと同様にキーボードで操作することができます。
```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

操作方法は、
- w：前進
- x：後進
- s：停止（スペースキーでもOK！）
- a：左回転
- d：右回転

empty_worldを終了し、次にturtlebot3_worldを試してみましょう。
```
$ ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Gazeboの物理演算エンジンにより、仮想的に画像を取得したり、障害物との衝突を判定したり、リアルロボット（実機）に近いシミュレーションを実施することができます。

各種センサーで取得される仮想的なデータは可視化ツール「RViz」で確認することができます。
```
$ rviz2
```

RVizの操作方法は、
- 「Global Options」→「Fixed Frame」をbase_linkやbase_scanに変更します。
- ボタン「add」をクリックし、「By topic」を選択するとトピック「/scan（LaserScan）」が選択できるので、追加します。
- マウスの左ボタンを押しながらドラッグすると、視点を回転移動できます。
- マウスの右ボタンを押しながらドラッグすると、視点を近づけたり遠ざけたりできます。スクロールホイールの回転でも変更できます。
- マウスの左ボタンとキーボードのShiftボタンを押しながらドラッグすると、視点を並進移動できます。スクロールホイールのドラッグでも変更できます。

点群（点の集まり）が表示されない場合は左列「Displays」の「LaserScan」→「Topic」→「Reliability Policy」をBest Effortに変更してみましょう。

点のスタイル（PointsやSpheresなど）やサイズ、カラーなどを変更できるので、見やすいように変更してみましょう。

RVizの設定を保存しておきたい人は終了する前に「Files」→「Save Config」を実行しておきましょう。


## 関連パッケージのインストール
次に、必要となる関連パッケージを予めインストールしておきます。

CartographerとNavigation2
```
$ sudo apt update
$ sudo apt install ros-jazzy-cartographer ros-jazzy-cartographer-ros -y
$ sudo apt install ros-jazzy-navigation2 ros-jazzy-nav2-bringup -y
```


## 参考：Gazeboの機能
Gazeboの基本機能はGazebo Tutorialsを確認してみてください。例えば、球などの基本物体（プリミティブ物体）やTurtleBot以外のロボットを追加することができます。人間を追加することもできるようです。

また、turtlebot3_worldの他にも様々なworldがありますので、遊んでみてください。

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
