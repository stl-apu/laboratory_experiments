# ROSトピック
- ROSの基本となるトピック通信について学びます。
- 3つのターミナルを使用し、コマンドを用いてトピック通信を体験します。
- 2つ目のターミナルからトピックを出版（publish）し、1つ目のターミナルで購読（subscribe）します。

## 1つ目：ROSトピックの購読
- Dockerコンテナーに入ります。
```
$ docker container exec -it ros-cui /bin/bash
```
- ROSの環境を設定します。
```
$ source ros_entrypoint.sh
```
- トピック「/turtle1/cmd_vel」を購読します。この時点では何も出版されていないので、何も表示されません。
```
$ ros2 topic echo /turtle1/cmd_vel
```
    - 「Ctrl＋c」で終了できますが、このページの内容が終わるまで待ちましょう。

## 2つ目：ROSトピックの出版
- 別のターミナルを開きます。
- Dockerコンテナーに入り、ROSの環境を設定します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
```
- トピック「/turtle1/cmd_vel」を出版します。出版すると、1つ目のターミナルに移動量が表示されます。
```
$ ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 3.14}}'
```  
    - 前半の3つの値で並進移動量（1つ目の値で前進・後進）を、後半の3つの値で回転移動量（3つ目の値で進行方向）を指定します。
    - 反時計回りが正になるので注意しましょう。
- 上記のコマンドでは、毎回「Ctrl＋c」で終了する必要があります。下記のようにオプションonceを付けることで、１回出版した後に自動的で終了するようにできます。
```
$ ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist '{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 3.14}}'
```
    - ちなみに、一定間隔で出版したい場合はオプションrateを使用します。

## 3つ目：ROSトピックの確認
- 別のターミナルを開きます。
- Dockerコンテナーに入り、ROSの環境を設定します。
```
$ docker container exec -it ros-cui /bin/bash
$ source ros_entrypoint.sh
```
- 利用可能なトピックの一覧を確認します。トピック「/rosout」などが存在していると思います。
```
$ ros2 topic list
```
- トピック「/turtle1/cmd_vel」のトピックメッセージ型を確認します。
```
$ ros2 topic info /turtle1/cmd_vel
```
    - 「geometry_msgs/msg/Twist」という型であることが分かります。ROS2にはintなどの単純な型（std_msgs/msg/Int32）も存在しますが、ROSはロボット用のソフトウェアなので、いくつかの変数がまとまった構造体のような型を利用することが多いです。
- トピックメッセージ型「geometry_msgs/msg/Twist」を構成する要素を確認してみます。2つの3次元ベクトル（linearとangular）で構成されており、それぞれの値はfloatであることが分かります。  
```
$ ros2 interface show geometry_msgs/msg/Twist
```

以上です。動作中のコマンドを終了しましょう。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
