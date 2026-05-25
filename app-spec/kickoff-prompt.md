# 新プロジェクト起ち上げプロンプト

> このファイルは、新しい空フォルダで Cursor を開いて **Eigononikki アプリの実装プロジェクトを始める** ときに、最初に貼るためのプロンプト集です。
> 使い方: 新フォルダで Cursor を開く → このファイルの該当セクションをチャットにコピペ → AI に進めてもらう。

---

## 0. 前提（事前準備）

新プロジェクトを始める前に、メイカさん側で以下を準備しておく:

1. **GitHub に `eigononikki` リポジトリを public で push する**（既にあれば不要）
   - これがアプリのデータソースになる
2. **新しい空フォルダを作る**（例: `~/Desktop/eigononikki-app`）
3. **そのフォルダで Cursor を開く**
4. **このリポジトリの `app-spec/` フォルダを参照可能にする**
   - 方法 A: `app-spec/` を新プロジェクトに **コピー** する（`requirements.md` と `design.md`）
   - 方法 B: `eigononikki` リポジトリをワークスペースに **追加** して両方開く

以下のプロンプトは、`requirements.md` と `design.md` が新プロジェクト内で参照できる前提です。

---

## 1. 初期セットアップ依頼プロンプト（Step 1: プロジェクト初期化）

以下をそのまま Cursor チャットに貼ってください。

````
@app-spec/requirements.md と @app-spec/design.md を読んでください。
このプロジェクトでこれから実装を進めます。

まずは Step 1 として、以下のセットアップだけ行ってください:

1. Next.js 15 + TypeScript + Tailwind CSS + App Router でプロジェクトを初期化
   - コマンド: `npx create-next-app@latest . --typescript --tailwind --app --eslint --src-dir --import-alias "@/*" --no-turbopack`
   - 既存ファイル（requirements.md, design.md, kickoff-prompt.md）は残してください
2. 以下の依存を追加:
   - `dexie` — IndexedDB ラッパ
   - `gray-matter` — markdown frontmatter
   - `unified`, `remark-parse`, `remark-gfm`, `mdast-util-to-string` — markdown パース
   - `@types/mdast`（dev） — mdast 型定義
3. shadcn/ui を初期化:
   - `npx shadcn@latest init -d`
   - 最初に必要なコンポーネントだけ追加: button, card, input, badge, dialog, tabs, toast
4. ディレクトリの空ファイル枠を design.md の §8 通りに作成（中身は空でも雛形でもよい）:
   - `src/lib/{github,markdown,db,srs,tts,word-normalize}.ts`
   - `src/types/index.ts`
   - `src/components/{DiaryCard,TtsControls,Flashcard,ParagraphHighlighter}.tsx`
   - `src/hooks/{useDiaries,useTts,useSettings}.ts`
   - `tests/` ディレクトリ
   - `src/fixtures/sample-diary.md`（後で本物の日記をコピーします）
5. README.md に「このアプリは eigononikki プロジェクトの活用レイヤです」と一文と、requirements.md と design.md へのリンクを書いてください
6. Vitest を dev dependency に追加し、`tests/` を実行できる最小設定を入れてください

完了したら、`npm run dev` で起動し、http://localhost:3000 が見られる状態にしてください。
````

---

## 2. 型定義依頼プロンプト（Step 2）

````
@app-spec/design.md の §3「データモデル」の通りに、
`src/types/index.ts` に型定義を実装してください。

- DiaryEntry, PatternNote, VocabularyItem, Phase などのメモリ上ドメインモデル
- DiaryCache, WordLearningState, WordOccurrence, DiaryReviewState, ParagraphScore, AppSettings の IndexedDB スキーマ

`src/lib/db.ts` には Dexie クラス AppDB の定義（design.md §3.2 のサンプル通り）を実装してください。
````

---

## 3. markdown パーサ実装依頼プロンプト（Step 3）

````
@app-spec/design.md の §4「markdown パース仕様」に従って、
`src/lib/markdown.ts` を実装してください。

事前に、eigononikki リポジトリの diaries/2026-05-25.md と diaries/2026-05-20.md を
src/fixtures/ にコピーしてください（テスト用フィクスチャ）。

実装後、tests/markdown.test.ts に Vitest のテストを書いてください:
- フィクスチャをパースして DiaryEntry の各フィールドが期待通りに埋まること
- 同日複数日記（YYYY-MM-DD-2.md）のファイル名で variant が正しく抽出されること
- Phase 1 の構造（English / 日本語訳 / 写経のポイント / Vocabulary）がすべて抽出されること

`npm test` で全テストが通る状態にしてください。
````

---

## 4. GitHub クライアント実装依頼プロンプト（Step 4）

````
@app-spec/design.md の §9「GitHub Contents API の使い方」に従って、
`src/lib/github.ts` を実装してください。

- fetchDiaryList(repo): GitHub Contents API で diaries/ の一覧を取得
- fetchDiaryContent(file): raw markdown を取得
- レート制限ヘッダ（X-RateLimit-Remaining, X-RateLimit-Reset）を返り値に含める

実装後、`src/hooks/useDiaries.ts` で:
1. Dexie の DiaryCache から既存データを即時返す
2. バックグラウンドで GitHub から一覧取得 → SHA 差分のあるものだけ raw 取得
3. パースして DiaryCache を更新、UI に新データを反映

の流れを実装してください。
````

---

## 5. Diary List / Detail 画面実装依頼プロンプト（Step 5）

````
@app-spec/design.md の §6.2 に従って、以下の 2 画面を実装してください:

1. `/diaries` (Diary List)
   - useDiaries で取得した一覧を日付降順で表示
   - 検索ボックスとフィルタ
   - DiaryCard コンポーネントを使う

2. `/diaries/[id]` (Diary Detail)
   - 該当 DiaryEntry を取得
   - ヘッダ（日付・タイトル・Topic・Phase バッジ）
   - 英文・日本語訳・写経のポイント・Vocabulary テーブル
   - TTS は次の Step で実装するので、ボタンの枠だけ置いてください

ホーム画面 (`/`) からも遷移できるよう、最低限のナビゲーションを layout.tsx に追加してください。
````

---

## 6. 以降のステップ

Step 6 以降は design.md §15「開発の進め方（実装順）」を参照。各ステップでチャットに同様のプロンプトを書く形で進めてください。

- Step 6: TTS（読み上げ + ハイライト）
- Step 7: Vocabulary List
- Step 8: Word Quiz
- Step 9: Translation Quiz
- Step 10: Settings + Export/Import
- Step 11: PWA 化
- Step 12: Vercel デプロイ

---

## 7. このプロンプトを育てるためのメモ

- 実装中に Cursor とのやり取りで「うまく伝わらなかった」「もっと先回りで指示しておけばよかった」と思った内容は、このファイルに追記して次のステップで再利用してください
- requirements.md / design.md の主要論点（クイズ出題方式・SRS・通知・カラー）は **すでに確定済み**（requirements.md §8 参照）。実装中に「やっぱり違う方がよかった」と気づいたら、まず両ドキュメントを更新してから実装を変える順序を守ってください
- ステップごとに git commit して、後で振り返れるようにすると、UX デザイナー視点での改善ポイントも見つけやすくなります

---

## 8. デバッグ Tips

- **GitHub API レート制限に引っかかった**: `X-RateLimit-Reset` のタイムスタンプ（unix 秒）を `new Date(reset * 1000)` で確認
- **Web Speech API で音が出ない（iOS Safari）**: ユーザー操作起点（クリック）でないと発話できない仕様。テスト時は必ずボタンクリックで発火
- **IndexedDB のスキーマを変えた**: Dexie の version() を上げる。開発中なら devtools の Application → IndexedDB から消すのが早い
- **markdown パースで段落が崩れる**: remark-parse の AST を一度 `console.dir(tree, { depth: null })` で見ると構造把握が早い
