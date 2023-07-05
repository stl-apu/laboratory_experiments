# Dockerの設定（Windows）

## コンピューターの設定
- 「タスクマネージャー」を起動し、［パフォーマンス］→［CPU］から［仮想化：有効］となっていることを確認する。
    - 有効にする方法はコンピューターのメーカーや機種によって異なるので、自分で調べる。
- 「コントロールパネル」を起動し、［プログラム］→［Windowsの機能の有効化または無効化］を開き、［Hyper-V］と［Linux用Windowsサブシステム（Windows Subsystem for Linux）］にチェックが入っていることを確認する。

## ソフトウェアのインストール
- DockerのアカウントでDocker Hubにアクセスする。
    - https://hub.docker.com/
- Docker Desktop for Windowsをインストールする。
    - https://docs.docker.com/get-docker/
- Docker Desktop.exeを起動し、IDとPasswordを入力する。
- コマンドプロンプトを起動し、コマンド`docker`が実行できることを確認する。
```
> docker version
```

## GUIの設定
- GUIを使用するためにVcXsrvをインストールする。
    - https://sourceforge.net/projects/vcxsrv/

- ホストOSのIPアドレスを確認する。Wi-FiなどのIPv4アドレスを確認する。
```
> ipconfig
```

- GUIを使用するためにX11サーバーの設定を変更する。VcXsrvのインストール先（/Program Files/VcXsrv）にあるX0.hostsファイルに、X Window Systemの転送を許可するホストOSのIPアドレスを記入し、保存する。
```
localhost
ホストOSのIPアドレス（←追記！）
```
    - ネットワーク環境によってIPアドレスは変化するので、大学以外で実験を実施する場合は必要に応じて修正・追記する。

- コンピューターを再起動する。

- VcXsrv（X11サーバー）をソフトウェア「XLanuch」で起動する。システムトレイ（通知領域）にXLanuchのアイコンが表示される。
    - 「Multiple windows」を選択する。「Display number」は「-1」でOK！
    - 「Start no client」を選択する。
    - 「Native opengl」のチェックを外す。

- <span style="color: #CC0066;">もし上記の設定でGUIが表示されない場合は実験中だけ「Disable access control」にチェックを入れる。</span>

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[情報科学実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
