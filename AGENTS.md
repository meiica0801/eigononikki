# このリポジトリでの作業ルール

このリポジトリは、英語日記学習アプリ「Eigononikki」のデータソース（日記・トピック・プロンプト集）です。
Cursor Agent や他の AI コーディングエージェントが本リポジトリで作業するときは、以下のルールに従ってください。

## 日記を新規生成・追加したとき

1. `diaries/YYYY-MM-DD.md` を作成または更新する
   - 同じ日に複数バリエーションがある場合は `diaries/YYYY-MM-DD-a.md`, `-b.md` のようにする
2. 作業が完了したら、必ずユーザーに **「git に push しますか？」と確認する**
3. ユーザーが Yes と答えたら、以下を実行する:

   ```bash
   git add diaries/<実際のファイル名>
   git commit -m "Add YYYY-MM-DD diary"
   git push
   ```

4. ファイル名と commit メッセージの日付は **実際のファイル名から取得する** こと
   （ユーザーの口頭指示やうろ覚えに頼らない。タイポ防止のため）
5. 同じ日に複数ファイルを追加した場合は、それらをまとめて 1 コミットにする

## 既存の日記を編集したとき

- 上記と同様に、編集後に push 確認をする
- commit メッセージは `Update YYYY-MM-DD diary` 形式

## トピック / プロンプト / README を変更したとき

- 該当ファイルを add → commit → push する流れは同じ
- commit メッセージはわかりやすい英語で（例: `Update prompts/02-anaume.md`, `Add new topic: emotions`）

## 注意事項

- `.DS_Store` などの不要ファイルを巻き込まないこと（`.gitignore` に登録済みのはずだが念のため `git status` で確認）
- `diaries/README.md` の編集と日記ファイルの追加は **分けてコミット** すると履歴が読みやすい
- push の前に `git status` と `git diff --cached` で内容を確認するクセをつける
