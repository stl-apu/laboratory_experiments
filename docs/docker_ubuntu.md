# Dockerの設定（Ubuntu）

## ソフトウェアのインストール
- Dockerをインストールします。
    ```
    $ sudo apt update
    $ sudo apt install docker.io -y
    ```
- コマンドdockerが実行できることを確認します。
    ```
    $ docker version
    ```

## ソケット通信の設定
- グループ「docker」にユーザーを追加します。
    ```
    $ sudo gpasswd -a $USER docker
    ```
- ソケット通信に関するファイルの所有者をグループ「docker」に変更します。
    ```
    $ sudo chgrp docker /var/run/docker.sock
    ```
- サービスDockerを再起動します。
    ```
    $ sudo service docker restart
    ```
- コンピューターを再起動します。
    ```
    $ sudo reboot
    ```
- 接続が許可されているクライアント（Docker Container）として「LOCAL:」があることを確認します。
    ```
    $ xhost
    access control enabled, only authorized clients can connect
    LOCAL:
    SI:localuser:ユーザー名
    ```
    - 「LOCAL:」が無い場合は追加します。
        ```
        $ xhost +local:
        ```
- 改めてDockerサービスを起動します。
    ```
    $ sudo service docker restart
    ```

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
