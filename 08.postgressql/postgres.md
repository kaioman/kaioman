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

  6. postgresユーザーに切り替え

     ```sh
     sudo su - postgres
     ```

  7. postgresにログイン

     ```sh
     -bash-4.2$ psql -U postgres
     ```

  8. データベース接続

     ```sh
     postgres= \c <データベース名>
     ```

  9. スキーマ作成SQL実行

     ```sh
     CREATE SCHEMA <スキーマ名> AUTHORIZATION <ユーザー名>;
     ```

10. テーブル作成

     ```sh
     create table <スキーマ名>.<テーブル名> ( 
     	 <フィールド名>	<データ型>
     	 ･･･
     	,PRIMARY KEY(<プライマリーキー>) 
     ); 
     ```

11. テーブル複製

    ```sh
    CREATE TABLE <新テーブル名> AS SELECT * FROM <既存テーブル名>;
    ```

12. データベースバックアップ

    1. postgresユーザーにスイッチ

       ```sh
       $sudo su - postgres
       ```

    2. pg_dumpで任意の場所にバックアップファイルを出力

       ```sh
       postgres> pg_dump -U postgres -Fc <データベース名> -f <出力先>
       ```

    3. docker-composeで動作するpostgresのバックアップファイルを出力

       事前にdocker-compose.ymlが配置してあるディレクトリに移動しておく

       ```sh
       $docker-compose exec -T <docker-compose service> pg_dump -Fc --no-acl --no-owner -U <postgres-user> -w <database-name> > backup/<file-name>.pg_dump
       ```

       コマンド例：

       ```sh
       $docker-compose exec -T pg12 pg_dump -Fc --no-acl --no-owner -U netkeiber -w raceanalyze > backup/raceanalyze.pg_dump
       ```

    4. docker-composeで動作するpostgresをリストアする

       事前にdocker-compose.ymlが配置してあるディレクトリに移動しておく

       ```sh
       $docker-compose exec -T <docker-compose service> pg_restore -cO -d <database-name> -U <postgres-user> -w < backup/<file-name>.pg_dump
       ```

       コマンド例：

       ```sh
       $docker-compose exec -T pg12_bktest pg_restore -cO -d raceanalyze -U netkeiber -w < backup/raceanalyze.pg_dump
       ```

       実行後に以下のようなエラーメッセージが表示される場合があるが、リストア対象となるスキーマが存在しない場合に

       発生する。ただし、エラーは発生するがちゃんとリストアは完了する。

       ```sh
       pg_restore: error: could not execute query: ERROR:  schema "nkaz" does not exist
       Command was: DROP TABLE nkaz.log_race_id;
       ```

    5. 定期的にバックアップを取得する
    
       バックアップ用シェルスクリプト
    
       ```sh
       vim /home/nkaz/pgbk/backup_sh/pg_raceanalyze_backup.sh
       ```
    
       ```sh
       #!/bin/bash
       
       # コマンドの実行履歴を出力する
       set -x$
       
       # コマンドの返り値が非ゼロのとき停止する
       set -e
       
       # バックアップファイルを残しておく日数
       PERIOD='+10'
       
       # 日付
       DATE=`date '+%Y%m%d-%H%M%S'`
       
       # バックアップ先ディレクトリ
       SAVEPATH='/home/nkaz/pgbk/'
       
       # 先頭文字
       PREFIX='raceanalyze-'
       
       # 拡張子
       EXT='.pg_dump'
       
       # コンテナ名
       CONTAINERNAME='[コンテナID]' # ここはdocker psでcontaineridを調べて指定する
       
       # ユーザー
       DBUSER='netkeiber'
       
       # データベース名
       DBNAME='raceanalyze'
       
       # バックアップ実行
       docker exec $CONTAINERNAME pg_dump -Fc --no-acl --no-owner -U $DBUSER -w $DBNAME > $SAVEPATH$PREFIX$DATE$EXT
       
       # 保存期間が過ぎたファイルの削除
       find $SAVEPATH -type f -daystart -mtime -maxdepth 1 $PERIOD -exec rm {} \;
       
       ```
       
       
