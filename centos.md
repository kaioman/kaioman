# memo

- インストール

- 設定
  - ネットワーク

- リポジトリ追加

- 追加パッケージ

  - Vim
  - etc

- バックアップ

- 定期実行(CronTab)

- ターミナル(TeraTerm)

- USBメモリアクセス
  
  rootに変更してから下記を実施

  - 1.deviceの割り当て確認

    - USBメモリ接続後、以下のコマンドを実行
    - sdaかsdbでヒットするはず
  
    ```centos
    # dmesg | grep sda
    ```

  - 2.マウント先となるディレクトリを作成

    ```centos
    # mkdir /mnt/flash
    ```

  - 3.マウント

    ```centos
    # mount /dev/sda1 /mnt/flash
    ```

    sda1の部分は1.でヒットしたデバイス名称を指定する

  - 4.確認

    マウントできたか以下のコマンドで確認
    成功していれば/dev/sda1 on /mnt/flash type ext4 (rw,relatime)等の表示が出る

    ```centos
    # mount     
    ```

    /dev/sda1が/mnt/flashにマウントされていることを確認したら以下のコマンドでディレクトリを移動して内容を確認する。

    ```centos
    # cd /mnt/flash
    # ls -a     
    ```

  - 5.アンマウント

    カレントディレクトリがマウント先の場合はcdしておく

    ```centos
    # cd
    # unmount /mnt/flash
    ```

    アンマウントできていることを確認する

    ```centos
    # mount
    '''
