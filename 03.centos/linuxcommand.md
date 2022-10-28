# Linuxコマンド

1. ## ファイル・ディレクトリ削除

   * ファイル削除

     ```sh
     $rm <ファイル名>
     * -fオプション→確認なしで削除する  
     * -rオプション→再帰的に削除する
     
     * ディレクトリ内にファイルがあっても削除する
     $rm -rf <ディレクトリ名>
     
     * 削除時に確認を行う場合\(yで許可,nでキャンセル\)
     $rm -r <ディレクトリ名>
     
     * 以下のコマンドではディレクトリ内にファイルがあるとエラーとなる
     $rmdir <ディレクトリ名>
     ```

2. ## ファイル・ディレクトリコピー

   * 通常のコピー\(基本形\)

       ```sh
       $cp <コピー元ファイルパス> <コピー先ファイルパス>
       ```
       
   * ディレクトリのコピー

       ```sh
       $cp -r <コピー元ディレクトリパス> <コピー先ディレクトリパス>
       ```

       

3. ## ファイル名変更(移動)

   ```sh
   $mv <変更前ファイル名> <変更後ファイル名>
   ```

   - ファイルの移動も同じコマンド

     ```sh
     $mv <移動前ファイルパス> <移動後ファイルパス>
     ```

4. ## ファイル・ディレクトリ圧縮・解凍

   1. ### ファイル・ディレクトリ圧縮

      ```sh
      $tar -Jcvf <ファイル名>.tar.xz <対象ファイル>
      ```

   2. ### ファイル・ディレクトリ解凍

      ```sh
      $tar -xvf <ファイル名>.tar.xz
      ```

      

5. ## パーミッション

   1. ### アクセス件変更

      ```sh
      $chmod <パーミッション> <ファイルorディレクトリ>
      ```

   2. ### 所有者変更

      ```sh
      $chown <[所有者]or[所有者:グループ]> <ファイルorディレクトリ>
      ```

   3. ### グループ変更

      ```sh
      $chgrp <グループ> <ファイルorディレクトリ>
      ```

       -Rオプションで指定したディレクトリ以下も変更する 

      ```sh
      $chgrp -R <グループ> <ファイルorディレクトリ>
      ```

   4. ### パーミッション確認

      ```sh
      $ls -l
      ```

   5. ### パーミッションの書き方

      ```sh
      drwxr-xr-x
      ```

       たとえば "drwxr-xr-x" というパーミッションがあったとする。 最初のd:ディレクトリであることを意味する。ファイルであれば「-」となる

      ```sh
      drwxr-xr-x 2 user_name group 68 4 25 13:44 common
      ```

      - それぞれの文字の意味

        -**r**:\(read\)読込権限

        -**w**:\(write\)書込権限

        -**x**:\(execute\)実行権限

      

      - 数字での略称記法

        | 記号 | 数字 | 意味     |
        | :--- | :--- | :------- |
        | r    | 4    | 読込権限 |
        | w    | 2    | 書込権限 |
        | x    | 1    | 実行権限 |

      - アクセス権の対象

        | 記号 | 意味                 |
        | :--- | :------------------- |
        | u    | 所有者\(user\)       |
        | g    | グループ\(group\)    |
        | o    | その他\(other\)      |
        | a    | 上の3つすべて\(all\) |

6. ## ユーザー

   1. ### ユーザーが所属しているグループを確認する

      ```sh
      $groups <ユーザー名>
      ```

   2. ### ユーザーの所属グループを変更する

      ```sh
      $usermod -G <グループ名> <ユーザー名>
      ```

   3. ### グループに所属しているユーザーの確認

      ```sh
      $getent group <ユーザー名>
      ```

      

   4. ### ユーザー一覧を確認する

      ```sh
      $cat /etc/passwd
      ```

      

7. ## suの使用を制限する

   1. ### /etc/pam.d/su を編集する

      ```sh
      $vim /etc/pam.d/su
      ```

      #auth～の行をコメントアウト解除

      - 変更前

        ```sh
        # Uncomment the following line to require a user to be in the "wheel" group.
        #auth            required        pam_wheel.so use_uid
        ```

      * 変更後

          ```sh
          # Uncomment the following line to require a user to be in the "wheel" group.
          auth            required        pam_wheel.so use_uid
          ```

   2. ### su許可ユーザーをwheelグループに追加する

      1. #### /etc/groupを編集する

         ```sh
         $vim /etc/group
         ```

      2. #### /etc/groupのwheelグループにsu許可ユーザーを追加する

         * 編集前

             ```sh
             wheel:x:10:
             ```
         
         * 編集後
         
             ```sh
             wheel:x:10:<su許可ユーザー>
             ```
         
             ※複数のユーザーを追加する場合は「,」で区切る

8. ## systemctl

   1. ### コマンド群

      | 操作                     | コマンド                                         |
      | ------------------------ | ------------------------------------------------ |
      | サービス起動             | systemctl start ${Unit}                          |
      | サービス停止             | systemctl stop ${Unit}                           |
      | サービス再起動           | systemctl restart ${Unit}                        |
      | サービスリロード         | systemctl reload ${Unit}                         |
      | サービスステータス表示   | systemctl status ${Unit}                         |
      | サービス自動起動有効     | systemctl enable ${Unit}                         |
      | サービス自動起動無効     | systemctl disable ${Unit}                        |
      | サービス自動起動設定確認 | systemctl is-enabled ${Unit}                     |
      | サービス一覧             | systemctl list-unit-files --type=service ${Unit} |
      | 設定ファイルの再読込     | systemctl daemon-reload ${Unit}                  |

9. ## yum

   1. ### パッケージをインストールする

      * 基本形。インストール途中で確認が入った場合は指定する(y/n)

        ```sh
        $yum install <パッケージ名>
        ```
      
        インストール途中で行われる確認をすべてyesとする場合
      
        ```sh
        $yum -y install <パッケージ名>
        ```
      
        複数のパッケージを一括でインストールする場合
      
        ```sh
        $yum install <パッケージ名1> <パッケージ名2>
        ```

      2. ### パッケージをアンインストールする

         ```sh
         $yum remove <パッケージ名>
         ```

      3. ### パッケージを更新する

         ```sh
         $yum update
         ```

10. ## シャットダウン・再起動

   1. シャットダウン

      ```sh
      $shutdown -h now
      ```

   2. 再起動

      ```sh
      $shutdown -r now
      ```

      または

      ```sh
      $reboot
      ```

      

11. ## NetworkManager

    * コマンド群

      |操作|コマンド|
      |-----|-----|
      |ネットワーク機器リスト表示|nmcli d|
      |wifiネットワークへ接続|nmcli radio wifi on|
      |wifiネットワークから切断|nmcli radio wifi off|
      |全てのネットワークを接続|nmcli networking on|
      |全てのネットワークを切断|nmcli networking off|
      |ネットワークデバイスを有効化|nmcli d connect <デバイス名>|
      |ネットワークデバイスを無効化|nmcli d disconnect <デバイス名>|
      |メモリ・ディスク上のプロファイル表示|nmcli c show|  
      |デバイスの接続状況|nmcli d status|
      |デバイスの詳細表示|nmcli d show <デバイス名>|
      |接続可能な無線アクセスポイント表示|nmcli d wifi|
      |接続状態(下のテーブル参照)|nmcli networking connectivity|

      * 接続状態
      
        |状態|意味|
        |-----|-----|
        |none|the host is not connected to any network.|
        |portal|the host is behind a captive portal and cannot reaach the full Internet.|
        |limited|the host is connected to a network, but it has no access to the Internet.|
        |full|the host is connected to a network and has full access to the Internet.|
        |unknown|the connectivity status cannot be found out.|
      

12. ## ip

    - ### ip rule(rule table参照)

      1. #### show

         * ルールの一覧を表示する

           ```sh
           $ip rule show
           
           0:      from all lookup local
           32766:  from all lookup main
           32767:  from all lookup default
           ```

    - ### ip route(routing table参照)

      1. #### show

         * ルーティングの一覧を表示する

            ```sh
            $ip route show
            
            default via 192.168.3.1 dev eth0 proto static metric 100 linkdown
            default via 192.168.3.1 dev wlan0 proto static metric 600
            192.168.3.0/24 dev eth0 proto kernel scope link src 192.168.3.121 metric 100 linkdown
            192.168.3.0/24 dev wlan0 proto kernel scope link src 192.168.3.22 metric 600
            ```

      2. #### get

         * 指定したIPアドレスの経路を調べる

            ```sh
            $ip route get <IPアドレス>
            ```

            (例) ゲートウェイまでの経路を調べる

            ```sh
            $ip route get 192.168.3.1
            
            192.168.3.1 dev wlan0 table subroute-wlan0 src 192.168.3.22 uid 1000
            cache
            ```

      3. ### ss(NICの状態確認)

         * netstatはCentOS7以降は非推奨

           ```sh
           $ss -nr
           
           Netid  State      Recv-Q Send-Q Local Address:Port               Peer Address:Port
           u_str  ESTAB      0      0         * 8957                  * 0
           u_str  ESTAB      0      0         * 9019                  * 0
           u_str  ESTAB      0      0         * 9041                  * 0
           u_str  ESTAB      0      0      /run/dbus/system_bus_socket 9042                  * 0
           (省略)
           ```

13. ## iwconfig

    1. インストール

       ```sh
       $yum -y install wireless-tools
       ```

    2. PowerManagementの有効/無効切り替え

       * 無効化

         ```sh
         $iwconfig wlan0 power off
         ```
       
       * 有効化
       
         ```sh
         $iwconfig wlan0 power on
         ```
       
         ※ただし、再起動後に設定が元に戻るので恒久的に変更したい場合は/usr/local/sbin/<ファイル名>.shに仕込んでおく     

14. ## cat

    - ファイルの閲覧を行う

      ```sh
      $cat <ファイルパス>
      ```

        [【Linuxコマンド集】3分でわかるcat コマンドの使い方](https://eng-entrance.com/linux_command_cat#Linux_cat)

15. ## less

    -   ファイルの内容を1画面ごとに閲覧する

      ```sh
      $less <ファイルパス>
      ```

        [【 less 】コマンド（基本編）――メッセージやテキストファイルを1画面ずつ表示する](https://www.atmarkit.co.jp/ait/articles/1702/09/news031.html)

16. ## curl

    -   Webサーバーからファイルのダウンロードを行う

      ```sh
      $curl <ターゲットURL>
      ```

      [curlコマンドの使い方](https://qiita.com/hana_shin/items/949cdbe6325c6eee730f)

17. ## su

    1. ユーザーを切り替える

       ```sh
       $su - <ユーザー名>
       ```

    2. /sbin/nologin(ログイン不可)のユーザーに切り替える

       ```sh
       $su -s /bin/bash <ユーザー名>
       ```

18. ## sudo

    1. スーパーユーザーでコマンドを実行する

       ```sh
       $sudo <コマンド>
       ```

    2. ユーザーを指定してコマンドを実行する

       ```sh
       $sudo -u <ユーザー名> <コマンド>
       ```

19. ## ps

    1. プロセスの一覧を表示する

       ```sh
       $ps -a
       ```

       ただし、この方法だとターミナルから切断するとその後はnohup実行のプロセスが表示されない。

    2. nohup実行のプロセス一覧を表示する

       ```sh
       $ps aux
       ```

20. ## kill

    1. プロセスを停止する

       ```sh
       $kill [PID]
       ```

       killするPIDはpsコマンドで調べる

21. ## nohup

    ターミナルから切断後もpythonを実行したままにする

    1. nohupを付けてpythonを実行

       ```sh
       (env)$nohup python hoge.py > /dev/null 2>&1 &
       ```

       

