# Dockerの使い方

## コンテナーの操作

### コンテナーの起動
- コンテナーを起動する。
    ```
    《記法》
    $ docker container start コンテナー名
    《実例》
    $ docker container start ros-cui
    ```

### コンテナーの停止
- コンテナーをコマンド`exit`で抜けてから、コンテナーを停止する。
    ```
    # exit
    ```
    ```
    《記法》
    $ docker container stop コンテナー名
    《実例》
    $ docker container stop ros-cui
    ```

### コンテナーの削除
- コンテナーを作り直す場合は、まず、コマンド`container ls`でコンテナーIDを確認する。そして、コンテナーを停止し、コマンド`container rm`で削除する。
    ```
    $ docker container ls -a --no-trunc
    ```
    ```
    《記法》
    $ docker container rm コンテナーID
    《実例》
    $ docker container rm 81ae65b0ac4296538f66d4037b18d36a2cc530b926eb0c9d3bce4a5a622ef003
    ```


[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
