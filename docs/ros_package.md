# ROSパッケージ
- 第1週はロボット用のソフトウェアを開発するために必要不可欠なDockerとGitHubの使い方を学びました。また、ROSトピックやROSノードといったROSの基本的な用語を学びました。
- 第2週は実際にROSノードを作成します。
- ROSノードはROSパッケージの中に入れます。複数のROSノードを入れることができ、複数のROSノードが連携することで、大規模・複雑な機能を実現することができます。

## ROSパッケージの作成
- ROSパッケージはディレクトリーsrcに作成します。今回はlaboratory_experiments_2023というメタパッケージの中に作成します。
    ```
    $ cd ~/colcon_ws/src/laboratory_experiments_2023
    ```
    - メタパッケージは複数のROSパッケージをまとめたものです。ロボットは複数の機能を繋げて使用することが多いので、多くのメタパッケージが使用されています。
- 下記のコマンドでパッケージを作成します。パッケージ名はtest_packageとします。
    ```
    《記法》
    $ ros2 pkg create パッケージ名 --build-type ament_python
    《実例》
    $ ros2 pkg create assignment_package --build-type ament_python
    ```
- 3つのファイルと3つのディレクトリーが作成されます。
    ```
    $ ls
    setup.py
    setup.cfg
    package.xml
    パッケージ名のディレクトリー
    resourceディレクトリー
    testディレクトリー
    ```
- setup.pyを開き、エントリーポイントを指定します。エントリーポイントはROSノードのプログラムの開始点で、main関数を指定します。
    ```
    $ nano setup.py
    ```
    - subscriberとpublisherを作成する予定なので、下記の通り追記しておきます。
        ```
        …
        entry_points={
            'console_scripts': [
                'test_subscriber_node = laboratory_experiments_2023.test_subscriber_node:main',
                'test_publisher_node = laboratory_experiments_2023.test_publisher_node:main',
            ],
        },
        …
        ```
- package.xmlを開き、Pythonプログラムの中でインポートするモジュールを事前に記述しておきます。
    ```
    $ nano package.xml
    ```
    - rclpyとstd_msgsを追記しておきます。
        ```
        <package format="3">
            …
            <exec_depend>rclpy</exec_depend>
            <exec_depend>std_msgs</exec_depend>
            …
        </package>
        ```

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
