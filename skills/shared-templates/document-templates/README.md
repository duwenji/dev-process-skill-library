# 成果物文書テンプレート

各Skillが生成する成果物文書（要件定義書、設計仕様書、テスト戦略書 等）の型を定義する。
`assets/*-log-template.md`（Skill実行ログ・振り返り用）とは役割が異なり、こちらは
**対象プロジェクトの docs/ 配下等に配置される成果物文書そのもの**の型である。

IDプレフィックス規約は [../../shared-references/traceability-id-convention.md](../../shared-references/traceability-id-convention.md) を参照。

## 共通スケルトン

全テンプレートは以下の章立てを共通で持つ。種別固有の詳細章は「方針・決定事項」の直後に挿入する。

```markdown
## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | |
| ドキュメント種別 | |
| 対象システム/機能 | |
| 関連Skill | |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

## 方針・決定事項

<!-- 種別固有の詳細章はここに挿入 -->

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

## 索引（15種類）

| # | ファイル | 文書種別 | IDプレフィックス | 対応Skill |
|---|---|---|---|---|
| 1 | `requirements-definition-document-template.md` | 要件定義書 | REQ | 010_requirements-refinement |
| 2 | `feature-design-document-template.md` | 機能設計・実装方針書 | DES | 020_feature-implementation-unified |
| 3 | `data-model-design-document-template.md` | データモデル設計書 | DM | 020_data-model-design-unified |
| 4 | `adr-template.md` | ADR | ADR | 020_architecture-decision-record |
| 5 | `api-contract-document-template.md` | API契約書 | API | 020_api-contract-design |
| 6 | `code-review-standard-document-template.md` | コードレビュー実施基準書 | REV | 020_code-review-assistant |
| 7 | `refactoring-plan-document-template.md` | リファクタリング計画書 | RFC | 020_refactoring-safety |
| 8 | `defect-investigation-report-template.md` | 不具合調査・対応報告書 | DEF | 030_defect-repair-unified |
| 9 | `security-hardening-policy-template.md` | セキュリティ対策方針書 | SEC | 030_security-hardening |
| 10 | `test-strategy-document-template.md` | テスト戦略書 | TS | 030_test-strategy-unified |
| 11 | `operability-design-document-template.md` | 運用設計書 | OPS | 040_observability-and-ops-readiness |
| 12 | `performance-investigation-report-template.md` | 性能調査・計測報告書 | PERF | 040_performance-investigation |
| 13 | `release-plan-document-template.md` | リリース計画書 | REL | 040_release-readiness |
| 14 | `documentation-update-plan-template.md` | ドキュメント更新計画書 | DOC | 050_documentation-sync |
| 15 | `postmortem-report-template.md` | ポストモーテム報告書 | PMR | 050_incident-postmortem |

<!-- リンク化はTask18で全ファイル作成後に行う -->
