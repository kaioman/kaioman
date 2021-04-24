# memo

## 本番環境構築

- 1.必要モジュールインストール

```sh
yum install httpd httpd-devel mod_wsgi
```

- 2.Apache用設定ファイル作成

↓以下、テスト用
/etc/httpd/conf.d/python.conf

```conf
# mod_wsgiの読み込み
LoadModule wsgi_module modules/mod_wsgi.so

# /test というリクエストに対して、/var/www/cgi-bin/hello.py 返す。
WSGIScriptAlias /test /var/www/cgi-bin/hello.py
```

- 3.Apache起動

```sh
sudo systemctl start httpd.service
```

- Apache動作確認

```sh
sudo systemctl status httpd.service
```
