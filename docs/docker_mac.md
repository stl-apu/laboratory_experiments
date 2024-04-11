# Dockerの設定（Mac）

## ソフトウェアのインストール
DockerのアカウントでDocker Hubにアクセスできることを確認します。

→[https://hub.docker.com/](https://hub.docker.com/)

Docker Desktop for Macをインストールします。

→[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

Docker.appを起動し、アカウント情報（IDとPassword）を入力します。

ターミナルを起動し、コマンド`docker`が実行できることを確認します。ClientやServerの情報が表示されると思います。
```
% docker version
```

## GUIの設定
XQuartzをインストールします。

→[https://www.xquartz.org/](https://www.xquartz.org/)

XQuartzを起動し、［設定］を選択する。そして、［セキュリティ］の［ネットワーク・クライアントからの接続を許可］にチェックを入れます。

コンピューターを再起動します。

アクセス制限を確認し、localhostが含まれていることを確認します。
```
% xhost
INET:localhost
INET6:localhost
```

含まれていなかった場合はlocalhostを許可し、localhostが許可されたことを確認します。
```
% xhost +localhost
% xhost
```

改めてXQuartzを起動します。第2週や第3週の実験を実施する前にも起動する必要があります。覚えておきましょう。

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
