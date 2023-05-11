# ROSパラメーター
- パラメーター通信についての演習を行います。
- パラメーターはノードの設定値です。
- ノードを実行した後（実行中）に値を変更したい時に使用します。実行する時の条件として設定することもできます。
- グローバル変数のように全てのノードで値を共有することができます。

## ROSパラメーターの確認
- とりあえず、turtlesimノードに属しているパラメーターを確認してみましょう。
```
$ ros2 run turtlesim turtlesim_node
```
```
$ ros2 param list
```
    - 全てのノードがuse_sim_timeを持ちます。コンピューターシステムの時間（false）とROSシステムの時間（true）のどちらを使用するかを選択するためのパラメーターです。

- パラメーターの種類と値を確認します。
```
《記法》
$ ros2 param get ノード名 パラメーター名
《実例》
$ ros2 param get /turtlesim background_b
```
    - 整数型、浮動小数点型、論理型、文字型などを使用することができます。

## ROSパラメーターの変更
- パラメーターの値を変更してみます。
```
《記法》
$ ros2 param set ノード名 パラメーター名 値
《実例》
$ ros2 param set /turtlesim background_b 51
```
    - シミュレーターの背景色が変わります。

- 同様に、background_gやbackground_rも変更してみましょう。

- 毎回パラメーターを1つ1つ変更するのは大変なので、ファイルで保存します。
```
《記法》
$ ros2 param dump ノード名
《実例》
$ ros2 param dump /turtlesim
```
    - 現在のディレクトリーにノード名のyamlファイル（例：turtlesim.yaml）が作成されます。

- 1度、turtlesimを停止し、パラメーターの値を指定しながら起動してみます。
```
《記法》
$ ros2 run パッケージ名 ノード名 --ros-args --params-file パラメーターファイル名
《実例》
$ ros2 run turtlesim turtlesim_node --ros-args --params-file ./turtlesim.yaml
```
    - 「./」は現在のディレクトリー（カレントディレクトリー）を意味します。


## ROSパラメーターの使い道
- ロボットを動かしながら値を変更できるので、狭い道に入ったら最高速度の上限を下げて安全に移動するなど、ロボットの動きを柔軟に調整することができます。


[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
