# RaspberryPi

## 製品名

* RaspberryPi Model B+

## 仕様

* CPU:700 MHz / ARM1176JZF-Sコア\(ARM11ファミリ\)
* GPU:Broadcom VideoCore IV,OpenGL ES 2.0, 1080p 30fps H.264/MPEG-4 AVC high-profile デコーダー
* メモリ\(SDRAM\):512MB
* USB 2.0 ポート:4 \(統合USBハブ\)
* 映像出力:HDMI\(rev1.3&1.4\),コンポジットビデオ\(3.5mm4極ジャック\)
* 音声出力:HDMI,3.5mm4極ジャック
* ストレージ:microSDメモリーカードスロット\(SDIO対応\)
* ネットワーク:10/100Mbpsイーサネット
* カメラコネクタ:15ピン MIPIカメラシリアルインターフェーズ\(CSI-2\) コネクタ搭載
* ディスプレイコネクタ:Display Serial Interface\(DSI\) 15ピンフラットケーブルコネクタ
* 電源ソース:5V / USB Micro-Bコネクタ または GPIOコネクタ
* 寸法:85mm × 56mm
* OS:Debian, Fedora, Arch Linux

## スタートアップ

### 1.パーテーションが存在するSDカードの初期化

1. コマンドプロンプトで「diskpart」を実行
2. 「list disk」で初期化するSDカードのディスク番号を調べる
3. 「select disk \[ディスク番号\]」を選択する
4. 「list partition」でパーテーションを確認して選択している番号が正しいことを確認
5. 「clean」で初期化する

   ![clean](img/cdcard-clean.png)

### 2.SDカードをフォーマットする

1. [SD/SDHC/SHXC用SDメモリカードフォーマッター5.0](https://1drv.ms/u/s!AtZZJevIaEATgvwe7Z7nzBiXVOm58w?e=nedXd8)を使用

   ![SdCard-Format](img/SdCardFormatter_01.png)

   ※基本は上書きフォーマットで実行する

   [クイックフォーマットと上書きフォーマットの違い](https://www.sdcard.org/ja/downloads-2/formatter-2/faq/#faq13)

### 3.SDカードへCentOSのimgファイルを書き込む

1. [CensOSのイメージファイル](https://buildlogs.centos.org/centos/7/isos/armhfp/CentOS-Userland-7-armv7hl-Minimal-1611-test-RaspberryPi3.img.xz)をダウンロードして7zipなどで展開する
2. Win32DiskImagerを使用して、imgファイルをSDカードに書き込む

   ![Win32DiskImager](img/Win32DiskImager_01.png)

### 4.起動直後

1. 初期ログイン

   centos-rpi3 login:root
   Password:centos

2. キーボード設定

   1. 109日本語レイアウトのキーボード設定

      ```sh
      localectl set-keymap jp106
      localectl set-keymap jp-OADG109A
      localectl set-locale LANG=ja_JP.utf8
      ```

   2. localectlで確認(以下のような表示がされていれば完了)

      ```sh
      localctl

      System Locale: LANG=ja_JP.utf8
         VC Keymap: jp-OADG109A
         X11 Layout: jp
         X11 Model: jp106
      X11 Options: terminate:ctrl_alt_bksp
      ```

3. タイムゾーン設定

   ```sh
   timedatectl set-timezone Asia/Tokyo
   ```

### 5.起動後のネットワーク設定

1. ネットワーク設定ファイルの変更

   1. 以下のファイルをviで開く

      ```sh
      $vi /etc/sysconfig/network-scripts/ifcfg-eth0
      ```

2. ifcfg-eth0に以下の内容を追記する

   ```sh
   TYPE="Ethernet"
   BOOTPROTO="static"
   NM_CONTROLLED="yes"
   DEFROUTE="yes"
   NAME="eth0"
   UUID="294c5f77-b5f2-42f8-ab6a-784dd6cffeea"
   ONBOOT="yes"
   (↓以降を追記)
   IPADDR="192.168.3.21"
   NETMASK="255.255.255.0"
   GATEWAY="192.168.3.1"
   DNS1="192.168.3.1"
   DNS2="8.8.8.8"
   ```

   [esc] → wq!で保存

3. ネットワーク再起動

   ```sh
   $nmcli connection down eth0; nmcli connection up eth0
   ```

      pingで導通確認を行う。速度が安定しない場合はこの時点でrebootすること。
      ここまでくれば、あとはクライアント側からTeraTermで接続するのでラズパイ本体からディスプレイの出力は無くして良い。

### 6.Rootパーテーションのサイズ拡張

   1. 現在の状態を確認(1.1Gしか確保できていない)

      ```sh
      $df -h

      ファイルシス   サイズ  使用  残り 使用% マウント位置
      /dev/root        2.0G  762M  1.1G   42% /
      devtmpfs         459M     0  459M    0% /dev
      tmpfs            463M     0  463M    0% /dev/shm
      tmpfs            463M   12M  451M    3% /run
      tmpfs            463M     0  463M    0% /sys/fs/cgroup
      /dev/mmcblk0p1   500M   43M  457M    9% /boot
      tmpfs             93M     0   93M    0% /run/user/0
      ```

   2. 拡張前にシェルのLANG設定を変更する

      ```sh
      $export LANG="en_US.UTF-8"
      ```

   3. 変更したら以下のコマンドを実行

      ```sh
      $/usr/local/bin/rootfs-expand
      ```

   4. 実行後の確認

      ```sh
      $df -h

      Filesystem      Size  Used Avail Use% Mounted on
      /dev/root        27G  767M   25G   3% /
      devtmpfs        459M     0  459M   0% /dev
      tmpfs           463M     0  463M   0% /dev/shm
      tmpfs           463M   12M  451M   3% /run
      tmpfs           463M     0  463M   0% /sys/fs/cgroup
      /dev/mmcblk0p1  500M   43M  457M   9% /boot
      tmpfs            93M     0   93M   0% /run/user/0
      ```

   5. LANG設定を元に戻す

      ```sh
      $export LANG="ja_JP.UTF-8"
      ```

   6. 念のためrebootして確認

      ```sh
      $reboot
      ```

### 7.yumを使用可能にする

   1. /etc/yum.repo.d/CentOS-armhfp-kernel.repoを編集する

      ```sh
      $cd /etc/yum.repos.d/
      $vi CentOS-armhfp-kernel.repo
      ```

      以下の通り変更する(baseurl=の箇所)

      ```sh
      [centos-kernel]
      name=CentOS Kernels for armhfp
      #baseurl=http://mirror.centos.org/altarch/7/kernel/$basearch/kernel-$kvariant
      baseurl=http://mirror.centos.org/altarch/7/kernel/$basearch/kernel-rpi2/
      enabled=1
      gpgcheck=1
      gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
            file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-AltArch-Arm32
      ```

   2. /etc/yum.repo.d/kernel.repoを編集する

      ```sh
      $cd /etc/yum.repos.d/
      $vi kernel.repo
      ```

      以下の通り変更する(baseurl=の箇所)

      ```sh
      [kernel]
      name=kernel repo for RaspberryPi 2 and 3
      #baseurl=http://mirror.centos.org/altarch/7/kernel/armhfp/kernel-rpi2/repodata/
      baseurl=http://mirror.centos.org/altarch/7/kernel/$basearch/kernel-rpi2
      gpgcheck=1
      enabled=1
      gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
            file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-AltArch-Arm32
      ```

### 8.yum updateの実行

   ```sh
   $yum update -y
   ```

   ※かなり時間がかかるので注意

### 9.ホスト名変更

   ```sh
   $hostnamectl set-hostname stockman.srv.world
   ```

### 10.epelをリポジトリに追加

   1. リポジトリの優先順位を設定するプラグインをインストール

      ```sh
      $yum -y install yum-plugin-priorities
      ```

   2. 標準リポジトリを最優先にする

      ```sh
      $sed -i -e "s/\]$/\]\npriority=1/g" /etc/yum.repos.d/CentOS-Base.repo
      ```

   3. epelをリポジトリに追加

      ```sh
      $cat > /etc/yum.repos.d/epel.repo << EOF

      [epel]
      name=Epel rebuild for armhfp
      baseurl=https://armv7.dev.centos.org/repodir/epel-pass-1/
      enabled=1
      gpgcheck=0
      EOF
      ```

### 11.パッケージインストール

1. make, gcc, gcc-c++

   ソースコンパイルを行う

   ```sh
   $yum install make gcc gcc-c++
   ```

2. bash-completion

   Tabキー補完を強化する

   ```sh
   $yum install bash-completion
   ```

### 12.wifi接続設定

   1. ファームウェアをダウンロードする

      ```sh
      $yum -y install git
      $git clone https://github.com/RPi-Distro/firmware-nonfree.git
      $mv /lib/firmware/brcm{,.org}
      $cp -R firmware-nonfree/brcm /lib/firmware/brcm
      ```

   2. rpi-updateを実行する

      ```sh
      $curl -L --output /usr/bin/rpi-update https://raw.githubusercontent.com/Hexxeh/rpi-update/master/rpi-update
      $chmod +x /usr/bin/rpi-update
      $rpi-update
      $reboot
      ```

      ToDo:vcgencmdに対してコマンドが見つからないエラーが発生するがファームウェアのアップデートは正常に行われている？

      再起動後の確認

      ```sh
      $nmcli d

      DEVICE         TYPE      STATE     CONNECTION
      eth0           ethernet  接続済み  eth0
      wlan0          wifi      切断済み  --
      p2p-dev-wlan0  wifi-p2p  切断済み  --
      lo             loopback  管理無し  --
      ```

   3. wlan0の接続パスワード設定

      1. アクセスポイント確認

         ```sh
         $nmcli d wifi
         ```

      2. 接続パスワード設定

         ```sh
         $nmcli d wifi connect <ssid> password <key>
         ```

   4. wlan0に割り当てられたIPを確認

      ``` sh
      $nmcli d show wlan0

      GENERAL.DEVICE:                         wlan0
      GENERAL.TYPE:                           wifi
      GENERAL.HWADDR:                         B8:27:EB:2D:D7:8D
      GENERAL.MTU:                            1500
      GENERAL.STATE:                          100 (接続済み)
      GENERAL.CONNECTION:                     E00EE4C99F81-2G
      GENERAL.CON-PATH:                       /org/freedesktop/NetworkManager/ActiveCo
      IP4.ADDRESS[1]:                         192.168.3.18/24
      IP4.GATEWAY:                            192.168.3.1
      IP4.ROUTE[1]:                           dst = 0.0.0.0/0, nh = 192.168.3.1, mt =
      IP4.ROUTE[2]:                           dst = 192.168.3.0/24, nh = 0.0.0.0, mt =
      IP4.DNS[1]:                             192.168.3.1
      IP6.ADDRESS[1]:                         2400:2651:ec0:9600:a781:3b74:e35f:c212/6
      IP6.ADDRESS[2]:                         fe80::ac2:b544:fc55:fac9/64
      IP6.GATEWAY:                            fe80::e20e:e4ff:fec9:9f80
      IP6.ROUTE[1]:                           dst = 2400:2651:ec0:9600::/64, nh = ::,
      IP6.ROUTE[2]:                           dst = ::/0, nh = fe80::e20e:e4ff:fec9:9f
      IP6.ROUTE[3]:                           dst = fe80::/64, nh = ::, mt = 600
      IP6.DNS[1]:                             2400:2651:ec0:9600:1111:1111:1111:1111
      ```

   5. wlan0を固定IPアドレスにする

      ``` sh
      $vi /etc/sysconfig/network-scripts/ifcfg-E00EE4C99F81-2G
      ```

      設定ファイルを編集

      ```sh
      ESSID=E00EE4C99F81-2G
      MODE=Managed
      KEY_MGMT=WPA-PSK
      SECURITYMODE=open
      MAC_ADDRESS_RANDOMIZATION=default
      TYPE=Wireless
      PROXY_METHOD=none
      BROWSER_ONLY=none
      BOOTPROTO=dhcp
      DEFROUTE=yes
      IPV4_FAILURE_FATAL=no
      IPV6INIT=yes
      IPV6_AUTOCONF=yes
      IPV6_DEFROUTE=yes
      IPV6_FAILURE_FATAL=no
      IPV6_ADDR_GEN_MODE=stable-privacy
      NAME=E00EE4C99F81-2G
      UUID=e2b4e2ff-a099-462a-b909-0624bc20bff6
      ONBOOT=yes

      ↓

      (変更)
      BOOTPROTO=static

      (追加)
      HWADDR=B8:27:EB:2D:D7:8D
      IPADDR=192.168.3.22
      NETMASK=255.255.255.0
      GATEWAY=192.168.3.1
      DNS1=192.168.3.1
      DNS2=8.8.8.8
      ```

   6. network.serviceの起動でエラーになる件の解消

      /etc/sysconfig/networkの空ファイルを作成する

      ```sh
      $cat /etc/sysconfig/network
      cat: /etc/sysconfig/network: そのようなファイルやディレクトリはありません
      $touch /etc/sysconfig/network
      $systemctl restart network
      $systemctl status network

      ● network.service - LSB: Bring up/down networking
      Loaded: loaded (/etc/rc.d/init.d/network; bad; vendor preset: disabled)
      Active: active (exited) since 月 2021-05-03 15:49:34 JST; 2s ago
        Docs: man:systemd-sysv-generator(8)
        Process: 965 ExecStart=/etc/rc.d/init.d/network start (code=exited, status=0/SUCCESS)
      ```

   7. 一般ユーザー作成

      1. ユーザーアカウント作成

         ```sh
         $useradd <username>
         $passwd <username>
         ```

      2. rootにスイッチ可能なユーザーに追加

         ```sh
         $usermod -G wheel <username>
         $vim /etc/pam.d/su

         auth            sufficient      pam_rootok.so
         # Uncomment the following line to implicitly trust users in the "wheel" group.
         #auth           sufficient      pam_wheel.so trust use_uid
         # Uncomment the following line to require a user to be in the "wheel" group.
         auth            required        pam_wheel.so use_uid
         auth            substack        system-auth
         auth            include         postlogin
         account         sufficient      pam_succeed_if.so uid = 0 use_uid quiet
         account         include         system-auth
         password        include         system-auth
         session         include         system-auth
         session         include         postlogin
         session         optional        pam_xauth.so

         ※#auth           required        pam_wheel.so use_uidをコメント解除
         ```

      3. root権限を移譲する

         ```sh
         $visudo
         ```

         最終行に以下を追記

         ```sh
         <username>  ALL=(ALL)   ALL
         ```

### 13.Vimインストール

   1. Vimインストール

      ```sh
      $yum -y install -y vim-enhanced
      ```

   2. コマンドエイリアスを適用

      ```sh
      $vi /etc/profile

      # 最終行にエイリアス追記
      alias vi='vim'

      $source /etc/profile #変更を繁栄
      ```

   3. Vimの設定

      ```sh
      $vi /etc/vimrc
      ```

      ```Vim Script
      " vim の独自拡張機能を使う(viとの互換性をとらない)
      set nocompatible
      
      " 行番号表示
      set number
      
      " ターミナルのタイトルをセットする
      set title
      
      " 自動インデントを有効にする
      set autoindent
      
      " 構文ごとに色分け表示する
      syntax on
      
      " [ syntax on ]の場合のコメント文の色を変更する
      highlight Comment ctermfg=LightCyan
      
      " ウィンドウ幅で行を折り返す
      set wrap
      ```

### 14.httpdインストール

   1. httpdインストール

      ```sh
      $yum -y install httpd
      $rm -f /etc/httpd/conf.d/welcome.conf #ウェルカムページ削除
      ```

   2. httpdの設定

      ```sh
      $vi /etc/httpd/conf/httpd.conf
      ```

      ```Vim Script
      # 86行目：管理者アドレス指定
      ServerAdmin root@[フルコンピュータ名]
      
      # 95行目：コメント解除しサーバー名指定
      ServerName www.[フルコンピュータ名]:80
      
      # 151行目：変更
      AllowOverride All
      
      # 164行目：ディレクトリ名のみでアクセスできるファイル名を追記
      DirectoryIndex index.html index.cgi index.php

      # 最終行に追記
      # サーバーの応答ヘッダ
      ServerTokens Prod
      ```

      ```sh
      $systemctl start httpd 
      $systemctl enable httpd
      ```

   3. ファイアウォール設定

      ```sh
      $firewall-cmd --add-service=http --permanent
      $firewall-cmd --reload
      ```

   4. htmlテストページを作成して動作確認

      ```sh
      $vi /var/www/html/index.html
      ```

      以下の内容を記述してファイル保存

      ```html
      <html>
      <body>
      <div style="width: 100%; font-size: 40px; font-weight: bold; text-align: center;">
      Test Page
      </div>
      </body>
      </html>
      ```

      以下のURLにアクセスして作成したhtmlファイルが表示されるかを確認する

      http://[ホスト名]

monitarix
ftp
