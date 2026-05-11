# ROSのビルド
ROSプログラム（ソースプログラム）をビルドし、ROSノード（オブジェクトプログラム）として実行できるようにします。

## プログラムのビルド
ツール「colcon（コルコン）」を用いてビルドします。

ビルドはワークスペース直下で行います。コマンド`cd`で移動してから、コマンド`colcon build`でビルドします。

```
$ cd ~/colcon_ws
$ colcon build
```

<span style="color: #CC0066;">上記のコマンドを実行した後の出力は入念に確認してください。</span>エラー（Fatal、Error、Warn）が発生している可能性がありますし、逆に何もビルドできていない可能性もあります。グループの他のメンバーの出力と比較してみてください。

ビルドに成功した後は自作ワークスペースの設定をROSシステム全体に反映させます。このコマンドを実行することで、自作パッケージ「practice_package」がDockerコンテナー内で使用できるようになります。

```
$ source ~/colcon_ws/install/setup.bash
```


### プログラムの再ビルド
参考情報ですので、実行する必要はありません。今後のために覚えておいてください。

実験（演習や課題）を進める中で、ビルドに失敗することがあると思います。上手くビルドできなかった時は1度、ディレクトリー「colcon_ws」にある3つのディレクトリー（build、install、そしてlog）を削除し、再ビルドしてみましょう。削除はコマンドを使った方法でもマウスを使った方法でも問題ありません。それでもダメだった場合は下記の通りオプション付きでビルドしてみましょう。

```
$ colcon build --cmake-clean-cache
```


## プログラムの実行
自作パッケージの動作を確認してみましょう。ターミナルを3つ使用しますので、ターミナルを3つ開いてください。

### 1つ目：ROSトピックの購読
ROSノード「listener」を起動します。

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ source ~/colcon_ws/install/setup.bash
$ ros2 run practice_package practice_subscriber_node
```

### 2つ目：ROSトピックの出版
ROSノード「talker」を起動します。

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ source ~/colcon_ws/install/setup.bash
$ ros2 run practice_package practice_publisher_node
```

### 3つ目：ROSノードの確認  
ツール「rqt_graph」でROSノード同士の繋がりを確認します。第1週のように、トピック「/practice_topic」で2つのROSノードが繋がっていればOKです！

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ rqt_graph
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
