# ROSパッケージ
第1週はロボット用のプログラムを作成するために必要不可欠なDockerとGitHubの使い方を学びました。そして、ROSトピックやROSノードといったROSの基本的な用語を学びました。

第2週はROSノードを自分で作成します。

ROSノードはROSパッケージの中に入れます。複数のROSノードを入れることができ、複数のROSノードを連携させることで、大規模・複雑な機能を実現することができます。

## ROSパッケージの作成
ROSパッケージはディレクトリー「src」の中に作成します。今回は「laboratory_experiments_2024」というメタパッケージの中に作成します。

```
$ docker container exec -it ros-cui /bin/bash
$ source /ros_entrypoint.sh
$ cd ~/colcon_ws/src/laboratory_experiments_2024
```

メタパッケージは複数のROSパッケージをまとめたものです。ロボットは複数の機能を組み合わせて使用することが多いので、多くのメタパッケージが使用されています。

下記のコマンドでパッケージを作成します。パッケージ名は「practice_package」とし、個人で練習するためのROSパッケージとして使用していきます。

```
《記法》
$ ros2 pkg create パッケージ名 --build-type ament_python
《実例》
$ ros2 pkg create practice_package --build-type ament_python
```

ディレクトリー「practice_package」の中に3つのファイルと3つのディレクトリーが作成されます。コマンド`ls`で確認してみます。

```
$ cd practice_package
$ ls
setup.py
setup.cfg
package.xml
パッケージ名のディレクトリー
resourceディレクトリー
testディレクトリー
```

Pythonファイル「setup.py」を開き、エントリーポイントを指定します。エントリーポイントはROSノードのプログラムの開始点で、通常はmain関数を指定します。

```
$ nano setup.py
```

2つのROSノード（subscriberとpublisher）を作成する予定なので、下記の通り追記しておきます。entry_pointsという項目があるので、その中に2行、追記します。なお、「：」は省略記号ですので、コピーしないで大丈夫です。

```
：
entry_points={
    'console_scripts': [
        'practice_subscriber_node = practice_package.practice_subscriber_node:main',
        'practice_publisher_node = practice_package.practice_publisher_node:main',
    ],
},
：
```

左辺（practice_subscriber_nodeなど）がノード名になります。同じ名前のノードは1つしか存在できないことに注意しましょう。

xmlファイル「package.xml」を開き、Pythonプログラムの中でインポートするモジュールを事前に記述しておきます。

```
$ nano package.xml
```

「rclpy」と「std_msgs」という2つのモジュールを追記しておきます。タグ「package」があるので、その間に2行、追記します。

```
<package format="3">
    ：
    <exec_depend>rclpy</exec_depend>
    <exec_depend>std_msgs</exec_depend>
    ：
</package>
```

[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
