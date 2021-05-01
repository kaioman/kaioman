# django

## 本番環境構築

* 1.必要モジュールインストール

```bash
yum install httpd httpd-devel mod_wsgi
```

* 2.Apache用設定ファイル作成

↓以下、テスト用 /etc/httpd/conf.d/python.conf

```text
# mod_wsgiの読み込み
LoadModule wsgi_module modules/mod_wsgi.so

# /test というリクエストに対して、/var/www/cgi-bin/hello.py 返す。
WSGIScriptAlias /test /var/www/cgi-bin/hello.py
```

* 3.Apache停止→起動

```bash
sudo systemctl stop httpd.service
sudo systemctl start httpd.service
```

再起動でも可

```bash
sudo systemctl restart httpd.service
```

* Apache動作確認

```bash
sudo systemctl status httpd.service
```

* ブラウザで以下のURLにアクセスし"Hello World!"が表示されることを確認する

  [http://192.168.3.22/test](http://192.168.3.22/test)

* 4.仮想環境をアクティベート

  ```bash
    cd [仮想環境のディレクトリ]
    . bin/activate
  ```

* 5.仮想環境にdjangoをインストールする

  ```bash
    pip install django==2.2.20
  ```

  djangoのバージョンはrequirements.txtに記載されているdjangoのバージョンを指定する

  djangoのバージョン確認

  ```bash
    python -m django --version
  ```

  djangoのバージョンが表示されればOK

* 6.ユーザーのサブグループにrootを追加する

  /var/www/cgi-binフォルダに対する書込権限付与の為、以下のコマンドでサブグループにrootを追加する

  ```bash
    usermod -aG root [ユーザー名]
  ```

  確認

  ```bash
    id kaioman
    uid=1000([ユーザー名]) gid=1000([グループ名]) groups=1000[グループ名]),0(root),1001([グループ名])
  ```

* 7./var/www/cgi-binフォルダのパーミッションを755→775に変更する

  ```bash
    sudo su -
    cd /var/www
    chmod 775 cgi-bin
  ```

  確認

  ```bash
    ls -l
    drwxrwxr-x 2 root root 4096  4月 24 16:16 cgi-bin
  ```

* 8.仮想環境のdjango-adminでプロジェクトを作成\(動作テスト用\)

  仮想環境アクティベート

  '''sh su - kaioman cd \[仮想環境のディレクトリ\] . bin/activate '''

  djangoプロジェクト作成

  ```bash
    django-admin startproject [プロジェクト名]
  ```

  これで/var/wwww/cgi-binにプロジェクトが作成される

