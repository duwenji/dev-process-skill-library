# skills

このディレクトリは、Skill をカテゴリ単位で管理します。

共有テンプレートは `shared-templates/` に配置し、新規 Skill 実装時の共通雛形として利用します。
共通参照ファイルは `shared-references/` に配置し、複数 Skill で再利用します。

## カテゴリ一覧

- requirements-and-planning: 要件・計画
- design-and-implementation: 設計・実装
- verification-and-quality: 検証・品質
- operations-and-release: 運用・リリース
- learning-and-improvement: 学習・改善
- development-method: 開発方法論

## 共有資産

- shared-templates: 共通テンプレート
- shared-references: 共通参照

## 現在の移行ステータス

- 旧配置（直下配置）: なし（移行完了）
- 新配置（カテゴリ配下）:
  - design-and-implementation/feature-implementation-unified ✅
  - design-and-implementation/data-model-design-unified ✅
  - design-and-implementation/architecture-decision-record ✅
  - design-and-implementation/api-contract-design ✅
  - design-and-implementation/refactoring-safety ✅
  - design-and-implementation/code-review-assistant ✅
  - requirements-and-planning/requirements-refinement ✅
  - verification-and-quality/defect-repair-unified ✅
  - verification-and-quality/test-strategy-unified ✅
  - verification-and-quality/security-hardening ✅
  - operations-and-release/release-readiness ✅
  - operations-and-release/performance-investigation ✅
  - operations-and-release/observability-and-ops-readiness ✅
  - learning-and-improvement/incident-postmortem ✅
  - learning-and-improvement/documentation-sync ✅
  - development-method/010_ddd-ai-responsibility ✅
  - カテゴリ別 README: 全6カテゴリ作成済み ✅
  - SKILL.md frontmatter を skill schema 準拠へ統一: 済み ✅
  - docs/, governance/, templates/ 新設: 済み ✅
  - shared-templates/ 配下の共通テンプレート整備: 済み ✅
  - shared-references/ 配下の共通参照整備: 済み ✅

## 移行ルール（要約）

- Skill は `カテゴリ/skill-name/` に配置する
- skill-name は kebab-case を使用する
- 各 Skill に `SKILL.md` を必須とする
- カテゴリは親ディレクトリで管理し、`SKILL.md` frontmatter には記載しない

## 実装順の目安

- Phase 0: shared-templates/ を起点にテンプレートを決める
- Phase A: requirements-refinement, architecture-decision-record, test-strategy-unified, security-hardening, release-readiness
- Phase B: data-model-design-unified, api-contract-design, refactoring-safety, code-review-assistant, performance-investigation, observability-and-ops-readiness
- Phase C: incident-postmortem, documentation-sync
- 全提案 Skill 実装: 完了
