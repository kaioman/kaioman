# Python

## 1.インストール

1. Python3.6インストール

  ```sh
  $yum -y install python3
  ```

## 3.pip

1. パッケージリストの確認

    ```sh
    $pip list
    ```

2. アップデートが必要なパッケージのリスト確認

    ```sh
    $pip list -o
    ```

3. pip自身のアップグレード

    ```sh
    $pip install -U pip
    ```

## 4.仮想環境の作成

1. venv

  ```sh
  $python3 -m venv <仮想環境名>
  ```
