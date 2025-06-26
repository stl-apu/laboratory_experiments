# ROS topic（トピック）
ROSの基本となるトピック通信について学びます。3つのターミナルを使用し、各種ROSコマンドを用いてトピック通信を体験します。1つ目のターミナルでトピックを出版（publish）し、2つ目のターミナルで購読（subscribe）します。そして、3つ目のターミナルでトピックを確認します。

## 1つ目：ROSトピックの出版
ターミナルを開きます。

Dockerコンテナーに入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

ROSの環境を設定します。
```
$ source ros_entrypoint.sh
```

トピック「/turtle1/cmd_vel」を出版してみます。「turtle1」は1台目の亀ロボットを意味し、「cmd_vel」は速度コマンドを意味します。前半の3つの値で並進移動量（1つ目の値で前進・後進）を、後半の3つの値で回転移動量（3つ目の値で進行方向）を指定します。回転移動については反時計回りが正になるので注意しましょう。
```
$ ros2 topic pub --once -w 0 /turtle1/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 3.14}}'
```  

オプションonceを削除することで、連続して出版するようになります。終了するときは「Ctrl＋c」を押しますが、このあと購読するので、しばらくそのままにしておいてください。ちなみに、一定間隔で出版したい場合はオプションrateを使用します。
```
$ ros2 topic -w 0 pub /turtle1/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 3.14}}'
```

## 2つ目：ROSトピックの購読
別のターミナルを開きます。

Dockerコンテナーに入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

ROSの環境を設定します。
```
$ source ros_entrypoint.sh
```

トピック「/turtle1/cmd_vel」を購読してみます。移動量が表示されると思います。終了するときは「Ctrl＋c」を押しますが、このあとトピックについて学習するため、しばらくそのままにしておいてください。
```
$ ros2 topic echo /turtle1/cmd_vel
```

## 3つ目：ROSトピックの確認
別のターミナルを開きます。

Dockerコンテナーに入ります。
```
$ docker container exec -it ros-cui /bin/bash
```

ROSの環境を設定します。
```
$ source ros_entrypoint.sh
```

利用可能なトピックの一覧を確認してみます。トピック「/rosout」などが存在すると思います。
```
$ ros2 topic list
```

トピック「/turtle1/cmd_vel」のトピックメッセージ型を確認してみます。「geometry_msgs/msg/Twist」という型であることが分かります。ROS2にはintなどの単純な型（std_msgs/msg/Int32）も存在しますが、ROSはロボット用のソフトウェアなので、いくつかの変数がまとまった構造体のような型を利用することが多いです。
```
$ ros2 topic info /turtle1/cmd_vel
```

トピックメッセージ型「geometry_msgs/msg/Twist」を構成する要素を確認してみます。2つの3次元ベクトル（linearとangular）で構成されており、それぞれの値はfloatであることが分かります。
```
$ ros2 interface show geometry_msgs/msg/Twist
```

___

以上です。実行中のコマンドを全て終了しましょう。

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
