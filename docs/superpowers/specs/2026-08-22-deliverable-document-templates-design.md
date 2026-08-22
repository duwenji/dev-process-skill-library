# 成果物文書テンプレートの体系化 設計

- 日付: 2026-08-22
- 対象リポジトリ: dev-process-skill-library
- 種別: architectural（shared-templates新設 + 15スキル横断の参照配線）
- 関連: [2026-08-22-inter-phase-handoff-contract-design.md](./2026-08-22-inter-phase-handoff-contract-design.md)（**superseded** — 段階リナンバリング/新ゲート案は本specに置き換え。理由は下記「背景・経緯」参照）

## 背景・経緯

当初、工程間の情報連携を「Skillの実行プロセス（段階・ゲート）」の問題として設計し、段階1〜14を1〜15に
リナンバリングして新ゲートを追加する案（前身spec）を作成した。しかしユーザーからのフィードバックにより、
本質は次の点にあると判明した。

- Skillと同じくらい重要なのは「コンテキスト情報」である
- プロジェクト開発におけるコンテキストとは、開発の成果物であるドキュメントそのものである
- したがって改善の焦点は、プロセス（段階・ゲート）ではなく、**ドキュメントの体系化**（情報の構造を統一し、
  生成AIが後続セッション/別Skillでドキュメントを読んだ時に必要な情報を迷わず正確に拾えるようにすること）
  であるべき

このため、前身specの段階リナンバリング/新ゲート案は保留し、成果物文書そのものの型を整備する本designに
差し替える。

## あるべき成果物文書の一覧（洗い出し結果）

対象は「Phase1-4のゲート構造を持つ16スキル」から、Skill自体を成果物とする `known-how-ingestion` を除いた
**15スキル**。各スキルの既存 SKILL.md Phase概要にある「出力」記述から、現に生成されている実質的な成果物を
抽出した。

| #  | カテゴリ | Skill                           | 成果物文書                    | 既存テンプレート状況                         |
| -- | -------- | ------------------------------- | ----------------------------- | -------------------------------------------- |
| 1  | 010      | requirements-refinement         | 要件定義書                    | なし（ログのみ）                             |
| 2  | 020      | feature-implementation-unified  | 機能設計・実装方針書          | なし（ログのみ）                             |
| 3  | 020      | data-model-design-unified       | データモデル設計書            | 部分あり（ERDガイド/データ辞書テンプレート） |
| 4  | 020      | architecture-decision-record    | ADR（アーキテクチャ決定記録） | なし                                         |
| 5  | 020      | api-contract-design             | API契約書                     | なし                                         |
| 6  | 020      | code-review-assistant           | コードレビュー実施基準書      | なし                                         |
| 7  | 020      | refactoring-safety              | リファクタリング計画書        | なし                                         |
| 8  | 030      | defect-repair-unified           | 不具合調査・対応報告書        | なし（ログのみ）                             |
| 9  | 030      | security-hardening              | セキュリティ対策方針書        | なし                                         |
| 10 | 030      | test-strategy-unified           | テスト戦略書                  | 部分あり（テストケーステンプレート）         |
| 11 | 040      | observability-and-ops-readiness | 運用設計書                    | なし                                         |
| 12 | 040      | performance-investigation       | 性能調査・計測報告書          | なし                                         |
| 13 | 040      | release-readiness               | リリース計画書                | なし                                         |
| 14 | 050      | documentation-sync              | ドキュメント更新計画書        | なし                                         |
| 15 | 050      | incident-postmortem             | ポストモーテム報告書          | なし                                         |

**対象外**:

- `known-how-ingestion`（成果物がSkill草案そのもの。Intake/Structuring/Codification/Publishingという
  別構造を持ち、既にCodificationステージでSkill草案という「型」を生成しているため対象外）
- `ddd-ai-responsibility`（Phase1-4構造を持たない方法論ガイドのため対象外。前身specから継続して対象外）

## 設計方針: 共通スケルトン + 種別固有の詳細章

15種類を個別にゼロ設計せず、2層構造にする。

- **共通スケルトン**: 全15文書で章立て・項目名・順序を統一する部分。AIがどの文書を読んでも「決定事項」
  「未決事項」「参照リンク」を同じ見出しで機械的に発見できるようにする。
- **種別固有の詳細章**: 文書種別ごとの専門的な内容（受入条件、ERD、比較した代替案 等）。既存の
  「出力」記述をそのまま章立てに落とし込む。

### 共通スケルトン（全テンプレート共通）

```markdown
## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | [本文書自身のID。例: REQ-001] |
| ドキュメント種別 | [固定文言。例: 要件定義書] |
| 対象システム/機能 | |
| 関連Skill | [例: 010_requirements-refinement] |
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
| [例: REQ-001] | [対応元の要件/決定の要約] | 対応済 / 一部対応 / 未対応 |

上流文書が存在しない場合（要件定義書など最上流の文書）は「該当なし（最上流）」と記載する。

## 方針・決定事項

<!-- ここに種別固有の詳細章が入る（下記「種別固有の詳細章」参照） -->

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

### 種別固有の詳細章（15種類）

既存 SKILL.md の「出力」記述（Phase1〜3）から抽出。各テンプレートは共通スケルトンの
「方針・決定事項」の直後にこの詳細章を挿入する。

1. **要件定義書**: 機能要件 ／ 非機能要件 ／ 受入条件 ／ 用語定義（必要時）
2. **機能設計・実装方針書**: 現行仕様との差分分析 ／ 方式案比較（採用方式と理由） ／ 実装方針 ／
   変更ファイル一覧・実装根拠
3. **データモデル設計書**: 概念モデル（ERD） ／ 論理モデル ／ 物理モデル ／ データ辞書 ／ DDL方針 ／
   移行・互換性メモ
4. **ADR**: 判断テーマ ／ 検討した代替案と比較軸 ／ 採用案と採否理由 ／ 影響範囲・追従タスク
5. **API契約書**: エンドポイント一覧 ／ リクエスト・レスポンス仕様 ／ エラー表 ／ 互換性方針・移行メモ
6. **コードレビュー実施基準書**: レビュー観点一覧 ／ 指摘分類基準 ／ 確認手順 ／ 改善候補リスト
7. **リファクタリング計画書**: 対象範囲・分割単位 ／ 変更順序と確認方法 ／ 保証観点（回帰確認範囲） ／
   差分リスクと戻し方
8. **不具合調査・対応報告書**: 事象・再現手順 ／ 原因調査結果 ／ 対応方針と実装内容 ／
   検証結果（正常系・境界値・異常系・回帰）
9. **セキュリティ対策方針書**: 資産・脅威一覧 ／ 脅威分析 ／ 対策方針と適用範囲 ／ 残余リスク台帳
10. **テスト戦略書**: 影響分析・既存テスト棚卸し ／ テスト層別方針（単体/結合/受入等） ／ 優先順位表 ／
    実行チェックリスト
11. **運用設計書**: 観測性現状評価と課題 ／ 監視・アラート設計 ／ 初動手順 ／ 引き継ぎ事項
12. **性能調査・計測報告書**: 症状整理・ボトルネック仮説 ／ 計測計画と条件 ／ 比較結果 ／ 改善提案
13. **リリース計画書**: リリース条件・依存関係 ／ 投入手順とロールバック手順 ／ 周知事項 ／ 最終確認結果
14. **ドキュメント更新計画書**: 影響文書一覧 ／ 更新対象分析 ／ 更新順序・計画 ／ 未更新リスク
15. **ポストモーテム報告書**: 事実整理・時系列 ／ 原因分析（仮説と検証） ／ 再発防止アクション ／ 共有事項

### IDプレフィックス規約（トレーサビリティ）

各文書は自身の「文書ID」（例: `REQ-001`）を持ち、下流文書はこのIDを「対応元ID」欄で逆参照する。
プロジェクト内で種別ごとに連番（3桁）を振る。中心集約のマトリクス文書は作らず、各文書が上流IDへの
逆リンクを持つ形に留める（運用負荷を最小化するため）。

| 文書種別 | プレフィックス | 例 |
|---|---|---|
| 要件定義書 | REQ | REQ-001 |
| 機能設計・実装方針書 | DES | DES-001 |
| データモデル設計書 | DM | DM-001 |
| ADR | ADR | ADR-001 |
| API契約書 | API | API-001 |
| コードレビュー実施基準書 | REV | REV-001 |
| リファクタリング計画書 | RFC | RFC-001 |
| 不具合調査・対応報告書 | DEF | DEF-001 |
| セキュリティ対策方針書 | SEC | SEC-001 |
| テスト戦略書 | TS | TS-001 |
| 運用設計書 | OPS | OPS-001 |
| 性能調査・計測報告書 | PERF | PERF-001 |
| リリース計画書 | REL | REL-001 |
| ドキュメント更新計画書 | DOC | DOC-001 |
| ポストモーテム報告書 | PMR | PMR-001 |

`PM` は `shared-references/glossary.md` で「プロジェクトマネージャー」の略として既に使われているため、
ポストモーテム報告書は `PMR` を用いて衝突を避ける。

この規約は新設の `shared-references/traceability-id-convention.md` に定義し、`glossary.md` からも
相互参照する。

## 既存資材との役割分担

- `assets/*-log-template.md`（既存、変更なし）: **この Skill 実行時に何をしたか**の記録・振り返り用
  （AI改善ログ、append-only）
- `shared-templates/document-templates/*.md`（新設）: **対象プロジェクトの成果物そのもの**の型。
  実行のたびに `docs/` 配下等に配置され、後続の別セッション/別Skillが読むコンテキストになる

両者は別物であり、統合しない。ログは「証跡」、文書テンプレートは「成果物の型」という役割分担を維持する。

## ファイル配置

```
skills/shared-references/
  traceability-id-convention.md                    # IDプレフィックス規約（新設）

skills/shared-templates/document-templates/
  README.md                                        # 共通スケルトンの説明 + 15種類の索引
  requirements-definition-document-template.md      # 1
  feature-design-document-template.md                # 2
  data-model-design-document-template.md              # 3
  adr-template.md                                     # 4
  api-contract-document-template.md                   # 5
  code-review-standard-document-template.md           # 6
  refactoring-plan-document-template.md               # 7
  defect-investigation-report-template.md             # 8
  security-hardening-policy-template.md               # 9
  test-strategy-document-template.md                  # 10
  operability-design-document-template.md             # 11
  performance-investigation-report-template.md        # 12
  release-plan-document-template.md                   # 13
  documentation-update-plan-template.md                # 14
  postmortem-report-template.md                        # 15
```

データモデル設計書・テスト戦略書は、既存の `erd-best-practices.md` / `data-dictionary-template.md` /
`testcase-template.md` / `test-case-csv-template.md` を「詳細章の中で参照する」形にし、重複定義しない。

## 各スキルへの配線（15スキル共通の変更パターン）

前身specと異なり、**段階番号・ゲート・mermaid図は一切変更しない**。以下3箇所の追記のみ。

1. **SKILL.md 入力リファレンス**: `成果物文書テンプレート: ../../shared-templates/document-templates/<file>.md` を追加
2. **SKILL.md Phase概要の該当「出力」行**: 既存の記述はそのまま維持し、末尾に
   `（<file>.md 形式で作成）` を追記
3. **SKILL.md 完了条件**: `成果物文書が <file>.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている` を追加。上流を持つSkillは
   `対応元IDが上流文書のIDで埋まっている（最上流のSkillを除く）` も追加

sub-skills/runbook/assets は変更不要（ログと文書テンプレートの役割分担を維持するため）。

## 共通ドキュメント側の変更

- `.github/SKILL-template.md`: 入力リファレンスの雛形に「成果物文書テンプレート」「トレーサビリティID規約」
  の行を追加（今後の新規スキル作成時から標準で組み込まれるようにする）
- `skills/README.md`: 「共通運用ポリシー」に「成果物文書テンプレート」節を新設し、15種類の索引表・
  IDプレフィックス表・`shared-templates/document-templates/README.md` と
  `shared-references/traceability-id-convention.md` へのリンクを追加
- `skills/shared-references/glossary.md`: 「略語・日本語対応表」に15種のIDプレフィックス
  （REQ, DES, DM, ADR, API, REV, RFC, DEF, SEC, TS, OPS, PERF, REL, DOC, PMR）を追加し、
  `traceability-id-convention.md` を参照させる
- `skills/VALIDATION_CHECKLIST.md` Layer A に追加:
  - 対象Skillの場合、入力リファレンスに成果物文書テンプレートへの参照リンクが存在する
  - 完了条件に成果物文書の必須章充足（対応元ID含む）が含まれている

## スコープ外（フォローアップ）

- `spa-quiz-app` のクイズJSONは段階番号を参照しているが、本designは段階番号を変更しないため
  **今回は影響なし**（前身specの懸念は解消）
- 15種類の詳細章は現状の「出力」記述からの抽出に留めており、章内の具体的な記入例（サンプル文）は
  今回のspecには含めない。実装計画で各テンプレートファイルを書く際に具体化する

## 完了条件

- `shared-templates/document-templates/` に15テンプレート + README(索引)が作成されている
- 各テンプレートが共通スケルトン（文書ID・対応元ID含む）+ 該当する種別固有の詳細章を満たしている
- `shared-references/traceability-id-convention.md` が作成され、15種類のIDプレフィックスが定義されている
- 対象15スキルすべてで、SKILL.mdの入力リファレンス・該当出力行・完了条件が更新されている
- `.github/SKILL-template.md`, `skills/README.md`, `skills/VALIDATION_CHECKLIST.md`, `glossary.md` が更新されている
- 前身spec（inter-phase-handoff-contract-design.md）が superseded として明記されている
