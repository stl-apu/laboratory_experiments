# 実機
- Ubuntuを使用している
- 順番に体験してもらいます。

## リモートコンピューターの設定
- 無線ネットワークを学部のネットワーク「IST Wireless」から実験用のネットワーク「STL-AE」に切り替える。
    - パスワードは教員またはTAから教えてもらう。
    - 実機実験を終えたら、元に戻しておく。

- リモートコンピューターのROS_DOMAIN_IDの値を確認し、TurtleBotのROS_DOMAIN_IDを変更する。

## ファイヤーウォールの設定
- 恐らく初期設定のままではロボットと通信できないので、ファイヤーウォールの設定を変更する。
- コマンドufwをインストールする。
```
$ sudo apt -y install ufw
```
- ファイヤーウォールの状態を確認する。
```
$ sudo ufw status numbered
```
- IPアドレス指定でポートを開放する。
```
《記法》
$ sudo ufw allow from ロボットのIPアドレス
《実例》
$ sudo ufw allow from 10.0.1.12
```
- ファイヤーウォールを有効化する。
```
$ sudo ufw enable
```
- 再度、ファイヤーウォールの状態を確認する。
```
$ sudo ufw status numbered
```

[このページのトップへ](#)

[TurtleBotページへ](https://stl-apu.github.io/laboratory_experiments/ros_turtlebot)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
