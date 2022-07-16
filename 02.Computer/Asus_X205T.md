# X205T A

------



## 製品名

------

- Asus EeeBook X205TA

  <img src="img\X205TA_image.png" alt="X205TA_image" style="zoom:75%;float:left" />

  [サポートサイト](https://www.asus.com/jp/supportonly/X205TA/HelpDesk_Download/)

## 仕様

------

- CPU：Atom Z3735F 1.33Ghz/4コア
- 画面サイズ：11.6型(インチ)
- 解像度：WXGA(1366x768)
- メモリ規格：DDR3L PC3-10600
- メモリ：2GB(増設不可)
- ビデオチップ：Intel HD Graphics
- その他：Webカメラ、microHDMI端子、Blutooth
- 無線LAN：IEEE802.11n
- 幅x高さx奥行：286x17.5x193.3 mm
- 重量：0.98 kg



## Linux Mintインストール

------

1. 参考サイト

   - [Asus x205ta にLinuxMintインストール＆Wifi、MicroSD有効化](http://solidstatelife.blogspot.com/2015/08/asus-x205ta-linuxmintwifimicrosdubuntu.html)
   - [ASUS X205TAにLinuxインストール (ocha-zuke.tokyo)](https://ocha-zuke.tokyo/2020/12/x205ta-linux-mint193/)

2. USB無線wifiを用意

   無くてもインストール自体は成功するが、インストール後にSSDからブートすることが後々困難になるので用意した方が無難

3. rufusをダウンロード

   [rufus:起動可能なUSBドライブの作成](https://rufus.ie/ja/)

4. Linux Mint 20.3(22/07/17時点の最新) Cinnamon 64bit版のisoファイルをダウンロード

   以下のエディションが存在するが、インストールするマシンのスペックに応じて選択

   - Cinnamon Edition
   - MATE Editon
   - Xfce Edition
   - Cinnamon "Edge" Edition

   [Download Linux Mint 20.3 - Linux Mint](https://www.linuxmint.com/download.php)

5. rufusを起動してインストール用USBを作成する

   linuxmintのisoファイルを指定してスタートボタンをクリック

   <img src="img\rufus-linuxmint-bootusb.png" alt="rufus-linuxmint-bootusb" style="zoom:75%;float:left" />

6. 作成したインストール用USBの/efi/bootに[bootia32.efi](https://github.com/jfwells/linux-asus-t100ta/tree/master/boot)をコピー

   bootia32.efiが無いと起動できない(X205TAが32bitUEFIブートに対応していない為)

7. X205TAのBIOS画面でSecureBootをdisableに変更する

   1. 電源ボタン押下にescボタン連打
   2. BIOS設定 -> secure boot control -< disableに設定
   3. usb controller -> ehciに設定
   4. 設定を保存してシャットダウン

8. linux mintインストール

   1. USB無線wifiを接続する

   2. インストール用USBをX205TAに差し込み電源ON

   3. escボタンを連打して起動デバイスを選択(差し込んだUSBメモリを選択)

   4. linux minのインストールウィザードが表示されるので以下内容を設定

      1. 言語設定(日本語を選択)
      2. wifiの設定
      3. タイムゾーンの設定(Tokyoを選択)
      4. コンピューター名やユーザー名、パスワードの設定

   5. インストール作業が開始されるので待つ（大体10分ほど）

   6. インストールが完了したら、まずアップデートマネージャーで各種パッケージを最新化する

   7. 内臓無線wifiの有効化

      1. wifiチップのファームウェアをセットアップ

         注意：brcmfmac43340-sdio.txtは[ここから](https://github.com/Asus-T100/firmware/blob/master/brcm/brcmfmac43340-sdio.txt#L124)取得する

         ```sh
         $wget https://android.googlesource.com/platform/hardware/broadcom/wlan/+archive/master/bcmdhd/firmware/bcm43341.tar.gz
         $tar -zxvf bcm43341.tar.gz
         $sudo cp fw_bcm43341.bin /lib/firmware/brcm/brcmfmac43340-sdio.bin
         $sudo cp brcmfmac43340-sdio.txt /lib/firmware/brcm/brcmfmac43340-sdio.txt
         $sudo modprobe -rv brcmfmac
         $sudo modprobe -v brcmfmac
         ```

      2. sysfsutilsをインストール

         ```sh
         $sudo apt-get install sysfsutils
         ```

      3. /etc/sysfs.confに以下を追記

         ```sh
         $vim /etc/sysfs.conf
         
         bus/platform/drivers/sdhci-acpi/INT33BB:00/power/control = on # 追記
         ```

      4. USB無線wifiを取り外して再起動する。再起動後、内臓無線wifiが使用できるようになっていることを確認する

   8. microSDカードリーダーの有効化

      1. /etc/modprobe.d/sdhci.confに以下を追記(無ければ新規作成)

         ```sh
         $vim /etc/modprobe.d/sdhci.conf
         
         options sdhci debug_quirks=0x8000　# 追記
         ```

      2. 以下のコマンドを実行

         ```sh
         $sudo update-initramfs -u -k all
         ```

         

## CloudReadyインストール

------

1. CloudRaady Home Edition入手

   1. 以下のサイトにアクセスしてCloudReady USB Makderをダウンロード

      [CloudReady Home Edition Free Download](https://www.neverware.com/freedownload#home-edition-install)

      <img src="img\Download_usb_maker.png" alt="Download_usb_maker" style="zoom:75%;float:left" />

   2. ダウンロードしたcloudready-usb-maker.exeを実行する

   3. 8GBか16GBのUSBメモリをPCに挿してNextをクリック

      <img src="img\usb_maker_w_1.png" alt="usb_maker_w_1" style="zoom:75%;float:left" />

   4. 対象のUSBメモリを選択してNextをクリック

      <img src="img\usb_maker_w_2.png" alt="usb_maker_w_2" style="zoom:75%;float:left" />

   5. CloudReadyのダウンロードとUSB書込が始まるので待つ。書いてあるとおり20分ほどかかるので終わったらFinishをクリック

      <img src="img\usb_maker_w_3.png" alt="usb_maker_w_3" style="zoom:75%;float:left" />

2. CloudReadyをX205TAにインストール

   1. 作成したUSBインストーラーをX205TAに挿入
   
   2. 電源投入後、escキーを連打
   
   3. bootデバイスの選択画面が現れるので、挿入したUSBメモリを選択
   
   4. CloudReadyのインストールウィザードが始まるので順に進める
      1. 言語、キーボードの選択
      2. wifi設定
      3. googleアカウントでログイン
   
   5. デスクトップ画面が表示される（この時点ではストレージにインストールされていないので注意）

   6. タスクバーにある時刻部分をクリックする
   
   7. install CloudReadyをクリックする
   
   8. ストレージへのインストールが始まるのでそのまま待つ。終了すると一旦ＰＣの電源が切れるので電源を再投入する
   
   9. USBインストーラーを外してもCloudReadyが起動するか確認する（途中で再度インストールウィザードが始まるので同じように設定する）
   
      (2022/07/16追記)　アップデートを実行すると何故かChrome Flex osに切り替わってしまい、内臓wifiが認識しなくなる。その場合、USBの無線wifiを接続する必要が出てくるが、特定のメーカーしか認識しない模様（この為に購入したelecomのWDC-150SU2MBKはダメだった）
   
      
   
   

