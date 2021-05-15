# Python

## 1.インストール

1. Python3.6インストール

  ```sh
  $yum -y install python3
  ```

## 2.pip

- python3系インストール後はpipが使用できないのでpip3を使う

1. pip自身のアップグレード

    ```sh
    $pip3 install -U pip
    ```

2. パッケージリストの確認

    ```sh
    $pip3 list
    ```

3. アップデートが必要なパッケージのリスト確認

    ```sh
    $pip3 list -o
    ```

## 3.仮想環境の作成

1. venv

    ```sh
    $python3 -m venv <仮想環境名>
    ```

2. アクティベート

    ```sh
    $. <仮想環境パス>\bin\activate
    ```

3. 仮想環境下でpip最新化

    ```sh
    (env)$pip install --upgrade pip
    ```

4. ディアクティベート

    ```sh
    (env)$deactivate
    ```
