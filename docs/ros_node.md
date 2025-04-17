# ROSノード
ROSコマンドでなくROSノードを用いて出版・講読を行ってみます。

## 準備
Dockerコンテナーに入っていない場合は、Dockerコンテナーに入ります。
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
$ sudo apt install ros-humble-rqt* -y
```

余計なトラブルを避けるため、アクセラレーターをオフに設定しておきます。
```
$ export LIBGL_ALWAYS_INDIRECT=1
```

GUIを用いたROSツールを使用するので、Dockerクライアント（Dockerコンテナー）側で環境変数DISPLAYが設定されていることを確認しましょう。設定されていないと、Dockerホスト（ホストOS）側にGUIの表示を依頼できません。設定されている場合は「host.docker.internal:0」など、何か文字列が返ってきます。設定させていない場合はページ「Dockerの設定」を再確認しましょう。
```
$ echo $DISPLAY
```

## 1つ目：ROSトピックの購読
listenerを起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run demo_nodes_cpp listener
```

## 2つ目：ROSトピックの出版
talkerを起動します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run demo_nodes_py talker
```

## 3つ目：ROSノードの確認  
ツール「rqt_graph」でROSノード同士の繋がりを確認します。これはCUIツールでなくGUIツールになりますが、非常に多用されるツールですし、Dockerの動作を確認する意味も含めて使用しています。左上の更新ボタンを押すとノードが表示されます。ROSノード「/talker」とROSノード「/listener」がROSトピック「/chatter」で繋がっていると思います。このように、トピックがトピックメッセージ型の情報（文字列や数字など）を伝達することで、2つのノード（機能）が連携できるようになります。終了するときは閉じるボタンか「Ctrl＋c」を押します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
$ ros2 run rqt_graph rqt_graph
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
