# claude-routine — Daily Cloud Intelligence Report

このリポジトリは [Claude Routines](https://code.claude.com/docs/en/routines) で毎朝のクラウド動向レポートを自動生成するためのプロンプト置き場です。

> Claude Routines は Anthropic がホストするスケジュール実行機能で、cron 的なタイミングで Claude を起動し、結果をリポジトリにコミット（または PR）してくれます。**あなたの PC は起動していなくて構いません。**

## ファイル構成

```
prompts/daily-cloud-report.md   # Routine が参照するプロンプト本文
README.md
.gitignore
```

## セットアップ手順（claude.ai 側で 1 回だけ）

1. **https://claude.ai/code/routines** を開く
2. 「**New routine**」→ 名前を「Daily Cloud Report」等に
3. **Prompt** 欄: 下記のいずれか
   - `prompts/daily-cloud-report.md` の中身をコピペ
   - もしくは「`prompts/daily-cloud-report.md` の指示に従ってクラウド動向レポートを作成し、`reports/CloudReport_<YYYYMMDD>.md` として保存・コミットしてください」と書く
4. **Repository** 欄: `1110025y/claude-routine` を選択
5. **Trigger** 欄: 「Schedule」→ Daily / 09:00 (Asia/Tokyo)
6. **Environment** 欄: ネットアクセスを許可（web_search 利用のため）
7. **Create** で確定

## 実行結果の見方

- 各実行で Claude が `claude/<...>` ブランチを作成し、`reports/` 配下にレポートを置きます
- claude.ai/code/routines のダッシュボードで実行履歴・ログ・差分を確認できます
- PR 形式の場合は GitHub の Pull requests タブに毎日 1 つ追加されます

## カスタマイズ

| 変えたいもの | 変える場所 |
|---|---|
| 実行時刻 | claude.ai/code/routines の Routine 設定（Trigger） |
| プロンプト内容 | `prompts/daily-cloud-report.md` を編集（リポジトリ側） |
| 対象クラウド | 同上ファイルの「対象クラウド」記述 |
| 通知先 | Routine の Connectors で Slack / Email 等を追加 |

## 利用上限（参考）

- Pro プラン: 5 routines/日
- Max プラン: 15 routines/日
- Team / Enterprise: 25 routines/日
- 手動の One-off 実行は上限にカウントされません

## 参照

- [Automate work with routines — Claude Code Docs](https://code.claude.com/docs/en/routines)
- [Introducing Routines in Claude Code — Anthropic Blog](https://claude.com/blog/introducing-routines-in-claude-code)
