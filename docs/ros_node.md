# ROSノード
- ROSコマンドでなくROSノードを用いた出版・講読を行います。

## 確認
- GUIのROSツールを使用するので、環境変数DISPLAYが設定されていることを確認する。設定されていないと、ホストOSにGUIの表示を依頼できない。
  ```
  $ echo $DISPLAY
  ```

## 1つ目：ROSマスターの起動
- roscoreを起動します。
  ```
  $ docker exec -it ubuntu /bin/bash
  $ source ~/catkin_ws/devel/setup.bash
  $ roscore
  ```

## 2つ目：ROSトピックの購読
- listenerを起動します。
  ```
  $ docker exec -it ubuntu /bin/bash
  $ source ~/catkin_ws/devel/setup.bash
  $ rosrun advanced_experiment listener
  ```

## 3つ目：ROSトピックの出版
- talkerを起動します。
  ```
  $ docker exec -it ubuntu /bin/bash
  $ source ~/catkin_ws/devel/setup.bash
  $ rosrun advanced_experiment talker
  ```

## 4つ目：ROSノードの確認  
- ツール「rqt_graph」でROSノード同士の繋がりを確認します。
  ```
  $ docker exec -it ubuntu /bin/bash
  $ source ~/catkin_ws/devel/setup.bash
  ```
  ```
  《正式な方法》
  $ rosrun rqt_graph rqt_graph
  《簡便な方法》
  $ rqt_graph
  ```
- 「/talker」という名前のROSノードと、「/listener」という名前のROSノードが、ROSトピック「/chatter」で繋がっていることが確認できると思います。このトピックが文字情報を伝達することで、2つのノードが連携することができます。
- 閉じるボタンか「Ctrlキー」＋「cキー」でrqt_graphを停止することができます。

[このページのトップへ](#)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
