# 工程間トレーサビリティ ID規約

`shared-templates/document-templates/` 配下の成果物文書は、自身の「文書ID」を持ち、下流文書は
「対応元ID（トレーサビリティ）」セクションで上流の文書IDを逆参照する。中心集約のマトリクス文書は
作らず、各文書が上流への逆リンクを持つ形に留める。

## ID形式

`<プレフィックス>-<3桁連番>`（例: `REQ-001`）。連番はプロジェクト内・文書種別ごとに独立して振る。

## プレフィックス一覧

| プレフィックス | 文書種別 | 対応Skill |
|---|---|---|
| REQ | 要件定義書 | 010_requirements-refinement |
| DES | 機能設計・実装方針書 | 020_feature-implementation-unified |
| DM | データモデル設計書 | 020_data-model-design-unified |
| ADR | ADR（アーキテクチャ決定記録） | 020_architecture-decision-record |
| API | API契約書 | 020_api-contract-design |
| REV | コードレビュー実施基準書 | 020_code-review-assistant |
| RFC | リファクタリング計画書 | 020_refactoring-safety |
| DEF | 不具合調査・対応報告書 | 030_defect-repair-unified |
| SEC | セキュリティ対策方針書 | 030_security-hardening |
| TS | テスト戦略書 | 030_test-strategy-unified |
| OPS | 運用設計書 | 040_observability-and-ops-readiness |
| PERF | 性能調査・計測報告書 | 040_performance-investigation |
| REL | リリース計画書 | 040_release-readiness |
| DOC | ドキュメント更新計画書 | 050_documentation-sync |
| PMR | ポストモーテム報告書 | 050_incident-postmortem |

`PM` は [glossary.md](./glossary.md) で「プロジェクトマネージャー」として既に使われているため、
ポストモーテム報告書には `PMR` を用いる。

## 使い方

1. 文書作成時、文書情報の「文書ID」欄に `<プレフィックス>-<連番>` を採番する
2. 上流の成果物（要件定義書等）を参照する場合、「対応元ID（トレーサビリティ）」セクションに
   上流の文書IDを記載する
3. 最上流の文書（要件定義書など、本ライブラリ管理外の上位文書しか持たない文書）は
   「該当なし（最上流）」と記載する
4. 中心集約のトレーサビリティマトリクスは作成しない。追跡は各文書の逆リンクを辿って行う
