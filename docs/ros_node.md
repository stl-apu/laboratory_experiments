# ROSノード
- ROSコマンドでなくROSノードを用いて出版・講読を行ってみます。

## 準備
- 最後にGUIのROSツールを使用するので、環境変数DISPLAYが設定されていることを確認しましょう。設定されていないと、ホストOSにGUIの表示を依頼できません。
```
$ echo $DISPLAY
```
    - 設定させていない場合は「Dockerの設定」を確認しましょう。
- 必要なパッケージをインストールしておきます。
```
$ docker container exec -it ros-cui /bin/bash
$ sudo apt update
$ sudo apt install ros-foxy-demo-nodes-cpp -y
$ sudo apt install ros-foxy-demo-nodes-py -y
$ sudo apt install ros-foxy-rqt* -y
```
- アクセラレーターをオフに設定しておきます。
```
$ export LIBGL_ALWAYS_INDIRECT=1
```

## 1つ目：ROSトピックの購読
- listenerを起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run demo_nodes_cpp listener
```

## 2つ目：ROSトピックの出版
- talkerを起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run demo_nodes_py talker
```

## 3つ目：ROSノードの確認  
- ツール「rqt_graph」でROSノード同士の繋がりを確認します。これはCUIツールでなくGUIツールですが、非常に多用されるツールですし、Dockerの動作を確認するために敢えて使用しています。左上の更新ボタンを押すとノードが表示されます。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run rqt_graph rqt_graph
```
    - 「/talker」という名前のROSノードと、「/listener」という名前のROSノードが、ROSトピック「/chatter」で繋がっていると思います。このように、トピックがトピックメッセージ型の情報（文字列や数字など）を伝達することで、2つのノード（機能）が連携できるようになります。
    - 閉じるボタンか「Ctrl＋c」で終了できます。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
