# postgresSQL

* インストール
  * PaspberryPi3 ModelB+ とCensOSの組み合わせ時における最新バージョン
* 設定
  * 自動起動  
  * ファイアウォール
  * その他
* 接続ツール
  * SQL MK-2
  * pgAdmin4
* ログイン
* データベース作成
* スキーマ作成
  * postgresユーザーに切り替え

    ```bash
    sudo su - postgres
    ```

  * postgresにログイン

    ```bash
    -bash-4.2$ psql -U postgres
    ```

  * データベース接続

    ```bash
    postgres= \c [データベース名]
    ```

  * スキーマ作成SQL実行

    ```sql
    CREATE SCHEMA [スキーマ名] AUTHORIZATION [データベース名];
    ```
* テーブル作成
* テーブル複製

  ```sql
  CREATE TABLE [新テーブル] AS SELECT * FROM [既存テーブル];
  ```

* データベースバックアップ

1.postgresユーザーにスイッチ

```bash
sudo su - postgres
```

2.pg\_dumpで任意の場所にバックアップファイルを出力

```bash
pg_dump -U postgres -Fc [データベース名] -f [出力先]
```

