# Javaによる並行プログラミング入門（Java演習 講義資料）

Javaのスレッドを使った並行プログラミングの講義資料。
[Quarto](https://quarto.org/) の本（book）形式で書かれており、GitHub Pages でWebサイトとして公開する。

## フォルダ構成

```
.
├── _quarto.yml                  # 本全体の設定（題名、章の並び、見た目）
├── index.qmd                    # はじめに
├── chapters/
│   ├── 01-concurrent-parallel.qmd
│   ├── 02-process-thread.qmd
│   ├── 03-create-thread.qmd
│   ├── 04-race-condition.qmd    # 練習問題1・2を含む
│   ├── 05-volatile.qmd
│   ├── 06-synchronization.qmd   # 練習問題3を含む
│   ├── 07-deadlock.qmd          # 練習問題4を含む
│   └── 08-parallel.qmd          # 演習を含む
├── appendix/
│   └── glossary.qmd             # 付録：用語集
├── solutions/                   # 解答例（Git の管理外。公開日に1つずつ追加する）
│   ├── ex1.qmd                  # 練習問題1の解答例
│   └── …
├── styles.css                   # 見た目の微調整
├── INSTRUCTOR_NOTES.md          # 指導者向けメモ（サイトには公開されない）
└── .github/workflows/publish.yml  # GitHub Pages への自動公開
```

## 手元で確認する

1. [Quarto](https://quarto.org/docs/get-started/) をインストールする。
2. このフォルダで次のコマンドを実行する。ブラウザでプレビューが開き、原稿を保存するたびに自動で更新される。

```bash
quarto preview
```

## 初回の公開手順（1回だけ）

GitHub Actions による自動公開を使う前に、次の作業を1回だけ行う。

1. GitHub に新しいリポジトリを作り、このフォルダの内容を `main` ブランチに push する。
2. リポジトリの **Settings → Actions → General** を開き、**Workflow permissions** で **Read and write permissions** を選んで保存する。
3. 手元のPCで、このフォルダから次のコマンドを実行せよ。`gh-pages` ブランチと `_publish.yml` が作られ、サイトが公開される。

   ```bash
   quarto publish gh-pages
   ```

4. 作られた `_publish.yml` をコミットして push する。
以後は、`main` ブランチに push するたびに、GitHub Actions がサイトを作り直して公開する。
公開先は `https://<ユーザー名>.github.io/<リポジトリ名>/`。

**2回目以降は、手元で `quarto publish gh-pages` を実行しないこと。**
手元の `solutions/` にある未公開の解答例まで公開される（「[解答例の扱い](#解答例の扱い)」を参照）。

## 原稿の書き方のきまり

- 章を追加するときは、`chapters/` にファイルを作り、`_quarto.yml` の `chapters:` に追加する。章番号・節番号は自動で振られる。
- 練習問題の見出しには `{.unnumbered}` を付け、節番号が振られないようにしている。
- 他の節を参照するときは、見出しに付けた識別子（例：`{#sec-volatile}`）へのリンクを使う。
- 囲み枠は Quarto の callout を使う。ヒントは折りたたみ表示（`collapse="true"`）にしていいる。

| 用途 | 書き方 |
| --- | --- |
| 補足・参考 | `::: {.callout-note}` |
| ヒント | `::: {.callout-tip}` |
| よくある誤り・注意 | `::: {.callout-warning}` |
| 必ず覚えること | `::: {.callout-important}` |

## 解答例の扱い

解答例は `solutions/` フォルダで作成し、公開日に1つずつリポジトリへ追加して公開する。

- `solutions/` は `.gitignore` で除外しているため、通常の `git add` ではコミットされない。
- `_quarto.yml` の `render:` に `solutions/*.qmd` を指定している。公開済みの解答例だけが GitHub Actions の環境に存在するため、サイトには公開済みの解答例だけが作られる。
- 各練習問題の末尾には、解答例へのリンク（`../solutions/exN.html`）を置いている。公開前にリンクを開くと「ページが見つからない」と表示される。

**公開リポジトリは誰でも閲覧できる。公開日前の解答例をコミット・push しないこと。**
一度 push すると、後から削除しても Git の履歴に残る。

### 前日までの準備

1. 解答例を `solutions/` に作る。ファイル名は、練習問題からのリンク先に合わせる。

   | 練習問題 | ファイル名 |
   | --- | --- |
   | 練習問題1 | `solutions/ex1.qmd` |
   | 練習問題2 | `solutions/ex2.qmd` |
   | 練習問題3 | `solutions/ex3.qmd` |
   | 練習問題4 | `solutions/ex4.qmd` |

2. 最初の見出しに、識別子と `.unnumbered` を付ける。`.unnumbered` を付けないと章番号が振られる。

   ```markdown
   # 練習問題1の解答例 {#sec-sol-ex1 .unnumbered}
   ```

3. `quarto preview` で、表示と練習問題からのリンクを確かめておく。

### 公開日の作業

1. 最新の状態にする。

   ```bash
   git pull
   ```

2. （任意）サイドバーにも表示したい場合は、`_quarto.yml` の `chapters:` の末尾に「解答例」の部を追加し、**今回公開するファイルだけ**を書く。

   ```yaml
       - part: "解答例"
         chapters:
           - solutions/ex1.qmd
   ```

   **`chapters:` に未公開のファイルを書かないこと。** GitHub Actions の環境にそのファイルがないため、サイトの作成がエラーで止まり、講義ページ全体が更新されなくなる。

3. 練習問題の末尾にある案内文を、次のように書き換える。

   ```markdown
   ::: {.callout-note}
   ## 解答例
   [練習問題1の解答例](../solutions/ex1.html)を公開しました。
   :::
   ```

4. `quarto preview` で、練習問題のリンクから解答例が開けること、表示が崩れていないことを確かめる。

5. 公開する解答例を、**`-f` を付けて、ファイル名を1つずつ指定して**追加する。

   ```bash
   git add -f solutions/ex1.qmd
   git add _quarto.yml chapters/04-race-condition.qmd   # 変更した場合のみ
   git status
   ```

   `git status` の「Changes to be committed」に、`solutions/` の中では**今回公開するファイルだけ**が含まれていることを確かめる。
   **`git add -f solutions/`（フォルダごと）や `git add -f .` は使わないこと。** 未公開の解答例まで追加される。

6. コミットして push する。

   ```bash
   git commit -m "練習問題1の解答例を公開"
   git push
   ```

### 公開後の確認

1. GitHub の **Actions** タブで、「Quarto Publish」が成功したことを確かめる。
2. 公開サイトで、練習問題のリンクから解答例が開けることを確かめる。
3. 未公開の解答例が開けないことを確かめる。たとえば、公開サイトの URL の末尾を `solutions/ex2.html` にして、「ページが見つからない」と表示されれば正常である。

### 補足

- 一度 `git add -f` で追加したファイルは、以後 Git の管理下に入る。公開後に修正した場合は、通常どおり `git add solutions/ex1.qmd` でコミットできる。
- 手元で `quarto preview` や `quarto render` を実行すると、未公開の解答例も手元の `_book/` に作られる。`_book/` は `.gitignore` で除外しているため push はされないが、**手元で `quarto publish gh-pages` を実行すると公開されてしまう。**

## PDF版を作る（任意）

日本語のPDFを作るには、手元のPCに TeX Live（LuaLaTeX）が必要。
`_quarto.yml` の末尾にある PDF 用の設定の `#` を外してから、次のコマンドを実行せよ。
自動公開（GitHub Actions）ではPDFは生成されない。

```bash
quarto render --to pdf
```

## ライセンス

公開前に、資料の利用条件を決めて `LICENSE` ファイルを追加せよ（教材では CC BY 4.0 などがよく使われる）。