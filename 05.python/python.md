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

### 4.requiremets.txtによる一括インストール

    ```sh
    $pip3 install -r requirements.txt

    # requirements.txt書き方(例)

        beautifulsoup4==4.9.3
        bs4==0.0.1
        configparser==5.0.2
        decorator==4.4.2
        numpy==1.19.5
        pandas==1.1.5
        pandas-datareader==0.9.0
        psycopg2==2.8.6
        requests==2.25.1
        six==1.15.0
        SQLAlchemy==1.4.18
        xlrd==2.0.1
        xlwt==1.3.0
    ```

### 5.現在の環境の設定ファイル書き出し

    ```sh
    $pip3 freeze > requirements.txt
    ```

## 3.仮想環境の作成

### 1.venv

    ```sh
    $python3 -m venv <仮想環境名>
    ```
    
    下記のエラーが発生する場合の対処方
    ```sh
    $python3 -m venv env
    Error: Command '['/path/to/env/bin/python3', '-Im', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1
    ```

#### 1.pipなしで仮想環境を作成する

    ```sh
    $python3 -m venv --without-pip <仮想環境名>
    ```

#### 2.仮想環境作成後、アクティベート

    ```sh
    $. <仮想環境パス>\bin\activate
    ```

#### 3.pipインストール

    ```sh
    (env)$pip curl -O https://bootstrap.pypa.io/get.pip.py
    (env)$python get-pip.py
    ```

#### 4.仮想環境に入り直す

    ```sh
    (env)$deactivate
    $. <仮想環境パス>\bin\activate
    ```

#### 5.pipのインストールパスが仮想環境側を向いているか確認

    ```sh
    (env)$which pip
    (env)$~/****/env/bin/pip (仮想環境のpipになっていればOK)
    ```

#### 6.上記作業を行うスクリプトファイル

    ```sh
    #!/bin/zsh
    set -eu
    python3 -m venv --without-pip <仮想環境名>
    curl -O https://bootstrap.pypa.io/get-pip.py
    (){ setopt local_options unset; . <仮想環境名>/bin/activate }
    python get-pip.py
    (){ setopt local_options unset; deactivate }
    (){ setopt local_options unset; . <仮想環境名>/bin/activate }
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

## 6.psycopg2インストール

### 6-1.環境インストール

    ```sh
    # postgresql-develインストール
    $yum install postgresql-devel
    ```

### 6-2.psycopg2インストール

    ```sh
    $pip3 install psycopg2
    ```
