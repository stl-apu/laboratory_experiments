# Dockerの設定（Mac）

## ソフトウェアのインストール
DockerのアカウントでDocker Hubにサインインできることを確認します。

→[https://hub.docker.com/](https://hub.docker.com/)

Docker Desktop for Macをインストールします。

→[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

Docker.appを起動し、アカウント情報（IDとpassword）を入力します。

ターミナルを起動し、コマンド`docker`が実行できることを確認します。clientやserverの情報が表示されると思います。
```
% docker version
```

## GUIの設定
MacでX Window Systemを利用するためにXQuartzをインストールします。

→[https://www.xquartz.org/](https://www.xquartz.org/)

XQuartzを起動し、［設定］を選択します。そして、［セキュリティ］の［ネットワーク・クライアントからの接続を許可］にチェックを入れます。

コンピューターを再起動します（再起動後、XQuartzは停止した状態になります）。

アクセス制限を確認し、localhostが含まれているかどうかを確認します。含まれていなかった場合はlocalhostを許可し、再確認します。
```
% xhost

↓含まれていなかったら（何も表示されなかったら）

% xhost +localhost
% xhost
INET:localhost
INET6:localhost
```

改めてXQuartzを起動します。第2週や第3週の実験を実施する前にもXQuartzを起動する必要があります。覚えておいてください。

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
