# learning-and-improvement

学習・改善カテゴリの Skill 一覧です。

## 目的

- インシデント学習の定着
- ドキュメント同期の仕組み化
- 改善ループの継続
- 暗黙知・ノウハウの形式知化とライブラリ蓄積

## 対象 Skill

- incident-postmortem（現在運用中）
- documentation-sync（現在運用中）
- known-how-ingestion（現在運用中）

## 選択ガイド

- 障害の再発防止を仕組み化したい: incident-postmortem
- 実装変更と文書更新を同期したい: documentation-sync
- チャットで話したノウハウをライブラリに取り込みたい: known-how-ingestion
- 既存 Skill を改善・マージ・廃止したい: known-how-ingestion

## 実装済み構成

- 010_documentation-sync/
- 020_incident-postmortem/
- 030_known-how-ingestion/

## 品質検証

- Skill を追加・更新した場合は [../VALIDATION_CHECKLIST.md](../VALIDATION_CHECKLIST.md) を実施する
- 判定は Pass / Fail の2値で記録する
- Fail は修正 PR に紐付けて再判定する
