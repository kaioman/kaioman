# D945GCLF

## 製品名

* intel デスクトップ・ボード D945GCLF

## 仕様

- [基本仕様](https://ark.intel.com/content/www/jp/ja/ark/products/42490/intel-desktop-board-d945gclf.html)

## スタートアップ

1. CentOS7のインストール

2. rootユーザーパスワード設定

3. ネットワーク設定

   インターフェース：enp1s0

   デフォルトでconnection.autoconnect=noとなっていたので再起動すると切断状態となってしまう。

   以下のコマンドで自動接続するよう設定

   ```sh
   $nmcli con mod enp1s0 connection.autoconnet "yes"
   ```

4. SSH設定

5. 一般ユーザー追加・sudo設定

6. ホスト名変更

7. システム最新化

8. リポジトリ追加

9. vimインストール・設定

10. デスクトップ環境構築

    1. GNOMEインストール

       ```sh
       $yum -y groups install "GNOME Desktop"
       ```

    2. GNOME起動確認

       ```sh
       $startx
       ```

    3. VNCインストール

       ```sh
       $yum -y install tigervnc-server
       ```

    4. VNCパスワード設定

       VNC接続させたいユーザーでログインした状態で以下のコマンド実行

       ```sh
       $vncpasswd
       Password:(任意のパスワード)
       Verify:（任意のパスワード再入力）
       Would you like to enter a view-only password (y/n)? n　（見るだけの権限が必要か？）
       ```

    5. ファイアーウォール設定でVNC接続許可

       ```sh
       $firewall-cmd --permanent --add-port=5901/tcp
       $firewall-cmd --reload
       ```

    6. VNC起動確認

       ```sh
       $vncserver :1
       ```

       この後、クライアント側からVNCビューアーで接続できるか確認する。

       <img src="img\vncLogin.png" style="zoom:100%; float:left" />
       
    7. VNCサービス登録

       設定ファイルコピー

       ```sh
       $cp -p /lib/systemd/system/vncserver@.service /etc/systemd/system/vncserver@:1.service
       $vim /etc/systemd/system/vncserver@:1.service
       ```

       設定ファイル編集

       <USER>の箇所は接続ユーザーに置き換える

       ```sh
       [Unit]
       Description=Remote desktop service (VNC)
       After=syslog.target network.target
       
       [Service]
       Type=simple
       
       # Clean any existing files in /tmp/.X11-unix environment
       ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
       ExecStart=/usr/bin/vncserver_wrapper <USER> %i
       ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'
       
       [Install]
       WantedBy=multi-user.target
       ```

       サービス登録

       ```sh
       $systemctl daemon-reload
       $systemctl start vncserver@:1.service
       $systemctl enable vncserver@:1.service
       ```

       上記実施後、rebootしてVNCクライアントから接続できるか確認する。
       
       

11. Wake On Lan設定

    1. ethtoolインストール

       ```sh
       $yum -y install ethtool
       ```

    2. ethtool設定

       ```sh
       $ethtool -s <ネットワークデバイス名> wol g
       ```

       ネットワークデバイス名は以下のコマンドで確認

       ```sh
       $nmcli -d
       ```

    3. 設定後確認

       ```sh
       $ethtool enp1s0 | grep Wake-on
           Supports Wake-on: pumbg
           Wake-on: g
       ```

    4. 設定を恒久化

       ```sh
       $nmcli connection modify enp1s0 ethernet.wake-on-lan magic
       ```

    5. クライアント側(Windows)

       nWOLをインストール。以下のように設定。

       <img src="img\nWOL-config.png" alt="nWOL-config" style="zoom:75%;float:left" />

    6. クライアント側(iOS)

       Wolowをインストール。以下のように設定。

       <img src="img\Wolow-config.png" alt="Wolow-config" style="zoom:30%;float:left" />
