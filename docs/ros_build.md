# ROSのビルド
ROSのプログラム（ソースプログラム）をビルドし、ROSのノード（オブジェクトプログラム）として実行できるようにします。

## プログラムのビルド
ツール「colcon（コルコン）」を用いてビルドします。

ワークスペース直下で行います。

```
$ cd ~/colcon_ws
$ colcon build
```

上手くビルドできなかった時は1度、ディレクトリー「colcon_ws」にある3つのディレクトリー（build、install、log）を削除し、再度ビルドしてみましょう。それでもダメだった場合は下記の通りオプション付きでビルドしてみましょう。

```
$ colcon build --cmake-clean-cache
```

自作ワークスペースの設定を反映させます。

```
$ source ~/colcon_ws/install/setup.bash
```

## プログラムの実行
3つのターミナルを使用します。

### 1つ目：ROSトピックの購読
listenerを起動します。

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ source ~/colcon_ws/install/setup.bash
$ ros2 run practice_package practice_subscriber_node
```

### 2つ目：ROSトピックの出版
talkerを起動します。

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ source ~/colcon_ws/install/setup.bash
$ ros2 run practice_package practice_publisher_node
```

### 3つ目：ROSノードの確認  
ツール「rqt_graph」でROSノード同士の繋がりを確認します。

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ rqt_graph
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
