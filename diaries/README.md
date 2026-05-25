# Diaries — 日記の保存場所

## ファイル名のルール

```
YYYY-MM-DD.md          通常の日記
YYYY-MM-DD-2.md        同じ日に 2 つ書いたとき
```

## どんなときにここにファイルができる？

| フェーズ | ファイルを作るのは誰 |
| --- | --- |
| Phase 1（写経） | **AI が書く** — それを紙のノートに写す |
| Phase 2（穴埋め） | **AI が書く** — それを画面で読んで空欄を考える |
| Phase 3（自由作文） | **自分が書く** → そのファイルに AI が添削を追記 |

## Phase 3 で書くときの最小テンプレート

```markdown
# YYYY-MM-DD — [title]

> Topic:
> Mode: Phase 3
> Focus today (optional):

## My draft (English)


## What I wanted to say (Japanese)

```

> 詳しい使い方は [`../prompts/03-sakubun.md`](../prompts/03-sakubun.md) を参照。

## 振り返りに便利な運用（任意）

- 月のはじめに `2026-05-summary.md` のようなまとめファイルを作る
  - 今月よく使った表現
  - 何度も同じミスをした文法ポイント
  - 印象に残った 3 篇
- AI に「今月の日記を読み返して、伸びたところ・課題を 3 つずつ挙げて」と頼む
