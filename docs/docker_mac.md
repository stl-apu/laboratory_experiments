# Dockerの設定（Mac）

## ソフトウェアのインストール
- DockerのアカウントでDocker Hubにアクセスする。
    - https://hub.docker.com/
- Docker Desktop for Macをインストールする。
    - https://docs.docker.com/get-docker/
- Docker.appを起動し、IDとPasswordを入力する。
- ターミナルを起動し、コマンドdockerが実行できることを確認する。
```
% docker version
```

## GUIの設定
- XQuartzをインストールする。
    - https://www.xquartz.org/
- XQuartzを起動し、［環境設定］→［セキュリティ］から［ネットワーク・クライアントからの接続を許可］にチェックを入れる。
- コンピューターを再起動する。
- アクセス制限を確認する。localhostが含まれているかを確認する。
```
% xhost
INET:localhost
INET6:localhost
```
    - 含まれていなかった場合はlocalhostを許可する。
```
% xhost +localhost
```
    - localhostが許可されたことを確認する。
```
% xhost
```
    - 再度、XQuartzを起動する。

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
