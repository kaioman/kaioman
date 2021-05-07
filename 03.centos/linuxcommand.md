# Linuxコマンド

## 1.ディレクトリ削除

* -fオプション→確認なしで削除する  
* -rオプション→再帰的に削除する
* ディレクトリ内にファイルがあっても削除する

    ```sh
    $rm -rf <ディレクトリ名>
    ```

* 削除時に確認を行う場合\(yで許可,nでキャンセル\)

    ```sh
    $rm -r <ディレクトリ名>
    ```

* 以下のコマンドではディレクトリ内にファイルがあるとエラーとなる

    ```sh
    $rmdir <ディレクトリ名>
    ```

## 2.ファイルコピー

* 通常のコピー\(基本形\)

    ```sh
    $cp <コピー元> <コピー先>
    ```

## 3.パーミッション

* アクセス件変更

  ```sh
  $chmod <パーミッション> <ファイルorディレクトリ>
  ```

* 所有者変更

  ```sh
  $chown <[所有者]or[所有者:グループ]> <ファイルorディレクトリ>
  ```

* グループ変更

  ```sh
  $chgrp <グループ> <ファイルorディレクトリ>
  ```

* パーミッション確認

  ```sh
  $ls -l
  ```

* パーミッションの書き方

  ```bash
  drwxr-xr-x
  ```

  たとえばdrwxr-xr-xというパーミッションがあったとする。 最初のd:ディレクトリであることを意味する。ファイルであれば「-」となる

  ```bash
  drwxr-xr-x 2 user_name group 68 4 25 13:44 common
  ```

  それぞれの文字の意味

  -**r**:\(read\)読込権限

  -**w**:\(write\)書込権限

  -**x**:\(execute\)実行権限

  数字での略称記法

  | 記号 | 数字 | 意味 |
  | :--- | :--- | :--- |
  | r | 4 | 読込権限 |
  | w | 2 | 書込権限 |
  | x | 1 | 実行権限 |

  アクセス権の対象

  | 記号 | 意味 |
  | :--- | :--- |
  | u | 所有者\(user\) |
  | g | グループ\(group\) |
  | o | その他\(other\) |
  | a | 上の3つすべて\(all\) |

## 4.suの使用を制限する

1. /etc/pam.d/su を編集する

    ```sh
    $vim /etc/pam.d/su
    ```

    #auth～の行をコメントアウト解除

   * 変更前

      ```sh
      # Uncomment the following line to require a user to be in the "wheel" group.
      #auth            required        pam_wheel.so use_uid
      ```

   * 変更後
  
      ```sh
      # Uncomment the following line to require a user to be in the "wheel" group.
      auth            required        pam_wheel.so use_uid
      ```

2. su許可ユーザーをwheelグループに追加する

   1. /etc/groupを編集する

      ```sh
      $vim /etc/group
      ```

   2. su許可ユーザーをwheelグループに追加する

      * 編集前

        ```sh
        wheel:x:10:
        ```

      * 編集後

        ```sh
        wheel:x:10:<su許可ユーザー>
        ```

        ※複数のユーザーを追加する場合は「,」で区切る

## 5.systemctl

* コマンド群

  |操作|コマンド|
  |-----|-----|
  |サービス起動|systemctl start ${Unit}|
  |サービス停止|systemctl stop ${Unit}|
  |サービス再起動|systemctl restart ${Unit}|
  |サービスリロード|systemctl reload ${Unit}|
  |サービスステータス表示|systemctl status ${Unit}|
  |サービス自動起動有効|systemctl enable ${Unit}|
  |サービス自動起動無効|systemctl disable ${Unit}|
  |サービス自動起動設定確認|systemctl is-enabled ${Unit}|  
  |サービス一覧|systemctl list-unit-files --type=service ${Unit}|
  |設定ファイルの再読込|systemctl deamon-reload ${Unit}|

## 6.yum

1. パッケージをインストールする

   * 基本形。インストール途中で確認が入った場合は指定する(y/n)

     ```sh
     $yum install <パッケージ名>
     ```

   * インストール途中で行われる確認をすべてyesとする場合

     ```sh
     $yum -y install <パッケージ名>
     ```

   * 複数のパッケージを一括でインストールする場合  

     ```sh
     $yum install <パッケージ名1> <パッケージ名2>
     ```

2. パッケージをアンインストールする

    ```sh
    $yum remove <パッケージ名>
    ```

## 7.シャットダウン・再移動

1. シャットダウン

    ```sh
    $shutdown -h now
    ```

2. 再起動

    ```sh
    $shutdown -r now
    ```
