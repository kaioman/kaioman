# centOS7

## インストール

## 設定

  1. ネットワーク

## リポジトリ追加

## 追加パッケージ

  1. Vim

  2. etc

## バックアップ

## 定期実行(CronTab)

## ターミナル

  [TeraTerm](https://1drv.ms/u/s!AtZZJevIaEATkxVBUel3Nn1-wStN?e=CIfTLm)

## USBメモリアクセス

- rootに変更してから下記を実施

### 1.deviceの割り当て確認

    USBメモリ接続後、以下のコマンドを実行。sdaかsdbでヒットするはず

    ```bash
    $dmesg | grep sda
    ```

### 2.マウント先となるディレクトリを作成

    ```bash
    $mkdir /mnt/flash
    ```

### 3.マウント

    ```bash
    $mount /dev/sda1 /mnt/flash
    ```

    sda1の部分は1.でヒットしたデバイス名称を指定する

### 4.確認

    マウントできたか以下のコマンドで確認 成功していれば/dev/sda1 on /mnt/flash type ext4 \(rw,relatime\)等の表示が出る

    ```bash
    $mount
    ```

    /dev/sda1が/mnt/flashにマウントされていることを確認したら以下のコマンドでディレクトリを移動して内容を確認する。

    ```bash
    $cd /mnt/flash
    $ls -a
    ```

### 5.アンマウント

    カレントディレクトリがマウント先の場合はcdしておく

    ```bash
    $cd
    $unmount /mnt/flash
    ```

    アンマウントできていることを確認する

    ```bash
    $mount
    ```

## make installしたプログラムをアンインストールする

### 1.ソースファイルを展開したディレクトリに移動する

  ```bash
  $cd /etc/local/src
  ```
