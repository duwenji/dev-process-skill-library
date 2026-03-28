---
name: documentation-sync
description: '実装変更と文書更新の対象、優先順位、更新タイミングを整理し、ドキュメント陳腐化を防ぐ運用ワークフロー。'
argument-hint: '対象変更、影響する文書、更新の締切、読者、未更新リスクを記述してください。開発者が承認判断を行います。'
user-invocable: true
disable-model-invocation: false
---

# ドキュメント同期 Skill（運用フレームワーク）

## 利用する場面
- 実装変更に追従して文書を更新したい
- どの文書をどこまで直すべきか整理したい
- 更新漏れや陳腐化を防ぎたい
- 更新タイミングと責任を明確にしたい

## 対応の流れ（高レベル）

```mermaid
flowchart TD
    classDef ai fill:#d0e8ff,stroke:#4a90d9,color:#000
    classDef dev fill:#d0f0d0,stroke:#3d8b3d,color:#000
    classDef gate fill:#fff0c0,stroke:#c8900a,color:#000,stroke-width:3px
    classDef endNode fill:#e8e8e8,stroke:#888,color:#000

    subgraph Ph1["Phase 1: 更新対象整理"]
        S1["段階1: 準備（変更内容と既存文書の収集）"]
        S2["段階2: 読者と更新優先度の詳細化"]
        S3["段階3: 開発者が同期条件を正式入力"]
        S4["段階4: AI が更新対象と不足を分析"]
        S5["段階5: AI が実装から文書への影響フローを生成"]
        S6["段階6: AI が更新案を生成"]
        S1 --> S2 --> S3 --> S4 --> S5 --> S6
    end

    subgraph Ph2["Phase 2: 更新方針決定・実行計画化"]
        G1["⭐️段階7: 更新案レビュー・決定<br/>ゲート条件 #1"]
        S8["段階8: AI が更新計画を作成"]
        S9["段階9: 開発者が内容をレビュー・承認"]
        G1 --> S8 --> S9
    end

    subgraph Ph3["Phase 3: 適用確認・例外整理"]
        S10["段階10: AI が確認項目を生成"]
        G2["⭐️段階11: 項目レビュー・承認<br/>ゲート条件 #2"]
        S12["段階12: AI が未更新リスクと例外を整理"]
        G3["⭐️段階13: 同期準備レビュー・承認<br/>ゲート条件 #3"]
        S10 --> G2 --> S12 --> G3
    end

    subgraph Ph4["Phase 4: 報告"]
        S14["段階14: AI が同期内容を報告"]
    end

    END(["✅ 完了"])

    S6 --> G1
    S9 --> S10
    G3 --> S14
    S14 --> END

    class S1,S2,S4,S5,S6,S8,S10,S12,S14 ai
    class S3,S9 dev
    class G1,G2,G3 gate
    class END endNode
```

## 実行モード（推奨: balance）
| モード | 特徴 | 用途 |
|--------|------|------|
| strict | 読者、導線、周辺資料まで広く更新対象を確認する | 大きな仕様変更 |
| speed | 必須文書に絞って更新漏れを防ぐ | 小規模変更 |
| balance | 読者影響と更新コストを両立する | 標準的な文書同期 |

## Phase（段階）の概要
### Phase 1: 更新対象整理（段階1-6）
### Phase 2: 更新方針決定・実行計画化（段階7-9）
### Phase 3: 適用確認・例外整理（段階10-13）
### Phase 4: 報告（段階14）

## ゲート条件と承認フロー
### 段階7: 更新案決定ゲート
判定条件:
- 変更影響を受ける文書が整理されているか
- 読者と優先度が明確か
- 更新しない文書の理由があるか

### 段階11: 項目承認ゲート
判定条件:
- 更新対象と更新内容が対応しているか
- 更新タイミングと責任が明確か
- 例外項目が管理されているか

### 段階13: 同期準備承認ゲート
判定条件:
- 未更新リスクが見えているか
- 後続フォローが定義されているか
- 読者にとって十分な更新になっているか

## 記録・証跡
- 各段階の内容を `AI改善/documentation_sync_${DATE}.md` に append-only で記録する
- 対象文書、更新優先度、例外、承認者を明記する

## 入力リファレンス
- 正本: runbook.md
- Phase 1 サブタスク: sub-skills/phase1-guideline-definition.md
- Phase 2 サブタスク: sub-skills/phase2-execution-planning.md
- Phase 3 サブタスク: sub-skills/phase3-feedback-and-adjustment.md
- Phase 4 サブタスク: sub-skills/phase4-continuous-improvement.md
- 記録テンプレート: assets/documentation-sync-log-template.md
