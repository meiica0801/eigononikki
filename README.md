# 英語日記プロジェクト 📓

英語学習のための日記練習用ワークスペース。Cursor のチャットで AI と一緒に進めます。

---

## 🎯 目的

- 英語の感覚を取り戻す → IELTS 6.5 を目標に積み上げる
- 「読める」止まりにせず、「書ける」に持っていく
- 毎日の小さな積み重ねを、無理なく続けられる仕組みにする

---

## 📚 3 つの学習フェーズ

| フェーズ | 期間の目安 | やること | 使うプロンプト |
| --- | --- | --- | --- |
| **Phase 1: 写経** | 最初の 1〜2 ヶ月 | AI が書いた英語日記を、手で書き写す。語彙・型・リズムを体に入れる。 | `prompts/01-shakyo.md` |
| **Phase 2: 穴埋め** | 2〜3 ヶ月目以降 | AI が書いた英語日記の一部を空欄にしてもらい、自分で埋める。 | `prompts/02-anaume.md` |
| **Phase 3: 自由作文＋添削** | 4 ヶ月目以降 | お題から自分で全部書いて、AI に添削してもらう。 | `prompts/03-sakubun.md` |

> フェーズはきっちり区切らなくて OK。気分や疲れ具合で 1 ↔ 2 ↔ 3 を行き来する想定。

---

## 🚀 使い方（毎日の流れ）

1. **お題を選ぶ**
   - `topics/` の中から気分に合うカテゴリを開いて、ピンと来たお題を 1 つ選ぶ
   - 迷ったら Cursor に「`prompts/topic-generator.md` を使ってお題を 3 つ出して」と頼む

2. **今日のフェーズのプロンプトを Cursor に渡す**
   - 例: Phase 1 なら `prompts/01-shakyo.md` の指示に従って AI に依頼
   - AI が日記を `diaries/YYYY-MM-DD.md` に書き出してくれる

3. **書く / 写す / 解く**
   - Phase 1: 紙のノートに英文を写す
   - Phase 2: 空欄を埋める → 答え合わせ
   - Phase 3: 自分で書く → 添削してもらう

4. **振り返り**
   - 知らなかった単語・表現は、その日の日記ファイルの下にメモしておくと後で見返せる

---

## 🗂️ フォルダ構成

```
eigononikki/
├── README.md                       このファイル
├── prompts/                        Cursor に渡すプロンプト集
│   ├── 00-profile.md               学習者プロフィール（共通コンテキスト）
│   ├── 01-shakyo.md                Phase 1: 写経モード
│   ├── 02-anaume.md                Phase 2: 穴埋めモード
│   ├── 03-sakubun.md               Phase 3: 自由作文＋添削モード
│   └── topic-generator.md          お題生成プロンプト
├── topics/                         お題ストック
│   ├── daily-life.md               日常の出来事
│   ├── emotions.md                 感情・気分
│   ├── design-and-work.md          デザイン・仕事
│   ├── observation.md              観察・気づき
│   ├── reading-and-writing.md      読書・短歌・エッセイ
│   └── ielts-style.md              IELTS 的なお題（後半用）
├── diaries/                        AI が書いた写経用日記
│   └── YYYY-MM-DD.md
└── app-spec/                       Web/PWA アプリの仕様書（実装は別リポジトリ）
    ├── requirements.md             要件定義書
    ├── design.md                   アプリ設計書
    └── kickoff-prompt.md           新プロジェクト起ち上げプロンプト
```

---

## 📱 Web/PWA アプリ化について

このリポジトリは **学習コンテンツ（日記・プロンプト・お題）の置き場** です。
生成された日記を活用する Web/PWA アプリ（読み上げ・単語帳・復習）の仕様は [`app-spec/`](./app-spec/) にまとめています。

- 要件定義: [`app-spec/requirements.md`](./app-spec/requirements.md)
- 設計書: [`app-spec/design.md`](./app-spec/design.md)
- 実装プロジェクト起ち上げ用プロンプト: [`app-spec/kickoff-prompt.md`](./app-spec/kickoff-prompt.md)

アプリ本体は別リポジトリ（例: `eigononikki-app`）で実装し、このリポジトリの `diaries/*.md` を GitHub Contents API 経由で読み込む構成です。

---

## 💡 続けるためのコツ

- **完璧主義を捨てる**：5 分でいい日、1 文だけの日があっていい
- **間違いを楽しむ**：Phase 3 の添削で間違いが多いほど学びが多い
- **興味のあるテーマに寄せる**：デザイン・ケア倫理・短歌・読書 etc. の topics は遠慮なく使う
- **同じお題を再利用していい**：3 ヶ月後にもう一度書いてみると成長が見える

---

## 🛠️ 育て方

- プロンプトは使いながら違和感があったらどんどん編集する
- お題が枯れてきたら `prompts/topic-generator.md` で量産して `topics/` に追記する
- 「こういうモードもほしい」と思ったら `prompts/` に新しいファイルを足す（例: `04-rewrite.md` リライト練習 など）

