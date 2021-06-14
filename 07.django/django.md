# django

## 本番環境構築

### 1. 必要モジュールインストール

  1. インストール

      ```bash
      $yum install httpd httpd-devel mod_wsgi
      ```

### 2. Apache用設定ファイル作成

  1. 設定ファイル作成

      ↓以下、テスト用 /etc/httpd/conf.d/python.conf

      ```sh
      # mod_wsgiの読み込み
      LoadModule wsgi_module modules/mod_wsgi.so

      # /test というリクエストに対して、/var/www/cgi-bin/hello.py 返す。
      WSGIScriptAlias /test /var/www/cgi-bin/hello.py
      ```

### 3. Apache停止→起動

  1. Apacheの停止・再起動

      ```bash
      $sudo systemctl stop httpd.service
      $sudo systemctl start httpd.service
      ```

      * 再起動でも可

        ```bash
        $sudo systemctl restart httpd.service
        ```

  2. Apache動作確認

      ```bash
      $sudo systemctl status httpd.service
      ```

  3. ブラウザで以下のURLにアクセスし"Hello World!"が表示されることを確認する

      [http://192.168.3.22/test](http://192.168.3.22/test)

### 4. 仮想環境をアクティベート

  1. アクティベート

      ```bash
      $cd <仮想環境のディレクトリ>
      $. bin/activate
      ```

### 5. 仮想環境にdjangoをインストールする

  1. インストール

      ```bash
      (env)$pip install django==3.2.4
      ```

      djangoのバージョンはrequirements.txtに記載されているdjangoのバージョンを指定する

  2. djangoのバージョン確認

      ```bash
      $python -m django --version
      ```

  3. djangoのバージョンが表示されればOK

### 6. ユーザーのサブグループにrootを追加する

  1. /var/www/cgi-binフォルダに対する書込権限付与の為、以下のコマンドでサブグループにrootを追加する

      ```sh
      $usermod -aG root <ユーザー名>
      ```

      もしくはcgi-binのグループをwheelに変更する(ユーザーがwheelグループに所属している前提)

      ```sh
      $chgrp wheel cgi-bin
      ```

  2. 確認

      ```bash
      $id <ユーザー名>
      $uid=1000(<ユーザー名>) gid=1000(<グループ名>) groups=1000<グループ名>),0(root),1001(<グループ名>)
      ```

### 7. /var/www/cgi-binフォルダのパーミッションを755→775に変更する

  1. パーミッション変更
  
      ```bash
      $sudo su -
      $cd /var/www
      $chmod 775 cgi-bin
      ```

  2. 確認

      ```bash
      $ls -l
      drwxrwxr-x 2 root root 4096  4月 24 16:16 cgi-bin
      ```

### 8. 仮想環境のdjango-adminでプロジェクトを作成(動作テスト用)

  1. 仮想環境アクティベート

      ```sh
      $su - <ユーザー名> 
      $cd <仮想環境のディレクトリ> 
      $. bin/activate 
      ```

  2. djangoプロジェクト作成

      ```sh
      (env)$cd /var/www/cgi-bin
      (env)$django-admin startproject <プロジェクト名>
      ```

      これで/var/wwww/cgi-binにプロジェクトが作成される
