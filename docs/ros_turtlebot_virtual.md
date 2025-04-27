# TurtleBot（virtual robot）
TurtleBot本体（実機）でなく、リモートPC上（シミュレーター）で実行します。Gazebo（ガゼボ）というソフトウェアを用いたシミュレーションになります。基本的に、ROS 1では旧Gazebo（Gazebo Classic）を使用し、ROS 2では新Gazebo（Gazebo Ignition）を使用します。また、通常はROS 2 Humbleをオススメしますが、Gazeboを用いる場合はROS 2 Jazzyをオススメします。よって、「ROS 2 Jazzy with Gazebo Ignition」という環境で演習を実施します。

## Dockerコンテナーの作成
GUIに対応したROS用のDockerコンテナーを作成します。コンテナー中でなく、PC上で実行してください。
```
$ docker container run -p 6080:80 --name ros-gui --shm-size=512m tiryoh/ros2-desktop-vnc:jazzy
```

PC上でウェブブラウザーを開き、URL「[http://localhost:6080/](http://localhost:6080/)」でアクセスします。ボタン「接続」を押すと、デスクトップが表示されます。

ターミナルは、デスクトップにあるTerminatorなどを使います。

コピー＆ペーストは左端のcontrol barのクリップボードを使用します。クリップボードにペースト（Ctrl＋v）し、その内容をターミナルにペースト（Ctrl＋Shift＋v）できます。

<span style="color: #CC0066;">コンテナーを停止する時は、念のためログアウトしてから停止しましょう。</span>コンテナーを再起動した際、「サーバーへの接続に失敗しました」というエラーが発生する可能性がありますので…。発生してしまった場合は、1度、コンテナーを削除し、再度、コンテナーを作成してください。

## シミュレーションの準備
とりあえず、ソフトウェアのアップデートを確認し、更新しておきます。Terminatorを開き、実行します。
```
$ sudo apt update
$ sudo apt upgrade -y
```

まず、Gazegoが起動するかを確認しておきます。ウィンドウ（dialog）が1つ開けばOKです。✕ボタンで閉じ、ウィンドウ（Gazebo Sim）も✕ボタンで閉じておきます。
```
$ ros2 launch ros_gz_sim gz_sim.launch.py
```

TurtleBot 3用のパッケージをインストールします。
```
$ mkdir -p ~/turtlebot3_ws/src && cd $_
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
- 終了したい時はターミナルで「Ctrl＋c」を押します。✕ボタンでも終了できます。

TurtleSimと同様にキーボードで操作することができます。シミュレーターが動いた状態で、別のターミナルを開き、実行します。ちなみに、Terminatorなら「Ctrl＋Shift＋t」でタブとして別のターミナルを開くことができます。
```
$ ros2 run turtlebot3_teleop teleop_keyboard
```

操作方法は、
- w：前進
- x：後進
- s：停止（スペースキーでもOK！）
- a：左回転
- d：右回転

1度empty_worldを終了し、次にturtlebot3_worldを試してみましょう。今度は壁や障害物のある世界になります。
```
$ ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Gazeboの物理演算エンジンにより、壁や障害物との衝突を判定したり、仮想的に画像や点群を取得したり、リアルロボット（実機）に近いシミュレーションを実施することができます。

各種センサーで取得される仮想的なデータは可視化ツール「RViz」で確認することができます。更に別のターミナル（3つ目）を開き、実行します。
```
$ rviz2
```

RVizの操作方法は、
- 「Global Options」→「Fixed Frame」をbase_linkやbase_scanに変更します。
- ボタン「add」をクリックし、「By topic」を選択するとトピック「/scan（LaserScan）」が選択できるので、追加します。
- マウスの左ボタンを押しながらドラッグすると、視点を回転移動できます。
- マウスの右ボタンを押しながらドラッグすると、視点を近づけたり遠ざけたりできます。スクロールホイールの回転でも変更できます。
- マウスの左ボタンとキーボードのShiftボタンを押しながらドラッグすると、視点を並進移動できます。スクロールホイールのドラッグでも変更できます。
- 終了したい時はターミナルで「Ctrl＋c」を押します。✕ボタンでも終了できます。

点群（点の集まり）が表示されない場合は左列「Displays」の「LaserScan」→「Topic」→「Reliability Policy」をBest Effortに変更してみましょう。

点のスタイル（PointsやSpheresなど）やサイズ、カラーなどを変更できるので、見やすいように変更してみましょう。

RVizの設定を保存しておきたい人は終了する前に「File」→「Save Config」を実行しておきましょう。


## 参考：関連パッケージのインストール
課題で使えそうな関連パッケージ（CartographerとNavigation2）を予めインストールしておきます。
```
$ sudo apt update
$ sudo apt install ros-jazzy-cartographer ros-jazzy-cartographer-ros -y
$ sudo apt install ros-jazzy-navigation2 ros-jazzy-nav2-bringup -y
```


## 参考：Gazeboの基本機能
Gazebo Simを用いて球などの基本物体（プリミティブ物体）を追加したり、Blender（ブレンダー）などを用いて自作したモデル（TurtleBot以外のロボット）を追加したり、世界を編集することができます。Gazebo IgnitionやGazebo SimのTutorialsが参考になると思いますので、確認してみてください。また、turtlebot3_worldの他にもworldファイルがありますので、遊んでみてください。

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
