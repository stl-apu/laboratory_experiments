# Dockerの設定（Windows）

## コンピューターの設定の確認
- 「タスクマネージャー」を起動し、［パフォーマンス］→［CPU］から［仮想化：有効］となっていることを確認する。
  - 有効にする方法はコンピューターのメーカーや機種によって異なるので、自分で調べる。
- 「コントロールパネル」を起動し、［プログラム］→［Windowsの機能の有効化または無効化］を開き、［Hyper-V］と［Linux用Windowsサブシステム（Windows Subsystem for Linux）］にチェックが入っていることを確認する。

## ソフトウェアのインストール
- DockerのアカウントでDocker Hubにアクセスする。
  - https://hub.docker.com/
- Docker Desktop for Windowsをインストールする。
  - https://docs.docker.com/get-docker/
- Docker Desktop.exeを起動し、IDとPasswordを入力する。
- コマンドプロンプトを起動し、コマンドdockerが実行できることを確認する。
  ```
  > docker version
  ```

## GUIの設定
- GUIを使用するためにVcXsrvをインストールする。
  - https://sourceforge.net/projects/vcxsrv/
- ホストのIPアドレスを確認する。Wi-FiなどのIPv4アドレスを確認する。
  ```
  > ipconfig
  ```
- ホストの設定を変更する。Xmingのインストール先（/Program Files (x86)/Xming）にあるX0.hostsファイルに、X Window Systemの転送を許可するホストのIPアドレスを記入する。
  ```
  localhost
  ホストのIPアドレス（←追記！）
  ```
- コンピューターを再起動する。
- VcXsrvをソフトウェア「XLanuch」で起動する。
  - 「Native opengl」のチェックを外す。
  - 「Disable access control」にチェックを入れる。

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
