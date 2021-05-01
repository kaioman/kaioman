# Markdown

## Markdownの記述

### ヘッダー

* 行の先頭に\#をつけることでH1～H6まで作成できる
* \#の数で文字サイズを調整可能

### 連番付きリスト

* 書き方

```bash
1. item1
2. item2
3. item3
```

* 結果
* item1
* item2
* item3

### コードブロック

* 記事内に\[\`\`\`\]を入れることでコードブロックを挿入できる。また、先頭の記号の後ろに言語名を入れることでシンタックスハイライトが適用される。

  \(例\)centosにてvimでcrontabを開くコマンド

  ```bash
    vim /etc/crontab
  ```

### テーブル

* Markdownテーブルの基本記法。以下は表題ありの３行３列テーブルの例

```text
| 列名１    | 列名２     | 列名３    | 
| --------- | --------- | --------- | 
| １行１列目 | １行２列目 | １行３列目 | 
| ２行１列目 | ２行２列目 | ２行３列目 | 
| ３行１列目 | ３行２列目 | ３行３列目 |
```

* プレビュー

| 列名１ | 列名２ | 列名３ |
| :--- | :--- | :--- |
| １行１列目 | １行２列目 | １行３列目 |
| ２行１列目 | ２行２列目 | ２行３列目 |
| ３行１列目 | ３行２列目 | ３行３列目 |

* 【便利ツール】Markdownテーブルを作成するツール

  [NotePM:https://notepm.jp/markdown-table-tool](https://notepm.jp/markdown-table-tool)

### 文書間リンク

```text
[xxxx](/xxxx.md)
```

※\(\)内は相対ファイルパスで記述

### シンタックスハイライト一覧

* シンタックスハイライトの一覧掲載

  [List of supported languages and lexers](https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers)

### 画像埋め込み

* 画像タイトルなし

  ```bash
    ![代替テキスト](画像のURL)
  ```

* 画像タイトルあり

  ```bash
    ![代替テキスト](画像のURL, "[画像タイトル]")
  ```
