# django

## 本番環境構築

### 1. 必要モジュールインストール

#### 1. httpd, httpd-develインストール

  ```bash
  $yum install httpd httpd-devel mod_wsgi
  ```

#### 2. mod_wsgiインストール

  ```bash
  (env)$pip install mod_wsgi
  ```

### 2. Apache用設定ファイル作成

#### 1. mod_wsgi*.soの場所を検索

  ```bash
  $find <仮想環境パス>/ -name 'mod_wsgi*.so'
  # mod_wsgi*.soの場所が表示される
  <仮想環境パス>/lib/python3.6/site-packages/mod_wsgi/server/mod_wsgi-py36.cpython-36m-arm-linux-gnueabi.so
  ```

#### 2. 設定ファイル作成

  ↓以下、テスト用 /etc/httpd/conf.d/python.conf

  ```sh
  # mod_wsgiの読み込み
  LoadModule wsgi_module <mod_wsgi*.soの場所>

  # /test というリクエストに対して、/var/www/cgi-bin/hello.py 返す。
  WSGIScriptAlias /test /var/www/cgi-bin/hello.py
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

#### 1. Apacheの停止・再起動

  ```bash
  $sudo systemctl stop httpd.service
  $sudo systemctl start httpd.service
  ```

  再起動でも可

  ```bash
  $sudo systemctl restart httpd.service
  ```

#### 2. Apache動作確認

  ```bash
  $sudo systemctl status httpd.service
  ```

#### 3. ブラウザで以下のURLにアクセスし"Hello World!"が表示されることを確認する

  [http://192.168.3.22/test](http://192.168.3.22/test)

  もしくは以下のコマンドで確認
  
  ```sh
  $curl localhost/test
  ```

### 4. 仮想環境をアクティベート

#### 1. アクティベート

  ```bash
  $cd <仮想環境のディレクトリ>
  $. bin/activate
  ```

### 5. 仮想環境にdjangoをインストールする

#### 1. インストール

  ```bash
  (env)$pip install django==3.2.4
  ```

  djangoのバージョンはrequirements.txtに記載されているdjangoのバージョンを指定する

#### 2. djangoのバージョン確認

  ```bash
  $python -m django --version
  # djangoのバージョンが表示されればOK
  3.2.4
  ```

### 6. ユーザーのサブグループにrootを追加する

#### 1. /var/www/cgi-binフォルダに対する書込権限付与の為、以下のコマンドでサブグループにrootを追加する

  ```sh
  $usermod -aG root <ユーザー名>
  ```

  もしくはcgi-binのグループをwheelに変更する(ユーザーがwheelグループに所属している前提)

  ```sh
  $chgrp wheel cgi-bin
  ```

#### 2. ユーザーグループ変更後確認

  ```bash
  $id <ユーザー名>
  $uid=1000(<ユーザー名>) gid=1000(<グループ名>) groups=1000<グループ名>),0(root),1001(<グループ名>)
  ```

### 7. /var/www/cgi-binフォルダのパーミッションを755→775に変更する

#### 1. パーミッション変更
  
  ```bash
  $sudo su -
  $cd /var/www
  $chmod 775 cgi-bin
  ```

#### 2. パーミッション変更後確認

  ```bash
  $ls -l
  drwxrwxr-x 2 root root 4096  4月 24 16:16 cgi-bin
  ```

### 8. 仮想環境のdjango-adminでプロジェクトを作成(動作テスト用)

#### 1. 仮想環境アクティベート

  ```sh
  $su - <ユーザー名> 
  $cd <仮想環境のディレクトリ> 
  $. bin/activate 
  ```

#### 2. djangoプロジェクト作成

  ```sh
  (env)$cd /var/www/cgi-bin
  (env)$django-admin startproject <プロジェクト名>
  ```

  これで/var/wwww/cgi-binにプロジェクトが作成される

#### 3. Apache設定ファイル修正

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

  上記パスにホームディレクトリが含まれている場合、初期状態ではApacheユーザーから参照ができない為、chmodで755をにパーミッションを変更

  ```sh
  $chmod 755 <ホームディレクトリ>

  # 上記例では以下のコマンドを実行
  $chmod 755 stockerbastard
  ```

#### 4. wsgi.py修正

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

#### 5. settings.py修正

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

#### 6. httpd再起動

  ```sh
  $systemctl restart httpd
  ```

#### 7. curlで結果確認

  ```sh
  $curl localhost/test_proj

  # Hello Djangoと表示されればOK
  Hello Django
  ```
