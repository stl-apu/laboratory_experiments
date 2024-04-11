# Dockerの設定（Windows）

## コンピューターの設定
「タスクマネージャー」を起動し、［パフォーマンス］→［CPU］から［仮想化：有効］となっていることを確認します。有効にする方法はコンピューターのメーカーや機種によって異なるので、自分で調べてください。

「コントロールパネル」を起動し、［プログラム］→［Windowsの機能の有効化または無効化］を開き、［Hyper-V］と［Linux用Windowsサブシステム（Windows Subsystem for Linux）］にチェックが入っていることを確認します。

## ソフトウェアのインストール
DockerのアカウントでDocker Hubにアクセスします。

→[https://hub.docker.com/](https://hub.docker.com/)

Docker Desktop for Windowsをインストールします。

→[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

Docker Desktop.exeを起動し、IDとPasswordを入力します。

コマンドプロンプトを起動し、コマンド`docker`が実行できることを確認します。
```
> docker version
```

## GUIの設定
GUIを使用するためにVcXsrvをインストールします。

→[https://sourceforge.net/projects/vcxsrv/](https://sourceforge.net/projects/vcxsrv/)

ホストOSのIPアドレスを確認します。Wi-FiなどのIPv4アドレスを確認しましょう。
```
> ipconfig
```

GUIを使用するためにX11サーバーの設定を変更します。VcXsrvのインストール先（/Program Files/VcXsrv）にあるX0.hostsファイルに、X Window Systemの転送を許可するホストOSのIPアドレスを記入し、保存します。注意点として、ネットワーク環境によってIPアドレスは変化するので、大学以外で実験を実施する場合は必要に応じて修正・追記してください。
```
localhost
ホストOSのIPアドレス（←追記！）
```

コンピューターを再起動します。

VcXsrv（X11サーバー）をソフトウェア「XLanuch」で起動します。「Multiple windows」を選択します。「Display number」は「-1」でOKです。「Start no client」を選択します。「Native opengl」のチェックを外します。システムトレイ（通知領域）にXLanuchのアイコンが表示されます。

<span style="color: #CC0066;">もし上記の設定でGUIが表示されない場合は実験中だけ「Disable access control」にチェックを入れるようにしましょう。</span>

[このページのトップへ](#)

[Dockerページへ](https://stl-apu.github.io/laboratory_experiments/docker)

[実験用サイトのトップへ](https://stl-apu.github.io/laboratory_experiments/)
