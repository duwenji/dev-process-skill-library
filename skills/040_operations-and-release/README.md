# operations-and-release

運用・リリースカテゴリの Skill 一覧です。

## 目的

- 観測可能性の向上
- 性能課題の再現と改善
- リリース品質の安定化

## 対象 Skill

- observability-and-ops-readiness（現在運用中）
- performance-investigation（現在運用中）
- release-readiness（現在運用中）

## 選択ガイド

- ログ/メトリクス/アラートを整備したい: observability-and-ops-readiness
- 性能ボトルネックを調査したい: performance-investigation
- リリース前チェックを標準化したい: release-readiness

## 実装済み構成

- 010_observability-and-ops-readiness/
- 020_performance-investigation/
- 030_release-readiness/

## 品質検証

- Skill を追加・更新した場合は [../VALIDATION_CHECKLIST.md](../VALIDATION_CHECKLIST.md) を実施する
- 判定は Pass / Fail の2値で記録する
- Fail は修正 PR に紐付けて再判定する
