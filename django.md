# django

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

- 3.Apache停止→起動

```sh
sudo systemctl stop httpd.service
sudo systemctl start httpd.service
```

再起動でも可

```sh
sudo systemctl restart httpd.service
```

- Apache動作確認

```sh
sudo systemctl status httpd.service
```

- ブラウザで以下のURLにアクセスし"Hello World!"が表示されることを確認する

    [http://192.168.3.22/test](http://192.168.3.22/test)

- 4.仮想環境をアクティベート

    ```sh
    cd [仮想環境のディレクトリ]
    . bin/activate
    ```

- 5.仮想環境にdjangoをインストールする

    ```sh
    pip install django==2.2.20
    ```

    djangoのバージョンはrequirements.txtに記載されているdjangoのバージョンを指定する

    djangoのバージョン確認

    ```sh
    python -m django --version
    ```

    djangoのバージョンが表示されればOK
