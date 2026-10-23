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

公開リポジトリは誰でも閲覧できるので、**解答例はリポジトリに入れないこと。**
解答例は別の非公開リポジトリで管理するか、授業後に別途配布する（`.gitignore` で `solutions/` フォルダを除外している）。

## PDF版を作る（任意）

日本語のPDFを作るには、手元のPCに TeX Live（LuaLaTeX）が必要。
`_quarto.yml` の末尾にある PDF 用の設定の `#` を外してから、次のコマンドを実行せよ。
自動公開（GitHub Actions）ではPDFは生成されない。

```bash
quarto render --to pdf
```

## ライセンス

公開前に、資料の利用条件を決めて `LICENSE` ファイルを追加せよ（教材では CC BY 4.0 などがよく使われる）。
