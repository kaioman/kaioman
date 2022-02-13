# postgresSQL

[参考リンク：postgresql9.2インストール](https://www.server-world.info/query?os=CentOS_7&p=postgresql&f=1)

1. インストール

   1. インストールコマンド

        ```sh
        $yum -y install postgresql-server
        ```

   2. PaspberryPi3 ModelB+ とCensOS7の組み合わせ時における最新バージョン

        ```sh
        $postgres -V
        
        postgres (PostgreSQL) 9.2.24
        ```

   3. データベース初期化

        ```sh
        $postgresql-setup initdb
        ```

2. 設定

   1. 設定ファイル

        ```sh
        $vim /var/lib/pgsql/data/postgresql.conf
        
        # 59行目：他ホストからのアクセスも受け付ける場合はコメント解除して変更
        listen_addresses = '*'
        # 395行目：ログ形式を変更する場合はコメント解除して追記 (以下は [日時 ユーザー DB ～] 形式)
        log_line_prefix = '%t %u %d '
        ```

   2. 起動と自動起動

        ```sh
        $systemctl start postgresql
        $systemctl enable postgresql
        ```

   3. ファイアウォール

        ```sh
        $firewall-cmd --add-service=postgresql --permanent
        $firewall-cmd --reload
        ```

   4. postgresユーザーのパスワード設定

        ```sh
        $su - postgres
        -bash-4.2$psql -C "alter user postgres with password '<password>'"
        ```

3. 接続ツール

     * SQL MK-2

     * pgAdmin4

4. データベースユーザー作成

   ```sh
   -bash-4.2$createuser <DBユーザー>
   ```

5. データベース作成

   ```sh
   -bash-4.2$createdb <データベース名> -O <DBユーザー>
   ```

6. スキーマ作成

  7. postgresユーザーに切り替え

     ```sh
     sudo su - postgres
     ```

  2. postgresにログイン

     ```sh
     -bash-4.2$ psql -U postgres
     ```

  3. データベース接続

     ```sh
     postgres= \c <データベース名>
     ```

  4. スキーマ作成SQL実行

     ```sh
     CREATE SCHEMA <スキーマ名> AUTHORIZATION <ユーザー名>;
     ```

7. テーブル作成

8. テーブル複製

   ```sh
   CREATE TABLE <新テーブル名> AS SELECT * FROM <既存テーブル名>;
   ```

9. データベースバックアップ

   1. postgresユーザーにスイッチ

      ```sh
      $sudo su - postgres
      ```

   2. pg_dumpで任意の場所にバックアップファイルを出力

      ```sh
      postgres> pg_dump -U postgres -Fc <データベース名> -f <出力先>
      ```
