# Dockerの設定（Mac）

## ソフトウェアのインストール
DockerのアカウントでDocker Hubにアクセスします。

[https://hub.docker.com/](https://hub.docker.com/)

Docker Desktop for Macをインストールします。

[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

Docker.appを起動し、IDとPasswordを入力します。

ターミナルを起動し、コマンド`docker`が実行できることを確認します。
```
% docker version
```

## GUIの設定
XQuartzをインストールします。

→[https://www.xquartz.org/](https://www.xquartz.org/)

XQuartzを起動し、［環境設定］→［セキュリティ］から［ネットワーク・クライアントからの接続を許可］にチェックを入れます。

コンピューターを再起動します。

アクセス制限を確認し、localhostが含まれていることを確認します。
```
% xhost
INET:localhost
INET6:localhost
```

含まれていなかった場合はlocalhostを許可し、localhostが許可されたことを確認します。改めてXQuartzを起動します。
```
% xhost +localhost
% xhost
```

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
