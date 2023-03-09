# ROSノード
- ROSコマンドでなくROSノードを用いた出版・講読を行います。

## 準備
- GUIのROSツールを使用するので、環境変数DISPLAYが設定されていることを確認する。設定されていないと、ホストOSにGUIの表示を依頼できない。
    ```
    $ echo $DISPLAY
    ```
- 必要なパッケージをインストールしておく。
    ```
    $ docker container exec -it ros-cui /bin/bash
    $ sudo apt update
    $ sudo apt install -y ros-foxy-demo-nodes-cpp
    $ sudo apt install -y ros-foxy-demo-nodes-py
    $ sudo apt install -y ros-foxy-rqt-graph
    ```
- アクセラレーターをオフに設定しておく。
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
- ツール「rqt_graph」でROSノード同士の繋がりを確認します。これはCUIツールでなくGUIツールですが、Dockerの動作を確認するために敢えて使用しています。左上の更新ボタンを押すとノードが表示されます。
    ```
    $ docker container exec -it ros-cui /bin/bash
    $ source ros_entrypoint.sh
    $ ros2 run rqt_graph rqt_graph
    ```
    - 「/talker」という名前のROSノードと、「/listener」という名前のROSノードが、ROSトピック「/chatter」で繋がっていることが確認できると思います。このトピックがトピックメッセージ型の情報（文字列や数字など）を伝達することで、2つのノード（機能）が連携することができます。
    - 閉じるボタンか「Ctrlキー」＋「cキー」で終了することができます。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
