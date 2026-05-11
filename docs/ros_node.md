# ROS node（ノード）
ROSコマンドでなくROSノードを用いて出版・講読を行ってみます。

## 準備
Docker、そしてDockerコンテナーが起動していることを確認した後、Dockerコンテナーの中に入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

ROSの環境を設定します。
```
$ source ros_entrypoint.sh
```

必要なパッケージをインストールします。
```
$ sudo apt update
$ sudo apt install ros-humble-demo-nodes-cpp -y
$ sudo apt install ros-humble-demo-nodes-py -y
$ sudo apt install ros-humble-rqt-graph -y
```

余計なトラブルを避けるため、アクセラレーターをオフに設定しておきます。下記2行のコマンドを1度実行するだけで、以後、自動的に設定されます。
```
$ echo "LIBGL_ALWAYS_INDIRECT=1" >> ~/.bashrc
$ source ~/.bashrc
```

## 1つ目：ROSトピックの購読
ROSノード「listener／リスナー」を起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run demo_nodes_cpp listener
```

## 2つ目：ROSトピックの出版
ROSノード「talker／トーカー」を起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run demo_nodes_py talker
```

## 3つ目：ROSノードの確認  
ROSツール「rqt_graph」でROSノード同士の繋がりを確認します。このツールはCUIツールでなくGUIツールになりますが、非常に多用されるツールですし、Dockerの動作を確認する意味も含めて使用します。

下記のコマンドを実行してください。実行後、左上の更新ボタンを押すとノードが表示されます。ROSノード「/talker」とROSノード「/listener」がROSトピック「/chatter」で繋がっていると思います。このように、トピックがトピックメッセージ型の情報（文字列や数字など）を伝達することで、2つのノード（機能）が連携できるようになります。終了する時は閉じるボタンか「Ctrl＋c」を押します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run rqt_graph rqt_graph
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
