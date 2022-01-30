# twitter API

1. ## TwitterのAPI登録申請

   1. [デベロッパーサイト](https://developer.twitter.com/en/apps/)へアクセスする

   2. Create an appボタンをクリックする

      <img src="img\create_app.png" style="zoom:75%; float:left;" />

   3. applyボタンをクリックする

      <img src="img\apply-button.png" alt="apply-button" style="zoom:75%; float:left;" />

      > 「既存のアプリは引き続き管理できますが、新しいアプリの作成やTwitterプレミアムAPIの利用を希望する場合は、
      >
      > 開発者アカウントを申請してください。
      >
      > 開発者向けプラットフォームとして、私たちの第一の責任はユーザーに対するものであり、Twitterでの会話の健全性をサポートする場を提供することです。私たちのプラットフォームの不正使用を引き続き防止するため、開発者向けにいくつかの新しい要件を導入しています。」

   4. いくつかの質問に答える。入力し終わったらNextボタンをクリックする。

      <img src="img\answer-questions.png" alt="answer-questions" style="zoom:75%; float:left" />

      > This @username will be used to log in to your account.

      アプリと関連付けるTwitterアカウントを指定する

      

      > Important messages will be sent to this email address. 

      重要な通知を受け取るEメールアドレスを指定する

      

      > What’s your name?

      ニックネームを入力する。なんでもいい。

      

      > What country are you based in?

      国名を選択する。もちろんJapan

      

      > What's your use case?

      利用用途を選択する。botを作りたければ「Making a bot」を指定。

      

      > Will you make Twitter content or derived information available to a government entity or a government affiliated entity?

      「Twitterのコンテンツや派生情報を政府機関や政府の関連団体が利用できるようにしますか？」という質問。

      「No」を選択で問題ない(はず)

      

   5. Twitter APIの利用目的を答える

      > **In English, please describe how you plan to use Twitter data and/or APIs. The more detailed the response, the easier it is to review and approve.**

      Twitter APIまたはTwitterデータの利用方法を教えて下さい、という質問。

      (例文)

      > 1.I want to use Twitter's API to automatically tweet quotes from around the world.
      > 2.The frequency of tweeting is about once a day.
      > 3.Even if I use Twitter's API to get contents from Twitter, I will not display them outside of Twitter.
      > 4.I want to inspire even one person with what I tweet.

      (日本語訳)

      > 1.TwitterのAPIを使って、世界中の名言を自動でツイートしたい。
      >
      > 2.つぶやく頻度は、1日1回程度です。
      >
      > 3.TwitterのAPIを使ってTwitterからコンテンツを取得しても、Twitter外では表示しない。
      >
      > 4.私がつぶやくことで、一人でも多くの人を笑顔にしたい。

      

      > **Are you planning to analyze Twitter data?**

      「Twitterのデータを利用しますか？」という質問。今回は特に利用しないのでNoを選択。

      

      > **Will your app use Tweet, Retweet, like, follow, or Direct Message functionality?**

      「アプリはツイート、リツイート、お気に入り、フォロー、ダイレクトメッセージを利用するか？」という質問。利用しないのでNoを選択。

      （よくよく考えたらツイート機能は利用しているのでYesとすべきだったかも)

      「My application uses a tweet function」(アプリはツイート機能を利用する)と回答しておけば問題ない。

      

      > **Do you plan to display Tweets or aggregate data about Twitter content outside of Twitter?**

      「Twitter以外のTwitterコンテンツに関するツイートを表示したり集計データを表示するか？」という質問。Noを選択。

      

      > **Will your product, service or analysis make Twitter content or derived information available to a government entity?**

      「あなたの製品・サービス，または分析によって，Twitterコンテンツまたは派生情報が政府機関が利用可能になりますか？」という質問。

      よくわからないのでNoを選択。

      以上の質問に対する回答を入力して設定すると、確認画面に進むので内容を確認して次へ進む。

      

   6. 利用規約に同意する

      <img src="img\policy-agree.png" alt="policy-agree" style="zoom:75%; float:left" />

      「同意する」にチェックを入れてSubmitボタンをクリック

      

   7. e-mailを認証する

      <img src="img\confirm-email.png" alt="confirm-email" style="zoom:75%; float:left" />

      

2. ## アプリの権限変更

   1. [Developer Portal](https://developer.twitter.com/en/portal/dashboard)のDashboardを開く

   2. アプリ一覧にある歯車マークをクリックする

      <img src="img\portal-dashboard.png" alt="portal-dashboard" style="zoom:75%; float:left" />

   3. **User authentication settings**でEditボタンをクリックする(未設定の場合はSetupボタン)

      <img src="img\authentication-settings.png" alt="authentication-settings" style="zoom:75%; float:left" />

   4. OAuth 1.0aを有効にし、App permissionsを「Read and write and Direct message」に変更する

      <img src="img\OAuth-1.0a.png" alt="OAuth-1.0a" style="zoom:75%; float:left" />

   5. URLを設定する(必須)

      <img src="img\url-setting.png" alt="url-setting" style="zoom:75%; float:left" />

      上記赤線部分を設定する。他は空欄でも可。

      設定するURLも形式が不正でなければ存在しないアドレスでも指定可。

      上記ではCallbackURIにループバックアドレス、Website URLにgithubのリポジトリアドレスを指定している。

      設定が終わったらSaveをクリックする。

      

3. ## キートークンの発行

   必ず2.の手順を実行してから行うこと。

   1. ダッシュボードのアプリ一覧にある鍵マークをクリックする

      <img src="img\portal-dashboard-key.png" alt="portal-dashboard-key" style="zoom:75%; float:left" />

   2. コンシューマキーとアクセストークンを発行する

      <img src="img\keys-and-token.png" alt="keys-and-token" style="zoom:75%; float:left" />

      それぞれRegenerateボタンをクリックして発行する。

      ボタン押下後、キー情報が表示された別ウィンドウが表示されるので忘れずに控えておくこと。

      