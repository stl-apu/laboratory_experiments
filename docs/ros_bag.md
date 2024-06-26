# rosbag（ロスバッグ）
- bagファイルを作成するためのROSツールです。
- 時刻と共に、出版されたROSトピックをデータとして記録することができます。
- 同じ実験を効率的に何度も繰り返すことができます。
    - 同じ出力を完全に再現することができます。

## 記録
- 記録したいノードを立ち上げます。
```
$ ros2 launch practice_package practice_launch.py
```

- 別のターミナルを開き、トピックの一覧を確認します。
```
$ ros2 topic list
```

- 記録したいトピックを選択しながらコマンド`bag record`を実行します。オプションoでバッグファイルに名前を付けることができます。Ctrl＋cで記録を終了できます。
```
《記法》
$ ros2 bag record -o バッグファイル名 トピック名
《実例》
$ ros2 bag record -o practice_bag practice_topic
```

- bagファイルはコマンド`bag record`を実行したディレクトリーに生成されます。

- 他のノードもCtrl＋cなどで停止します。

## 確認
- コマンド`bag info`で確認します。記録したかったトピックが正しく記録されているかを確認することができます。記録の長さなども確認することができます。
```
《記法》
$ ros2 bag info バッグファイル名
《実例》
$ ros2 bag info practice_bag
```

## 再生
- コマンド`bag play`で再生します。
```
《記法》
$ ros2 bag play バッグファイル名
《実例》
$ ros2 bag play practice_bag
```
    - 別のターミナルを開き、再生中にトピックが正常に出力されていることをコマンド`topic list`やコマンド`topic echo`で確認してみましょう。
```
$ ros2 topic echo practice_topic
```

- オプションx（--rate）で再生速度の倍率を設定できます。速くすることも遅くすることもできます。再生速度を遅くすることで、ロースペックのPCでもシミュレーションを実行することができます。また、プログラムのデバッグや実験結果の分析が行いやすくなるという利点もあります。一方、再生速度を速くすることで、シミュレーション実験を高速に実施することができます。


[このページのトップへ](#)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
