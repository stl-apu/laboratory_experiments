# ROSのビルド
ROSプログラム（ソースプログラム）をビルドし、ROSノード（オブジェクトプログラム）として実行できるようにします。

## プログラムのビルド
ツール「colcon（コルコン）」を用いてビルドします。

ワークスペース直下で行います。

```
$ cd ~/colcon_ws
$ colcon build
```

ビルドに成功した後は自作ワークスペースの設定をROSシステム全体に反映させます。

```
$ source ~/colcon_ws/install/setup.bash
```


### プログラムの再ビルド
参考情報ですので、実行する必要はありません。覚えておいてください。

今後、演習や課題を進める中で、ビルドに失敗することがあると思います。上手くビルドできなかった時は1度、ディレクトリー「colcon_ws」にある3つのディレクトリー（build、install、そしてlog）を削除し、再ビルドしてみましょう。それでもダメだった場合は下記の通りオプション付きでビルドしてみましょう。

```
$ colcon build --cmake-clean-cache
```


## プログラムの実行
ターミナルを3つ使用しますので、ターミナルを3つ開いてください。

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
