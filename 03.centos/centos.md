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
