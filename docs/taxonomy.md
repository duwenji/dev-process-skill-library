# Skill 分類体系（Taxonomy）

Skill 全体を5カテゴリで管理します。カテゴリと物理フォルダ名の対応は以下の通りです。

| カテゴリ（論理） | フォルダ名（物理） | 目的 |
|---|---|---|
| 要件・計画 | requirements-and-planning | 要件定義、優先度判断、導入計画 |
| 設計・実装 | design-and-implementation | 設計判断、実装品質、契約設計 |
| 検証・品質 | verification-and-quality | テスト戦略、品質検証、セキュリティ検証 |
| 運用・リリース | operations-and-release | 監視、性能、リリース準備 |
| 学習・改善 | learning-and-improvement | 振り返りとドキュメント同期 |

## 分類ルール

- 各 Skill は必ず1カテゴリに帰属する
- 複数カテゴリに跨る場合は、主たる目的で判断する
- 判断が分かれる場合は Reviewer に諮る

## Skill 間の依存関係

- 要件・計画 → 設計・実装 → 検証・品質 → 運用・リリース の順で上流依存
- 学習・改善 は全カテゴリに対して横断的に作用する
