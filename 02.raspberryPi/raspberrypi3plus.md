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

   ![clean](/02.raspberryPi/img/cdcard-clean.png)

### 1.SDカードをフォーマットする

1. [SD/SDHC/SHXC用SDメモリカードフォーマッター5.0](https://1drv.ms/u/s!AtZZJevIaEATgvwe7Z7nzBiXVOm58w?e=nedXd8)を使用

   ![SdCard-Format](/02.raspberryPi/img/SdCardFormatter_01.png)

   ※基本は上書きフォーマットで実行する

   [クイックフォーマットと上書きフォーマットの違い](https://www.sdcard.org/ja/downloads-2/formatter-2/faq/#faq13)

### 2.SDカードへCentOSのimgファイルを書き込む

1. [CensOSのイメージファイル](https://buildlogs.centos.org/centos/7/isos/armhfp/CentOS-Userland-7-armv7hl-Minimal-1611-test-RaspberryPi3.img.xz)をダウンロードして7zipなどで展開する
2. Win32DiskImagerを使用して、imgファイルをSDカードに書き込む

   ![Win32DiskImager](/02.raspberryPi/img/Win32DiskImager_01.png)

### 3.起動直後

1. 初期ログイン

   centos-rpi3 login:root
   Password:centos

2. キーボード設定

   109日本語レイアウトのキーボード設定

   ```sh
   localectl set-keymap jp106
   localectl set-keymap jp-OADG109A
   localectl set-locale LANG=ja_JP.utf8
   ```

   localectlで確認(以下のような表示がされていれば完了)

   ```sh
   localctl

   System Locale: LANG=ja_JP.utf8
       VC Keymap: jp-OADG109A
      X11 Layout: jp
       X11 Model: jp106
     X11 Options: terminate:ctrl_alt_bksp
   ```

### 4.起動後のネットワーク設定

1. ネットワーク設定ファイルの変更

   以下のファイルをviで開く

   ```sh
   vi /etc/sysconfig/network-scripts/ifcfg-eth0
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
   nmcli connection down eth0; nmcli connection up eth0
   ```

      pingで導通確認を行う。速度が安定しない場合はこの時点でrebootすること。
      ここまでくれば、あとはクライアント側からTeraTermで接続するのでラズパイ本体からディスプレイの出力は無くして良い。

パーテーション拡張
wifi接続設定
vimインストール
ユーザーkaioman作成
