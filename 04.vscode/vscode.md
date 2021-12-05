# vscode

1. ## vscodeインストール

   1. ## ダウンロード

      [Visual Sudio CodeのWebサイト](https://code.visualstudio.com/Download)より、インストーラーをダウンロード

      <img src="img\vscode-installer-download.png" alt="vscode-installer-download" style="zoom: 80%; float:left;" />

   2. ## インストーラーを実行する

      1. ### 使用許諾契約書の同意

         <img src="img\vscode-installer-accept.png" alt="vscode-installer-accept" style="zoom:75%;float:left;" />

      2. ### 追加タスクの指定(デフォルトのまま)

         <img src="img\vscode-installer-addtask.png" alt="vscode-installer-addtask" style="zoom:80%;float:left;" />

      3. ### インストール実行

         <img src="img\vscode-installer-install.png" alt="vscode-installer-install" style="zoom:80%;float:left;" />

      

   3. ## 日本語化

      1. ### [View] - [Command Palette]を選択

         <img src="img\vscode-change-language-commandpalette.png" alt="vscode-change-language-commandpalette" style="zoom:100%;float:left;" />

      2. ### 入力欄に Configure Display Language と入力、または選択する

         <img src="img\vscode-change-language-configure-display-language.png" alt="vscode-change-language-configure-display-language" style="zoom:100%;float:left;" />

      3. ### 画面左パネルの一覧から「Japanese Language Packs for..」を選択してInstallボタンをクリックする

         <img src="img\vscode-change-language-configure-display-language-addJp.png" alt="vscode-change-language-configure-display-language-addJp" style="zoom:80%;float:left;" />

      4. ### vscodeの再起動確認があるので、vscodeを再起動する

      

2. ## Gitインストール及び初期設定

   [参考：Visual Studio Code（VSCode）でGitを使う（Windows 10環境）](https://cravelweb.com/webdesign/post-3876)

   1. ### 公式サイトよりインストーラーをダウンロードする

      <img src="img\git-download-website.png" alt="git-download-website" style="zoom:80%;float:left;" />

   2. ### インストールウィザードを下記の通り進める（基本はデフォルト値のままでOK）

   3. ### gitアカウントのユーザー名、Eメールアドレスを設定する

      ```sh
      $git config --global user.name '<user-name>'
      $git config --global user.email '<email-address>'
      ```

3. ## Python(3.9.6)インストール

   1. ### 公式サイトよりインストーラーをダウンロードする

   2. ### インストーラーを実行してPythonをインストールする

   3. ### インストール後、仮想環境のアクティベートを可能とする為に以下のコマンドをコマンドプロンプト(管理者権限)にて実行する

      ```shell
      >PowerShell Set-ExecutionPolicy RemoteSigned
      ```

      [参考：PowerShell のスクリプトが実行できない場合の対処方法](https://qiita.com/Targityen/items/3d2e0b5b0b7b04963750)

      

4. ## 拡張機能

   * Japanese Language Pack fro Visual Studio Code

   * Python
     * Ptyhon Extension Pack
       * Python Docstring Generator
       * Jinja
       * Django
       * Visual Studio IntelliCode
       * Python Indent
   
   
      * SFTP(liximomo)
   
   
   
      * Git History
   
   
   
      * Easy icon thema
   
   
   
      * vscode-workspace-switcher
   
   
   
      * Material Thema
   
   
   
      * Peacock
      * Prettier - Code formatter
      * Jupyter
      * Todo+
   

5. ## vscode設定

   - github連携
   - sftp.json
   - launch.json
   - settings.json
     - pythonライブラリの場所指定(python.pythonPath)
     - 単体テストファイル設定(python.testing.unittestArgs)

   1. ### 	改行コード

      シェルスクリプトファイルをVsCodeで編集してLinuxサーバーにアップする場合、改行コードCrLfだとshコマンドでシェルスクリプトファイルを実行時に「そのようなファイルやディレクトリはありません」と表示されることがある

      1. #### 拡張機能にcode-eol 2019(Line Endings)を追加

      2. ##### インストールボタンをクリック

         <img src="img/code-eol.png" alt="code-eol2019" style="float:left;" />

      3. ##### 改行コードが可視化される

         <img src="img/eof-visible.png" alt="eof-visible" style="float:left;" />

   2. ### ファイルアイコン変更

      1. #### 左下の歯車マークからファイルアイコンの変更をクリック

         <img src="img/fileicon-change.png" alt="fileicon-change" style="float:left;" />

      2. #### コマンドパレットから任意のファイルアイコンテーマを選択

         <img src="img/fileicon-select.png" alt="fileicon-select" style="float:left;" />

6. ## ワークスペース

   ワークスペース追加方法

   1. #### ファイル-ワークスペースにフォルダーを追加の順に選択する

      <img src="img/workspace_folder_add.png" alt="workspace-in-folder-add" style="float:left;" />

   2. #### ワークスペースとして追加するフォルダを選択する

      <img src="img/workspace-folder-select.png" alt="workspace-folder-select" style="float:left;" />

   3. #### ワークスペースとして選択されたフォルダの直下にworkspace.code-workspaceというファイルが作成される

      <img src="img/workspace-createfile.png" alt="workspace-createfile" style="float:left;" />

7. ## git除外設定

   1. ### ワークスペース直下に.gitignoreという名前のファイルを新規作成する

   2. ### 除外するファイル、フォルダを.gitignoreに記述する

      ```python
      CodeList/data_j.xls
      log/
      Lib/__pycache__
      Model/__pycache__
      ```

   3. ### ファイルが除外されない場合はキャッシュを削除する

      ```sh
      # ファイルの場合
      (projectDirectory)/git rm --cache <対象ファイル>
      # ディレクトリの場合
      (projectDirectory)/git rm -r --cache <対象ディレクトリ>
      ```

8. ## リポジトリクローン

   1. ### githubでリポジトリ作成

   2. #### Newをクリック

      <img src="img/github-new-repo_1.png" alt="github-new-repo_1" style="float:left;" />

   3. #### リポジトリ名を入力

   4. #### Privateに変更

   5. #### Add a README fileにチェックを入れる

   6. #### CreateRepositoryをクリック

      <img src="img/github-new-repo_2.png" alt="github-new-repo_2" style="float:left;" />

   7. ### リポジトリの場所となるフォルダを作成

      <img src="img/new-repository-place-create.png" alt="new-repository-place-create" style="float:left;" />

   8. ### VsCodeを起動する

      <img src="img/open-vscode-newwindow.png" alt="img/open-vscode-newwindow" style="float: left; zoom: 80%;" />

   9. ### ソース管理を開き、「リポジトリのクローン」をクリックする

      <img src="img/vscode-repository-clone.png" alt="vscode-repository-clone" style="float: left;zoom:80%;" />

   10. ### リポジトリURLを指定する

       <img src="img/input-repository-url.png" alt="input-repository-url" style="float: left;zoom:80%;" />

9. ## ブランチ作成

   1. ### 新しい分岐の作成

      <img src="img/new-branch-create.png" alt="new-branch-create" style="float: left;zoom:80%;" />

   2. ### ブランチ名を入力

      <img src="img/new-branch-inputname.png" alt="new-branch-inputname" style="float: left;" />

   3. ### 変更の発行(push)

      

10. ## ブランチマージ

    1. ### チェックアウト先をmainに変更する

       <img src="img/main-branch-select.png" alt="checkout-change" style="float: left;" />

    2. ### コマンドパレットにてGit:Mergeを選択(Ctrl+Shift+Pでコマンドパレット表示)

       <img src="img/branch-merge-select.png" alt="brancd-merge-select" style="float: left;" />

    3. ### マージ対象となるリポジトリを選択

       <img src="img/merge-repo-select.png" alt="merge-repo-select" style="float: left;" />

    4. ### マージ対象となるブランチを選択

       <img src="img/merge-branch-select.png" alt="merge-branch-select" style="float: left;" />

    5. ### mainリポジトリをコミットする

    6. ### githubへpushする

