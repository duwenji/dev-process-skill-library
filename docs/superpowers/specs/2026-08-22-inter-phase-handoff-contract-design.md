# 工程間 受け渡し契約（Handoff Contract）設計

> **⚠️ Superseded**: 本specは段階1〜14のリナンバリングと新ゲート追加という「プロセス」寄りの案だったが、
> ユーザーフィードバックにより本質は「ドキュメントの体系化」であると判明したため、
> [2026-08-22-deliverable-document-templates-design.md](./2026-08-22-deliverable-document-templates-design.md)
> に置き換えられた。段階番号・ゲートは変更しない方針となったため、本specの内容は実装しない。

- 日付: 2026-08-22
- 対象リポジトリ: dev-process-skill-library
- 種別: architectural（全16スキル横断のインターフェース変更）

## 背景・課題

現状、各Skillの Phase1（段階1-6）は「段階3 入力テンプレート」を個別に定義しているが、上流Skillの
Phase4（旧段階14, 報告）が出力する「次に渡す設計入力」には構造化フォーマットがない。そのため、

- 上流の出力と下流の入力テンプレートが独立して定義されており、対応関係が保証されない
- Phase1開始前に「上流からの入力が揃っているか」を確認する仕組みがない

「設計・実施工程でどのような要件情報が必要かを明確にし、成果物のTemplateがあればチェック可能になる」という
運用改善要望に対応するため、工程間の受け渡しを契約化する。

## 対象スキル（16件、Phase1-4ゲート構造を持つもの）

- 010: requirements-refinement
- 020: api-contract-design, architecture-decision-record, code-review-assistant,
  data-model-design-unified, feature-implementation-unified, refactoring-safety
- 030: defect-repair-unified, security-hardening, test-strategy-unified
- 040: observability-and-ops-readiness, performance-investigation, release-readiness
- 050: documentation-sync, incident-postmortem, known-how-ingestion

**対象外**: 060_development-method/010_ddd-ai-responsibility
（sub-skills/assets を持たない参照系スキルのため、Phase1-4構造が存在しない）

## 新設する共通基盤（2ファイル）

### `skills/shared-templates/handoff-deliverable-template.md`

各SkillのPhase4（報告、新段階15）が出力する「次工程への引き渡し」セクションの共通フォーマット。
スキル固有の追加成果物（ERD、API仕様、テスト結果等）は別セクションでそのまま維持する。

```markdown
## 次工程への引き渡し

| 項目 | 内容 |
|------|------|
| 確定事項・決定内容 | |
| 受入条件 / 成功基準 | |
| スコープ外 | |
| 制約（性能・互換性・納期等） | |
| 未決事項・残課題・既知リスク | |
| 参照リンク（関連ADR/仕様/データ辞書等） | |
| 次工程 Skill 名 | |

## スキル固有の成果物
- [各Skillのassetsテンプレート/出力物へのリンクまたは要約]
```

### `skills/shared-references/handoff-input-checklist.md`

各SkillのPhase1新設ゲート（段階1）が、前工程成果物を確認する際に使う共通チェックリスト。

```markdown
# 工程間 受け渡し 入力チェックリスト（共通）

前工程の成果物（handoff-deliverable-template.md 形式）を受け取ったら、段階1（前工程受け渡し確認ゲート）で
このチェックリストを使って確認する。

## 必須項目チェック
- [ ] 確定事項・決定内容が明記されている
- [ ] 受入条件 / 成功基準が判定可能な文で書かれている
- [ ] スコープ外が明記されている
- [ ] 制約（性能・互換性・納期等）が明記されている
- [ ] 未決事項・残課題・既知リスクが明記されている
- [ ] 参照リンクが有効/到達可能である

## 不足時の対応
- 1項目でも欠落している場合、AIは不足項目を一覧化し、開発者の正式入力段階（旧段階3）で確認を依頼する
- 前工程の成果物自体を上書き・修正はしない（history保持の原則）
- 確認結果は本skillの記録テンプレートに追記する

## 初回利用時（前工程成果物が存在しない場合）
- 開発者が直接要求を入力するフローとみなし、チェック結果を「該当なし（初回）」として記録する
- ゲートは形骸化させず、記録は必須とする
```

## リナンバリング方式（段階1〜14 → 1〜15）

新ゲートを正式な「段階1」とし、既存の段階1〜14はすべて+1する。Phase構成（Phase0〜4のくくり）は
以下のように変わる。

| 新段階 | 旧段階 | 内容 | 種別 |
|---|---|---|---|
| 段階1 | (新設) | 前工程受け渡し確認ゲート | 🟡⭐️ 新ゲート（開発者承認必須） |
| 段階2〜7 | 旧1〜6 | Phase1: 要求整理・方式候補化 | 変更なし（+1） |
| 段階8〜10 | 旧7〜9 | Phase2: 設計決定・実装（旧ゲート#1 含む） | 変更なし（+1） |
| 段階11〜14 | 旧10〜13 | Phase3: 検証・受入確認（旧ゲート#2, #3 含む） | 変更なし（+1） |
| 段階15 | 旧14 | Phase4: 報告 + 引き渡しセクション追加 | +1、出力拡張 |

ゲート数は3→4（新ゲート0系 + 既存ゲート#1〜#3）。「Phase 0: 前工程受け渡し確認」を新設し、既存Phase1〜4の
くくりと意味は維持する（中身の段階番号のみ+1）。

## 変更対象ファイル（1スキルあたり）

- `SKILL.md`: mermaid図に段階1(新ゲート)を追加、Phase概要・ゲート条件・完了条件・入力リファレンス・
  クイックパスの段階番号を+1、入力リファレンスに2つの共通ファイルへのリンクを追加
- `runbook.md`: 段階番号の言及箇所を+1
- `sub-skills/phase1-*.md`: 冒頭に段階1(新ゲート)の実施内容を追加、既存段階1-6の記述を段階2-7に読み替え
- `sub-skills/phase2-*.md`, `phase3-*.md`, `phase4-*.md`: 段階番号の言及箇所を+1
- `assets/*-log-template.md`: 段階番号の言及箇所を+1（あれば）

## 共通ドキュメント側の変更

- `.github/SKILL-template.md`: 新ゲート段階1を組み込んだ雛形に更新（今後の新規スキル作成が最初から準拠する）
- `skills/README.md`: 「共通運用ポリシー」に工程間受け渡し契約の説明と2つの共通ファイルへのリンクを追加
- `skills/VALIDATION_CHECKLIST.md` Layer A に追加:
  - 段階1が前工程受け渡し確認ゲートとして定義されている（初回利用時の代替条件を含む）
  - Phase4（段階15）の出力が `handoff-deliverable-template.md` の必須項目を含む

## スコープ外（フォローアップタスク）

- `spa-quiz-app` のクイズJSON（`operations-release.json`, `requirements-planning.json`,
  `design-implementation.json`, `learning-improvement.json`）が段階番号（段階3, 7, 11, 13等）を直接
  参照しており、リナンバリング後に不整合が生じる。本改修完了後、既存の「クイズ同期ガイド」手順に沿って
  別タスクとして追従する。今回のスコープには含めない。

## 完了条件

- 対象16スキル全てで新段階1(ゲート)が追加され、既存段階が1〜15に振り直されている
- 2つの共通ファイルが新設され、全16スキルの入力リファレンスからリンクされている
- SKILL-template.md, skills/README.md, VALIDATION_CHECKLIST.md が更新されている
- 各スキルのmermaid図が新しい段階構成と矛盾しない
- spa-quiz-appへの影響がフォローアップタスクとして記録されている（本スペックに明記済み）
