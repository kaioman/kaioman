# git関連

## git

1. インストール

## github

1. アカウント作成
2. リポジトリ作成
3. リポジトリクローン
4. リポジトリ削除
5. VsCodeからgithubへプッシュ

## gitbookとgithubの連携

### 1. gitbookアカウント作成

   [gitbook.com](https://www.gitbook.com/)

   ※gitbookでドキュメントを直接編集しなければFreeプランで問題ない

### 2. ログイン後、左端にあるIntegretionsからgithubとの連携を有効にする

   ![integretions](img/github_to_gitbook_01.png)

### 3. githubとgitbookの認証連携の承認画面が表示されるので承認する(初回のみ)

### 4. 連携するgithub側のリポジトリを指定する

   ![Authority](img/github_to_gitbook_03.png)

### 5. リンクするリポジトリをmasterに限定するか指定

   *「Sync "master" branch only」を指定

   ![linkRepo](img/github_to_gitbook_02.png)

### 6. コンテンツの編集元を指定

   *「I write my content on GitHub」を指定

   ![linkRepo](img/github_to_gitbook_04.png)

### 7. Go Liveをクリックしてgithubのリポジトリの内容が表示されることを確認する

## gitbook.comで作成したドキュメントの公開

### 1. [Share]→[Visibility]から Public を選択

   ![SharedSetting](img/github_to_gitbook_05.png)

### 2. Shareable Linkにて公開用のURLが確認できる

   ![ShareableLink](img/github_to_gitbook_06.png)
