---
name: performance-investigation
description: '計測に基づいて性能ボトルネックを特定し、仮説、改善案、検証計画を承認付きで整理するワークフロー。'
argument-hint: '性能問題の症状、発生条件、目標値、計測可能な情報、影響範囲を記述してください。開発者が承認判断を行います。'
user-invocable: true
disable-model-invocation: false
---

# 性能調査 Skill（統合フレームワーク）

## 利用する場面
- レスポンスや処理時間の遅さを調査したい
- 計測に基づいて改善したい
- 仮説と検証順序を整理したい
- 改善の根拠を残したい

## 対応の流れ（高レベル）

```mermaid
flowchart TD
    classDef ai fill:#d0e8ff,stroke:#4a90d9,color:#000
    classDef dev fill:#d0f0d0,stroke:#3d8b3d,color:#000
    classDef gate fill:#fff0c0,stroke:#c8900a,color:#000,stroke-width:3px
    classDef endNode fill:#e8e8e8,stroke:#888,color:#000

    subgraph Ph1["Phase 1: 症状整理・仮説抽出"]
        S1["段階1: 準備（症状と計測資産の収集）"]
        S2["段階2: 目標値と発生条件の詳細化"]
        S3["段階3: 開発者が調査条件を正式入力"]
        S4["段階4: AI がボトルネック候補を分析"]
        S5["段階5: AI が性能フローを生成"]
        S6["段階6: AI が改善仮説案を生成"]
        S1 --> S2 --> S3 --> S4 --> S5 --> S6
    end

    subgraph Ph2["Phase 2: 調査方針決定・計測計画化"]
        G1["⭐️段階7: 仮説案レビュー・決定<br/>ゲート条件 #1"]
        S8["段階8: AI が計測計画を作成"]
        S9["段階9: 開発者が内容をレビュー・承認"]
        G1 --> S8 --> S9
    end

    subgraph Ph3["Phase 3: 検証準備・改善比較"]
        S10["段階10: AI が確認項目を生成"]
        G2["⭐️段階11: 項目レビュー・承認<br/>ゲート条件 #2"]
        S12["段階12: AI が比較方法と例外を整理"]
        G3["⭐️段階13: 計画レビュー・承認<br/>ゲート条件 #3"]
        S10 --> G2 --> S12 --> G3
    end

    subgraph Ph4["Phase 4: 報告"]
        S14["段階14: AI が調査方針と期待効果を報告"]
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
| strict | 広い計測と複数比較を行う | 深刻な性能劣化 |
| speed | 最小限の仮説と計測に絞る | 早期トリアージ |
| balance | 計測コストと改善効果を両立する | 標準的な性能調査 |

## Phase（段階）の概要
### Phase 1: 症状整理・仮説抽出（段階1-6）
### Phase 2: 調査方針決定・計測計画化（段階7-9）
### Phase 3: 検証準備・改善比較（段階10-13）
### Phase 4: 報告（段階14）

## ゲート条件と承認フロー
### 段階7: 仮説案決定ゲート
判定条件:
- 症状と目標値が明確か
- 仮説と計測方法が対応しているか
- 優先順位が妥当か

### 段階11: 項目承認ゲート
判定条件:
- 計測項目と比較条件が明確か
- 再現条件が揃っているか
- 外的要因の扱いが決まっているか

### 段階13: 計画承認ゲート
判定条件:
- 期待効果が説明可能か
- 計測コストが許容範囲か
- 改善案比較ができるか

## 記録・証跡
- 各段階の内容を `AI改善/performance_investigation_${DATE}.md` に append-only で記録する
- 症状、目標値、仮説、計測方法、承認者を明記する

## 入力リファレンス
- 正本: runbook.md
- Phase 1 サブタスク: sub-skills/phase1-investigation.md
- Phase 2 サブタスク: sub-skills/phase2-measurement-planning.md
- Phase 3 サブタスク: sub-skills/phase3-comparison-validation.md
- Phase 4 サブタスク: sub-skills/phase4-reporting.md
- 記録テンプレート: assets/performance-investigation-log-template.md
