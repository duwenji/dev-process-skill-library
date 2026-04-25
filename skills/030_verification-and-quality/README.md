# verification-and-quality

検証・品質カテゴリの Skill 一覧です。

## 目的

- テスト戦略の標準化
- 品質検証の網羅性向上
- セキュリティ観点の組み込み

## 対象 Skill

- defect-repair-unified（現在運用中）
- test-strategy-unified（現在運用中）
- security-hardening（現在運用中）

## 選択ガイド

- 不具合の調査から改修まで進めたい: defect-repair-unified
- テスト方針を体系化したい: test-strategy-unified
- セキュリティ観点を強化したい: security-hardening

## 実装済み構成

- 010_defect-repair-unified/
- 020_security-hardening/
- 030_test-strategy-unified/

## 品質検証

- Skill を追加・更新した場合は [../VALIDATION_CHECKLIST.md](../VALIDATION_CHECKLIST.md) を実施する
- 判定は Pass / Fail の2値で記録する
- Fail は修正 PR に紐付けて再判定する
