# claude-routine — Daily Cloud Intelligence Report

毎朝 JST 09:00 に GitHub Actions が起動し、Claude API（`claude-opus-4-7` + web_search ツール）で AWS / Azure / Google Cloud / OCI の最新動向を収集して、Markdown と DOCX のレポートを `reports/` 配下に自動コミットします。あなたの PC は起動していなくて構いません。

## 仕組み

```
GitHub Actions (cron: 0 0 * * * UTC = 09:00 JST)
   │
   ├─ scripts/generate_report.py を実行
   │     └─ Anthropic SDK + web_search で当日の動向を調査・Markdown 生成
   │     └─ python-docx で DOCX に変換
   │
   └─ reports/CloudReport_YYYYMMDD.md / .docx をコミット & push
```

## ファイル構成

```
.github/workflows/daily-cloud-report.yml   # cron + push の Workflow
prompts/daily-cloud-report.md              # プロンプト本文（{{TODAY}} / {{YESTERDAY}} 置換）
scripts/generate_report.py                 # 生成スクリプト
requirements.txt                           # anthropic, python-docx
reports/                                   # 自動生成物（コミットされる）
```

## 初回セットアップ（GitHub 側で 1 回だけ手動）

1. **GitHub Secrets に `ANTHROPIC_API_KEY` を登録**
   - Repository Settings → Secrets and variables → Actions → "New repository secret"
   - Name: `ANTHROPIC_API_KEY`
   - Value: `sk-ant-api03-...`（[Anthropic Console](https://console.anthropic.com/settings/keys) で発行）

2. **Workflow の権限確認**
   - Repository Settings → Actions → General → Workflow permissions
   - 「Read and write permissions」を有効化（自動コミット push のため）

## 手動実行（テスト）

GitHub Actions の "Daily Cloud Report" → "Run workflow" でいつでも実行可能。

ローカル実行:

```bash
export ANTHROPIC_API_KEY=sk-ant-...
pip install -r requirements.txt
python scripts/generate_report.py
# → reports/CloudReport_YYYYMMDD.{md,docx} が生成される
```

## モデル / コスト

- モデル: `claude-opus-4-7`（adaptive thinking, effort=high, web_search_20260209 server tool）
- 1 回の実行で概算 30-150K input tokens / 5-15K output tokens 程度。
- コストを抑えたい場合は `scripts/generate_report.py` の `MODEL = "claude-opus-4-7"` を `claude-sonnet-4-6` に変更。

## カスタマイズ

- **実行時刻を変える**: `.github/workflows/daily-cloud-report.yml` の `cron` を編集（UTC 表記）
- **出力フォーマット変更**: `prompts/daily-cloud-report.md` の Section 構造を編集
- **対象クラウド追加**: 同プロンプト内の「対象クラウド」記述を追加

## 注意事項

- レポート内容は web_search 結果に依拠するため、料金・SLA・EOL の最終判断は必ず各社公式ドキュメントで確認してください。
- メール送信は本リポジトリには含まれていません。必要になった場合は SMTP / SendGrid / Gmail API を別途追加してください。
