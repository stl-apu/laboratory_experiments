# ROSのビルド
- ROSのプログラムをビルドする方法を学びつつ、ソースコードを作成するための準備を進めます。

## プログラムのビルド
- ビルドツールcolcon（コルコン）を用いてビルドします。
- ワークスペース直下で行います。
    ```
    $ cd ~/colcon_ws
    $ colcon build
    ```
- 自作ワークスペースの設定を反映させます。
    ```
    $ source ~/colcon_ws/install/setup.bash
    ```

## プログラムの実行
### 1つ目：ROSトピックの購読
- listenerを起動します。
    ```
    $ ros2 run test_package test_subscriber_node
    ```

### 2つ目：ROSトピックの出版
- talkerを起動します。
    ```
    $ docker container exec -it ros-cui /bin/bash
    $ source ros_entrypoint.sh
    $ ros2 run demo_nodes_py talker
    ```

### 3つ目：ROSノードの確認  
- ツール「rqt_graph」でROSノード同士の繋がりを確認します。
    ```
    $ rqt_graph
    ```

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
