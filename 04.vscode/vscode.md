# vscode

## VSCodeインストール方法

* 日本語化

## 拡張機能

* SFTP
* Markdown All in One
* Markdown Checkbox
* Markdown Preview Enhanced
* Markdown Preview Github Styling
* Markdownlint
* Prettier - Code formatter
* Python
* Ptyhon Extension Pack
* Jupyter
* Japanese Language Pack fro Visual Studio Code
* Git History
* Django
* Jinja
* Visual Studio IntelliCode

## VSCode設定

### github連携

### sftp.json

### launch.json

### settings.json

#### pythonライブラリの場所指定(python.pythonPath)

#### 単体テストファイル設定(python.testing.unittestArgs)

### 改行コード

* シェルスクリプトファイルをVsCodeで編集してLinuxサーバーにアップする場合、改行コードCrLfだとshコマンドでシェルスクリプトファイルを実行時に「そのようなファイルやディレクトリはありません」と表示されることがある

#### 1.拡張機能にcode-eol 2019(Line Endings)を追加

##### 1.インストールボタンをクリック

![code-eol2019](img/code-eol.png)

#### 2.改行コードが可視化される

![eof-visible](img/eof-visible.png)

### ファイルアイコン変更

#### 1.左下の歯車マークからファイルアイコンの変更をクリック

![fileicon-change](img/fileicon-change.png)

#### 2.コマンドパレットから任意のファイルアイコンテーマを選択

![fileicon-select](img/fileicon-select.png)

## ワークスペース

### ワークスペース追加方法

#### 1.ファイル-ワークスペースにフォルダーを追加の順に選択する

![workspace-in-folder-add](img/workspace_folder_add.png)

#### 2.ワークスペースとして追加するフォルダを選択する

![workspace-folder-select](img/workspace-folder-select.png)

#### 3.ワークスペースとして選択されたフォルダの直下にworkspace.code-workspaceというファイルが作成される

![workspace-createfile](img/workspace-createfile.png)
