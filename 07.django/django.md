# django

## 本番環境構築

### 1. 必要モジュールインストール

#### 1-1. httpd, httpd-develインストール

  ```bash
  $yum install httpd httpd-devel mod_wsgi
  ```

#### 1-2. mod_wsgiインストール

  ```bash
  (env)$pip install mod_wsgi
  ```

### 2. Apache用設定ファイル作成

#### 2-1. mod_wsgi*.soの場所を検索

  ```bash
  $find <仮想環境パス>/ -name 'mod_wsgi*.so'
  # mod_wsgi*.soの場所が表示される
  <仮想環境パス>/lib/python3.6/site-packages/mod_wsgi/server/mod_wsgi-py36.cpython-36m-arm-linux-gnueabi.so
  ```

#### 2-2. 設定ファイル作成

  ↓以下、テスト用 /etc/httpd/conf.d/python.conf

  ```sh
  # mod_wsgiの読み込み
  LoadModule wsgi_module <mod_wsgi*.soの場所>

  # /test というリクエストに対して、/var/www/cgi-bin/hello.py 返す。
  WSGIScriptAlias /test /var/www/cgi-bin/hello.py

  # <hello.pyが格納されているディレクトリ>のアクセス許可(テスト用で作成するので最終的に削除してもいい設定)
  <Directory <hello.pyが格納されているディレクトリ>>
    Require all granted
  </Directory>
  ```
  
  hello.pyの内容

  ```sh
  def application(environ, start_response):
      status = '200 OK'
      output = 'Hello World!'
      output = bytes(output, 'utf-8')

      response_headers = [('Content-type', 'text/html'),
                          ('Content-Length', str(len(output)))]
      start_response(status, response_headers)
      return [output]
  ```

### 3. Apache停止→起動

#### 3-1. Apacheの停止・再起動

  ```bash
  $sudo systemctl stop httpd.service
  $sudo systemctl start httpd.service
  ```

  再起動でも可

  ```bash
  $sudo systemctl restart httpd.service
  ```

#### 3-2. Apache動作確認

  ```bash
  $sudo systemctl status httpd.service
  ```

#### 3-3. ブラウザで以下のURLにアクセスし"Hello World!"が表示されることを確認する

  [http://192.168.3.22/test](http://192.168.3.22/test)

  もしくは以下のコマンドで確認
  
  ```sh
  $curl localhost/test
  ```

### 4. 仮想環境をアクティベート

#### 4-1. アクティベート

  ```bash
  $cd <仮想環境のディレクトリ>
  $. bin/activate
  ```

### 5. 仮想環境にdjangoをインストールする

#### 5-1. インストール

  ```bash
  (env)$pip install django==3.2.4
  ```

  djangoのバージョンはrequirements.txtに記載されているdjangoのバージョンを指定する

#### 5-2. djangoのバージョン確認

  ```bash
  $python -m django --version
  # djangoのバージョンが表示されればOK
  3.2.4
  ```

### 6. ユーザーのサブグループにrootを追加する

#### 6-1. /var/www/cgi-binフォルダに対する書込権限付与の為、以下のコマンドでサブグループにrootを追加する

  ```sh
  $usermod -aG root <ユーザー名>
  ```

  もしくはcgi-binのグループをwheelに変更する(ユーザーがwheelグループに所属している前提)

  ```sh
  $chgrp wheel cgi-bin
  ```

#### 6-2. ユーザーグループ変更後確認

  ```bash
  $id <ユーザー名>
  $uid=1000(<ユーザー名>) gid=1000(<グループ名>) groups=1000<グループ名>),0(root),1001(<グループ名>)
  ```

### 7. /var/www/cgi-binフォルダのパーミッションを755→775に変更する

#### 7-1. パーミッション変更
  
  ```bash
  $sudo su -
  $cd /var/www
  $chmod 775 cgi-bin
  ```

#### 7-2. パーミッション変更後確認

  ```bash
  $ls -l
  drwxrwxr-x 2 root root 4096  4月 24 16:16 cgi-bin
  ```

### 8. 仮想環境のdjango-adminでプロジェクトを作成(動作テスト用)

#### 8-1. 仮想環境アクティベート

  ```sh
  $su - <ユーザー名> 
  $cd <仮想環境のディレクトリ> 
  $. bin/activate 
  ```

#### 8-2. djangoプロジェクト作成

  ```sh
  (env)$cd /var/www/cgi-bin
  (env)$django-admin startproject <プロジェクト名>
  ```

  これで/var/wwww/cgi-binにプロジェクトが作成される

#### 8-3. Apache設定ファイル修正

  ```sh
  $vim /etc/httpd/conf.d/python.conf

  # mod_wsgiの読み込み
  LoadModule wsgi_module /home/stockerbastard/stockGetter/env/lib/python3.6/site-packages/mod_wsgi/server/mod_wsgi-py36.cpython-36m-arm-linux-gnueabi.so
  
  # WSGIPythonHome,WSGIPythonPathの設定
  # <注意>ここで指定されているディレクトリはApacheユーザーから参照可能である必要がある
  WSGIPythonHome /home/stockerbastard/stockGetter/env
  WSGIPythonPath /var/www/cgi-bin/test_proj:/home/stockerbastard/stockGetter/env/lib/python3.6/site-packages
  
  # /test というリクエストに対して、/var/www/cgi-bin/hello.py 返す。
  WSGIScriptAlias /test /var/www/cgi-bin/hello.py
  # /test_django というリクエストに対して、/var/www/cgi-bin/test_proj/test_proj/wsgi.py 返す。
  WSGIScriptAlias /test_proj /var/www/cgi-bin/test_proj/test_proj/wsgi.py
  
  # /var/www/cgi-bin/test_proj/test_projのアクセス制限を設定
  <Directory /var/www/cgi-bin/test_proj/test_proj>
    <Files wsgi.py>
      Require all granted
    </Files>
  </Directory>
  ```

  上記パスにホームディレクトリが含まれている場合、初期状態ではApacheユーザーから参照ができない為、chmodで755にパーミッションを変更

  ```sh
  $chmod 755 <ホームディレクトリ>

  # 上記例では以下のコマンドを実行
  $chmod 755 stockerbastard
  ```

#### 8-4. wsgi.py修正

  ```sh
  $vim /var/www/cgi-bin/test_proj/test_proj/wsgi.py

  import os
  import sys
 
  from django.core.wsgi import get_wsgi_application
 
  sys.path.append('/var/www/cgi-bin/test_proj')
  sys.path.append('/var/www/cgi-bin/test_proj/test_proj')
 
  os.environ.setdefault("DJANGO_SETTINGS_MODULE","test_proj.settings")
 
  application = get_wsgi_application()
  ```

#### 8-5. settings.py修正

  ```sh
  $vim /var/www/cgi-bin/test_proj/test_proj/settings.py

  # ALLOWED_HOST変更
  ALLOWED_HOSTS = []
  ↓
  ALLOWED_HOSTS = ['127.0.0.1','localhost',<ホストIPアドレス>]

  # DATABASESコメントアウト(テストの場合sqliteは必要ないので)
  DATABASES = {
  #    'default': {
  #        'ENGINE': 'django.db.backends.sqlite3',
  #        'NAME': BASE_DIR / 'db.sqlite3',
  #    }
  }
  ```

#### 8-6. httpd再起動

  ```sh
  $systemctl restart httpd
  ```

#### 8-7. curlで結果確認

  ```sh
  $curl localhost/test_proj

  # Hello Djangoと表示されればOK
  Hello Django
  ```

### 9. アプリのデプロイ

#### 9-1. ディレクトリ構成

  ```sh
  [project-directory]/
   ┣ [project-name]/
   ┃    ┣ settings.py
   ┃    ┗ ... 
   ┣ env/ # 仮想環境
   ┣ static/ # このディレクトリはmanage.pyのcollectstaticコマンドで作成(後述)
   ┃    ┗ ... 
   ┣ [app-name]/
   ┃    ┣ static/
   ┃    ┃    ┗ [app-name]/
   ┃    ┃         ┣ css/
   ┃    ┃         ┃   ┗ ...
   ┃    ┃         ┣ js/
   ┃    ┃         ┃   ┗ ...
   ┃    ┃         ┣ img/
   ┃    ┃         ┃   ┗ ...
   ┃    ┃         ┗ ...
   ┃    ┗ ...
   ┣ manage.py
   ┗ requirements.txt
  ```

#### 9-2. VsCodeからsftpでファイルをアップロード

  sftp.json内容(例)

  ```sh:sftp.json
  {
    "name": "strockman-stocker",
    "host": "192.168.3.22",
    "protocol": "sftp",
    "port": 52318,
    "username": "stockerbastard",
    "password": "ym520318",
    "remotePath": "/home/stockerbastard/stocker",
    "uploadOnSave": false,
    "downloadOnOpen": false,
    "ignore":[
        ".vscode",
        ".history",
        ".git",
        ".DS_Store",
        "log",
        "env"
    ]
  }
  ```

#### 9-3. デプロイに当たって修正が必要なファイル

- wsgi.py
  - wsgi.pyの場所、アプリの場所を追加

    ```sh
    import os
    import sys # 追加

    from django.core.wsgi import get_wsgi_application

    sys.path.append('/home/stockerbastard/stocker/stocker') # 追加
    sys.path.append('/home/stockerbastard/stocker/app_stock') # 追加

    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'stocker.settings')

    application = get_wsgi_application()
    ```

- settings.py
  - ALLOW_HOST変更 

    ```sh
    #ALLOWED_HOSTS = ['127.0.0.1','localhost']
    ALLOWED_HOSTS = ['127.0.0.1','localhost',<ホストアドレス>] # ホストアドレスを追加する
    ```

  - STATIC_ROOT変更
  
    ```sh
    #STATIC_ROOT = 'C:/static/'
    STATIC_ROOT = os.path.join(BASE_DIR, 'static')
    ```

#### 9-4. 静的ファイルの配置

##### 9-4-1. 仮想環境にアクティベートして以下のコマンドを入力する

  settings.pyのSTATIC_ROOTに設定されているパスの直下に静的ファイルがコピーされる

  ```sh
  (env)$python manage.py collectstatic
  ```

##### 9-4-2. staticフォルダのアクセス許可設定

  httpd.confに以下の設定を追記する(または別名で定義された/etc/httpd/conf.d/*.confファイルに追記でも可)

  ```sh
  <IfModule alias_module>
    # 静的ファイルが配置されているディレクトリをエイリアスstaticと関連付ける
    Alias /static /home/stockerbastard/stocker/static 
    # 静的ファイルが配置されているディレクトリをアクセス許可する
    <Directory /home/stockerbastard/stocker/static>
      Require all granted
    </Directory>
  </IfModule>
  ```

##### 9-4-3. httpd再起動

  ```sh
  $systemctl restart httpd.service
  ```

## マイグレーション

### 1.マイグレーションファイルの作成

  *事前にmodels.pyでモデルを定義しておく

  ```sh
  $python manage.py makemigrations
  ```

### 2.マイグレーションの実行

  *下記コマンドの実行により、対象のデータベースにモデル定義に従ってテーブルが作成される

  ```sh
  $python manage.py migrate
  ```
