# centOS7

## インストール

## 設定

  1. ネットワーク

## リポジトリ追加

## 追加パッケージ

  1. vim

  2. etc

## バックアップ

### DDバックアップ

    ```sh
    $dd if=/dev/<デバイス名> conv=sync,noerror bs=512 status=progress | gzip -c > /mnt/flash/<バックアップファイル名>
    #(例) $dd if=/dev/sda1 conv=sync,noerror bs=512 status=progress | gzip -c > /mnt/flash/dd.img.gz
    ```

## 定期実行(CronTab)

    毎朝9:00に特定のpythonファイルを実行する設定

    ```sh
    $vim /etc/crontab

    (↓ 以下設定例)
    SHELL=/bin/bash
    PATH=/sbin:/bin:/usr/sbin:/usr/bin
    MAILTO=root

    # For details see man 4 crontabs

    # Example of job definition:
    # .---------------- minute (0 - 59)
    # |  .------------- hour (0 - 23)
    # |  |  .---------- day of month (1 - 31)
    # |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
    # |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
    # |  |  |  |  |
    # *  *  *  *  * user-name  command to be executed
    0 9 * * * root cd /home/stockerbastard/stockGetter; bash -l -c '/home/stockerbastard/stockGetter/env/bin/python /home/stockerbastard/stockGetter/Stocker.py >> /home/stockerbastard/stockGetter/log/`date +\%Y\%m\%d`.log'
    0 9 * * * root cd /home/stockerbastard/stockGetter; bash -l -c '/home/stockerbastard/stockGetter/env/bin/python /home/stockerbastard/stockGetter/SyncStock.py >> /home/stockerbastard/stockGetter/log/`date +\%Y\%m\%d`.log'
    ```

## ターミナル

  [TeraTerm](https://1drv.ms/u/s!AtZZJevIaEATkxVBUel3Nn1-wStN?e=CIfTLm)

## USBメモリアクセス

- rootに変更してから下記を実施

### 1.deviceの割り当て確認

    USBメモリ接続後、以下のコマンドを実行。sdaかsdbでヒットするはず

    ```sh
    $dmesg | grep sda
    ```

### 2.マウント先となるディレクトリを作成

    ```sh
    $mkdir /mnt/flash
    ```

### 3.マウント

    ```sh
    $mount /dev/sda1 /mnt/flash
    ```

    sda1の部分は1.でヒットしたデバイス名称を指定する

### 4.確認

    マウントできたか以下のコマンドで確認 成功していれば/dev/sda1 on /mnt/flash type ext4 \(rw,relatime\)等の表示が出る

    ```sh
    $mount
    ```

    /dev/sda1が/mnt/flashにマウントされていることを確認したら以下のコマンドでディレクトリを移動して内容を確認する。

    ```sh
    $cd /mnt/flash
    $ls -a
    ```

### 5.アンマウント

    カレントディレクトリがマウント先の場合はcdしておく

    ```sh
    $cd
    $unmount /mnt/flash
    ```

    アンマウントできていることを確認する

    ```sh
    $mount
    ```

## make installしたプログラムをアンインストールする

### 1.ソースファイルを展開したディレクトリに移動する

    ```sh
    $cd /etc/local/src
    ```

### 2.移動したディレクトリにMakefileがあることを確認する

### 3.アンインストール実行

    ```sh
    $make uninstall
    ```

## 不正なSSHアクセスを調べる

[参考リンク:失敗ログイン (lastb) の IP アドレスを一括拒否](https://qiita.com/bezeklik/items/6467c966338e610ef333)

### 1.不正アクセスの試行ユーザー名ランキング

    ```sh
    $lastb -i \
    | head --lines=-2 \
    | awk '{print $1}' \
    | sort \
    | uniq --count \
    | sort --reverse \
    | less
    ```

### 2.不正アクセスのIPアドレスランキング

    ```sh
    $lastb -i \
    | head --lines=-2 \
    | awk '{print $3}' \
    | sort \
    | uniq --count \
    | sort --reverse \
    | less
    ```

### 3.失敗ログインの IP アドレスの一括拒否設定

#### 1.新しいIPセットの作成

    ```sh
    $firewall-cmd --permanent --new-ipset=blacklist --type=hash:net
    ```

#### 2.IPセットの一覧

    ```sh
    $firewall-cmd --permanent --get-ipsets
    ```

#### 3.IPセットの詳細

    ```sh
    $firewall-cmd --permanent --info-ipset=blacklist
    ```

#### 4.拒否する IP アドレス一覧ファイルの作成

    ```sh
    $lastb -i | head --lines=-2 | awk '{print $3}' | sort | uniq | sort --numeric-sort > /etc/firewalld/ipsets/blacklist
    ```

#### 5.拒否する IP アドレス一覧ファイルの登録

    ```sh
    $firewall-cmd --permanent --ipset=blacklist --add-entries-from-file=/etc/firewalld/ipsets/blacklist 
    ```

#### 6.IP セットの詳細の再確認

    ```sh
    $firewall-cmd --permanent --info-ipset=blacklist
    ```

#### 7.IP セットによる拒否設定

    ```sh
    $firewall-cmd --permanent --zone=public --add-rich-rule='rule source ipset=blacklist drop'
    ```

#### 8.リロードと反映確認

    ```sh
    $firewall-cmd --reload && \
    $firewall-cmd --list-rich-rules
    ```

## ログイン履歴やログイン状況の確認

[参考:【CentOS】ログイン履歴やログイン状況の確認方法](https://www.server-memo.net/tips/server-operation/login-history.html)

## httpdで発生したエラーを確認する  

    ```sh
    $less /var/log/httpd/error_log
    ```

## ディスクの空き容量を確認する

    ```sh
    $df -h

    ファイルシス   サイズ  使用  残り 使用% マウント位置
    /dev/root         27G  3.5G   23G   14% /
    devtmpfs         430M     0  430M    0% /dev
    tmpfs            463M     0  463M    0% /dev/shm
    tmpfs            463M   47M  416M   11% /run
    tmpfs            463M     0  463M    0% /sys/fs/cgroup
    /dev/mmcblk0p1   500M   59M  441M   12% /boot
    tmpfs             93M     0   93M    0% /run/user/1000
    /dev/sda1        7.2G   41M  6.8G    1% /mnt/flash
    tmpfs             93M     0   93M    0% /run/user/1001
    ```
