# design-and-implementation

設計・実装カテゴリの Skill 一覧です。

## 目的

- 設計判断の品質向上
- 実装時の一貫性確保
- 変更時の安全性確保

## 対象 Skill

- feature-implementation-unified（現在運用中）
- data-model-design-unified（現在運用中）
- architecture-decision-record（現在運用中）
- api-contract-design（現在運用中）
- refactoring-safety（現在運用中）
- code-review-assistant（現在運用中）

## 選択ガイド

- 新規機能・仕様変更を進めたい: feature-implementation-unified
- 要件からデータモデルを固めたい: data-model-design-unified
- 設計判断を記録したい: architecture-decision-record
- API 契約を明確化したい: api-contract-design
- 安全にリファクタしたい: refactoring-safety
- レビュー観点を標準化したい: code-review-assistant

## 実装済み構成

- 010_api-contract-design/
- 020_architecture-decision-record/
- 030_code-review-assistant/
- 040_data-model-design-unified/
- 050_feature-implementation-unified/
- 060_refactoring-safety/

## 品質検証

- Skill を追加・更新した場合は [../VALIDATION_CHECKLIST.md](../VALIDATION_CHECKLIST.md) を実施する
- 判定は Pass / Fail の2値で記録する
- Fail は修正 PR に紐付けて再判定する
