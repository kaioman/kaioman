# memo

- インストール
  - PaspberryPi3 ModelB+ とCensOSの組み合わせ時における最新バージョン

- ログイン

- データベース作成

- スキーマ作成  
  
  - postgresユーザーに切り替え

    ```sh
    sudo su - postgres
    ```

  - postgresにログイン

    ```bash
    -bash-4.2$ psql -U postgres
    ```
  
  - データベース接続

    ```sh
    postgres= \c [データベース名]
    ```

  - スキーマ作成SQL実行

    ```sql
    CREATE SCHEMA [スキーマ名] AUTHORIZATION [データベース名];
    ```
  
- テーブル作成

- 設定
  - 自動起動  
  - ファイアウォール
  - その他
  
- 接続ツール
  - SQL MK-2
  - pgAdmin4
