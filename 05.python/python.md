# Python

## 1.インストール

### 1.Python3.6インストール

    ```sh
    $yum -y install python3
    ```

## 2.pip

* python3系インストール後はpipが使用できないのでpip3を使う

### 1.pip自身のアップグレード

    ```sh
    $pip3 install -U pip
    ```

### 2.パッケージリストの確認

    ```sh
    $pip3 list
    ```

### 3.アップデートが必要なパッケージのリスト確認

    ```sh
    $pip3 list -o
    ```

## 3.仮想環境の作成

### 1.venv

    ```sh
    $python3 -m venv <仮想環境名>
    ```

### 2.アクティベート

    ```sh
    $. <仮想環境パス>\bin\activate
    ```

### 3.仮想環境下でpip,setuptools最新化

    ```sh
    (env)$pip install --upgrade pip
    (env)$pip install --upgrade pip setuptools
    ```

### 4.ディアクティベート

    ```sh
    (env)$deactivate
    ```

## 4.numpyインストール

* ハマったのでメモ
* pythonバージョンは3.6

### 4-1.環境インストール

    ```sh
    # gcc-gfortran,blas-devel,lapack-devel,freetypeインストール
    $yum install -y gcc-gfortran blas-devel lapack-devel freetype libpng-devel
    ```

### 4-2.python36-develインストール

    ```sh
    # インストールするpython-develのバージョン調査
    $yum list available | grep python | grep devel

    # python-develインストール
    $yum install -y python36-devel

    # numpyのインストールに失敗する場合は以下をインストール
    $yum install -y python-devel
    ```

### 4-3.numpyインストール

    ```sh
    $pip3 install numpy
    ```

## 5.pandas-datareaderインストール

### 5-1.環境インストール

    ```sh
    # libxml2-devel,libxslt-develインストール
    $yum install libxml2-devel libxslt-devel
    ```

### 5-2.pandas-datareaderインストール

    ```sh
    $pip3 install pandas-datareader
    ```
