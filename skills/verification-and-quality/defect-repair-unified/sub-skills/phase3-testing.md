# Phase 3 Sub-Skill: テスト・チェック（段階10-13）

## 概要
Phase 3 では、改修点を網羅したテストチェック項目を生成し、開発者の承認を得た上で実施、結果を記録します。

## 責務
- **AI**: チェック項目生成（段階10）、テスト実施（段階12）
- **開発者**: チェック項目承認（段階11）、結果承認（段階13）

## ゲート条件 #2: 段階11 進行可否判定

**判定項目**:
- [ ] チェック項目が改修点を十分にカバーしているか
- [ ] テスト方法が実現可能か
- [ ] ダミー実装対象が明示されているか（あれば）

**判定結果**:
- **OK**: 段階12 へ進行可能
- **NG**: 段階10 へ返却（チェック項目追加/修正）

## ゲート条件 #3: 段階13 進行可否判定

**判定項目**:
- [ ] 全チェック項目について実施結果が記録されているか
- [ ] 不合格項目について原因・対応が明示されているか
- [ ] ビルド・静的チェック成功か

**判定結果**:
- **OK**: Phase 4（報告）へ進行可能
- **NG**: 段階12 へ返却（テスト追加/修正）

---

## 段階ごとの入出力

### 段階10: AI がチェック項目を生成

**チェック項目設計の原則**:
1. **改修点を網羅** — 段階8 で変更した全ロジック・分岐をカバー
2. **品質優先** — 正常系 + 境界値 + 異常系
3. **実施可能性** — 自動テスト / 手動テスト / ダミー実装 を分類

**改修点の回想** (段階8から):
- ProcessDataRecord() メソッドで DataRecordExtensions を使用
- NULL値時に既定値を適用
- 新規テストケース追加

**チェック項目生成ロジック**:

#### 正常系
- 通常の入力（全てフィールドが有効値）
- 既存テストケースで大部分カバー

#### 境界値
- NULL: DISPLAY_NAME=NULL, RECORD_ID=NULL, CATEGORY_CODE=NULL（各単独/組合わせ）
- 空文字列: DISPLAY_NAME="", RECORD_ID=""
- 最大値/最小値: CATEGORY_CODE の min/max

#### 異常系
- 型不一致: CATEGORY_CODE に文字列が入っている
- DB接続エラー: DB接続失敗時の例外ハンドリング

**チェック項目表**:
```markdown
## 段階10: AI がチェック項目を生成（2026-03-27 16:30:00 +09:00）

### チェック項目一覧

| TRK | 分類 | テスト項目 | 実施方法 | 期待結果 | 備考 |
|-----|------|----------|--------|--------|------|
| TRK-001 | 正常系 | 全フィールド有効値で処理実行 | 自動（ユニットテスト） | データ項目を正常に処理 | 既存テスト ProcessDataRecord_ValidInput() |
| TRK-002 | 境界値 | DISPLAY_NAME = NULL で処理実行 | 自動（新規テスト） | 既定値「(未設定)」を適用、正常処理継続 | DataRecordExtensions.GetStringOrNull() の動作確認 |
| TRK-003 | 境界値 | RECORD_ID = NULL で処理実行 | 自動（新規テスト） | 既定値「」を適用、正常処理継続 | 前後の処理で影響がないこと確認 |
| TRK-004 | 境界値 | CATEGORY_CODE = NULL で処理実行 | 自動（新規テスト） | 既定値 0 を適用、正常処理継続 | 商域コード用の適切な既定値の確認 |
| TRK-005 | 境界値 | DISPLAY_NAME, RECORD_ID 両方 NULL | 自動（新規テスト） | 両既定値を適用、正常処理継続 | 複合NULL条件の確認 |
| TRK-006 | 異常系 | CATEGORY_CODE に文字列が入っている場合 | 自動（新規テスト） | Exception ハンドリング、ログ出力 | GetInt32OrNull() で型チェック実施 |
| TRK-007 | 異常系 | DB接続エラー時の Exception ハンドリング | ダミー実装 | エラーログ出力、上位への例外伝播 | Mock DataRecord で SqlException を投出 |
| TRK-008 | 統合 | 複数台の DS telegram 連続処理 | 手動/自動 | 各telegram が独立に処理される、メモリリークなし | ストレステスト的観点 |
| TRK-009 | 正常系 | 既存ユニットテスト群全実行（回帰避止） | 自動 | 全テスト合格（既存テスト影響なし） | 回帰テスト |

### ダミー実装対象

| TRK | 対象 | ダミー実装概要 |
|-----|------|--------------|
| TRK-007 | DB接続エラー時の処理 | Mock<IDataRecord> でSqlException を投出、catch ブロック動作確認 |

### テスト実行計画

1. **自動テスト** (段階12前半)
   - ユニットテスト: Visual Studio / dotnet test で実行
   - コマンド: `dotnet test ProcDataSyncTest.csproj`
   - 期待: 全テスト合格 (9項目のうち自動7項目 + 既存回帰テスト)

2. **ダミー実装テスト** (段階12中盤)
   - Mock データで DB接続エラーシナリオ再現
   - Exception がログに出力されることを確認

3. **手動テスト** (段階12後半, 必要な場合)
   - 複数telegram 連続処理（TRK-008）

### 実施方法別の詳細

#### 自動テスト (Moq, NUnit例)
\`\`\`csharp
[Test]
public void ProcessDataRecord_NullDisplayName()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(r => r.GetOrdinal("DISPLAY_NAME")).Returns(0);
    mockRecord.Setup(r => r["DISPLAY_NAME"]).Returns(DBNull.Value);
    mockRecord.Setup(r => r["RECORD_ID"]).Returns("ID001");
    mockRecord.Setup(r => r["CATEGORY_CODE"]).Returns(100);
    
    var processor = new DataProcessor(_mockLogger);
    
    // Act
    processor.ProcessDataRecord(mockRecord.Object);
    
    // Assert
    Assert.That(processor.LastProcessedEntry.DisplayName, Is.EqualTo("(未設定)"));
}
\`\`\`

#### ダミー実装テスト
\`\`\`csharp
[Test]
public void ProcessDataRecord_DBConnectionError()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(r => r["DISPLAY_NAME"]).Throws(new SqlException(...));
    
    var processor = new DataProcessor(_mockLogger);
    
    // Act & Assert
    Assert.That(() => processor.ProcessDataRecord(mockRecord.Object), 
        Throws.TypeOf<SqlException>());
    
    _mockLogger.Verify(l => l.Error(It.IsAny<string>()), Times.Once);
}
\`\`\`

---

**次: 段階11（チェック項目承認）へ進む**
```

---

### 段階11: 開発者がチェック項目をレビュー・承認 ⭐️ **ゲート条件 #2**

**開発者の作業**:
1. 段階10 のチェック項目を確認
2. 改修点のカバー度を評価
3. テスト方法の実現可能性を判定
4. ダミー実装の必要性を確認

**記録フォーマット**:
```markdown
## 段階11: 開発者がチェック項目をレビュー・承認（2026-03-27 17:00:00 +09:00）

### レビュー内容

#### チェック項目の十分性
**総数**: 9項目（自動7 + ダミー実装1 + 手動1）

**分類別カバー度**:
- 正常系: 2項目 ✓ OK（既存+回帰）
- 境界値: 3項目 ✓ OK（NULL単独/組合わせ）
- 異常系: 2項目 ✓ OK（型不一致 + DB接続エラー）
- 統合: 1項目 ✓ OK（連続処理）
- 回帰: 1項目 ✓ OK（既存テスト全実行）

**評価**: ✓ 十分にカバー。改修点（NULL判定・既定値適用）が網羅されている

#### テスト実施方法の実現可能性

**自動テスト (7項目)**:
- ✓ ユニットテスト環境あり（ProcDataSyncTest.csproj）
- ✓ Moq/NUnit 使用可能（既存テストと同技術スタック）
- ✓ 実施時間: 1分以内

**ダミー実装 (1項目)**:
- ✓ Mock<IDataRecord> で SqlException 投出可能
- ✓ 既存パターン（他プロジェクトのテストで確認）
- ✓ 実施時間: 5分以内

**手動テスト (1項目)**:
- ✓ テスト環境で複数telegram送受信スクリプト実行可能
- ✓ 実施時間: 15分以内

**評価**: ✓ 全項目実施可能

#### ダミー実装対象の確認

**TRK-007 (DB接続エラー時のハンドリング)**:
- ✓ DB接続実機テストは時間制約があるため、Mock化は妥当
- ✓ Exception ハンドリング（catch, ログ出力）は Mock で十分検証可能
- ✓ 本番環境の DB エラーは別途監視カバー

**評価**: ✓ ダミー実装の妥当性を承認

### 最終判定

**レビュー結果**: ✓ **承認** — 段階12 へ進行可能

**指摘事項**: なし

**条件・制約**: 
- 自動テスト実行環境（Visual Studio 2022 or dotnet CLI）の確認
- Mock ライブラリ（Moq）が ProcDataSyncTest.csproj に参照されていることを確認

**承認日時**: 2026-03-27 17:00:00
**承認者**: [開発者名]

---
```

**ゲート条件チェック** (段階12 進行可否):
- [ ] チェック項目が改修点を網羅しているか ✓
- [ ] テスト方法が実現可能か ✓
- [ ] ダミー実装対象が明示されているか ✓

---

### 段階12: AI が動作確認を実施

**テスト実施フロー**:

#### 1. 自動テスト実行
```powershell
cd ProcDataSyncTest
dotnet test ProcDataSyncTest.csproj --verbosity detailed

# 期待出力例:
# Testing: ProcDataSyncTest
# TRK-001: ProcessDataRecord_ValidInput ... PASSED
# TRK-002: ProcessDataRecord_NullDisplayName ... PASSED
# TRK-003: ProcessDataRecord_NullRecordId ... PASSED
# TRK-004: ProcessDataRecord_NullCategoryCode ... PASSED
# TRK-005: ProcessDataRecord_BothNullFields ... PASSED
# TRK-006: ProcessDataRecord_InvalidCategoryCodeType ... PASSED
# TRK-009: Regression Test Suite ... PASSED (42 tests)
# 
# Test Run Successful.
# Total: 49 tests, Passed: 49, Failed: 0
```

#### 2. ビルド確認
```powershell
dotnet build ProcDataSync.csproj
# Expected: Build succeeded. (Warning count: 0)
```

#### 3. 静的チェック
```powershell
# StyleCop / Code Analyzers
dotnet build ProcDataSync.csproj /p:EnforceCodeStyleInBuild=true
# Expected: No code style violations
```

#### 4. ダミー実装テスト実行
```
TRK-007: DB接続エラー時の処理
- Mock を使用して SqlException を投出
- ProcessDataRecord() が exception を catch して、ログ出力するか確認
- Result: ✓ PASSED
```

**テスト結果ログ**:
```markdown
## 段階12: AI が動作確認を実施（2026-03-27 17:30:00 +09:00）

### テスト実行結果サマリ

**実施日時**: 2026-03-27 17:30:00

**総チェック項目**: 9

| 項目 | TRK | 状態 | 結果 | 備考 |
|------|-----|------|------|------|
| 正常系 | TRK-001 | ✓ 実施 | PASSED | 既存テスト合格 |
| 境界値 | TRK-002 | ✓ 実施 | PASSED | NULL/既定値適用確認 |
| 境界値 | TRK-003 | ✓ 実施 | PASSED | NULL/既定値適用確認 |
| 境界値 | TRK-004 | ✓ 実施 | PASSED | NULL/既定値適用確認 |
| 境界値 | TRK-005 | ✓ 実施 | PASSED | 複合NULL/既定値適用確認 |
| 異常系 | TRK-006 | ✓ 実施 | PASSED | 型不一致ハンドリング |
| 異常系 | TRK-007 | ✓ ダミー実装 | PASSED | DB接続エラーハンドリング（Mock） |
| 統合 | TRK-008 | ✓ 実施 | PASSED | 連続処理、メモリリーク なし |
| 回帰 | TRK-009 | ✓ 実施 | PASSED | 既存テスト全42件合格 |

**合格率**: 100% (9/9)

### ビルド確認結果

**プロジェクト**: ProcDataSync.csproj, ProcDataSyncTest.csproj

**ビルド結果**: ✓ 成功

**エラー/警告**: 0

**静的チェック**: ✓ PASSED (StyleCop, Code Analyzers)

### ダミー実装実施内容

#### TRK-007: DB接続エラー時の処理

**実装内容**: Mock<IDataRecord> で SqlException を投出

**テストコード**:
\`\`\`csharp
[Test]
public void ProcessDataRecord_DBConnectionError()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    // SqlException を投出する設定
    mockRecord.Setup(r => r["DISPLAY_NAME"])
        .Throws(new SqlException("Connection failed"));
    
    var processor = new DataProcessor(_mockLogger);
    
    // Act & Assert
    var ex = Assert.Throws<SqlException>(() => {
        processor.ProcessDataRecord(mockRecord.Object);
    });
    
    // ログ出力を確認
    _mockLogger.Verify(
        l => l.Error(It.Is<string>(s => s.Contains("Connection failed"))),
        Times.Once
    );
}
\`\`\`

**実施結果**: ✓ PASSED

**備考**: Mock を使用することで、DB接続エラーシナリオを安全に재現

### 異常隠し防止チェック

**チェック項目**:
- [X] 同じ Exception を複数キャッチしていないか → OK
- [X] エラーログが出力されているか → OK
- [X] リソースリークがないか（connection close 等） → OK
- [X] NULL判定ロジックが損なわれていないか → OK

**結果**: ✓ 異常隠し防止 OK

### テスト環境情報

- **OS**: Windows Server 2022
- **.NET**: 8.0.0
- **Unit Test Framework**: NUnit 3.x
- **Mock Library**: Moq 4.x
- **実行時間**: 約3分（全9項目）

---

**次: 段階13（テスト結果承認）へ進む**
```

---

### 段階13: 開発者がテスト結果をレビュー・承認 ⭐️ **ゲート条件 #3**

**開発者の作業**:
1. 段階12 のテスト結果を確認
2. 結果が承認基準を満たしているか判定
3. ビルド・静的チェック成功を確認
4. 本番環境へのリリース可否を最終判定

**記録フォーマット**:
```markdown
## 段階13: 開発者がテスト結果をレビュー・承認（2026-03-27 18:00:00 +09:00）

### テスト結果最終確認

#### 全チェック項目の実施状況
**総項目数**: 9
**合格**: 9 ✓
**不合格**: 0
**スキップ**: 0
**合格率**: 100%

#### ビルド・静的チェック確認
**ビルド**: ✓ 成功（Warning なし）
**StyleCop**: ✓ PASSED
**Code Analyzers**: ✓ PASSED

#### ダミー実装確認
**TRK-007 (DB接続エラー)**
- ✓ Mock で SqlException を安全に再現
- ✓ Exception ハンドリング正常作動確認
- ✓ ログ出力確認

### 承認基準との対照

| 基準 | 状態 | 評価 |
|------|------|------|
| 全チェック項目が「合格」または「ダミー実装で対応」 | ✓ 9/9 PASSED | OK |
| 不合格項目がない | ✓ 0/9 | OK |
| ビルドエラー/WARNING なし | ✓ 0件 | OK |
| 静的チェック PASSED | ✓ 両方 OK | OK |

### 最終品質判定

**品質判定**: ✓ **OK** — リリース可能

**確認事項**:
- ✓ 改修点（NULL判定、既定値適用）が十分にテストされている
- ✓ 既存機能への影響（回帰）がないことを確認
- ✓ エラーハンドリングが正確に動作
- ✓ 本番環境へのリリース準備完了

### 承認ステータス

**最終承認**: ✓ **承認済** — Phase 4（報告）へ進行可能

**承認日時**: 2026-03-27 18:00:00
**承認者**: [開発者名]

**次のステップ**: 段階14（最終報告）

---
```

**ゲート条件チェック** (Phase 4 進行可否):
- [ ] 全チェック項目が「合格」または「ダミー実装」か ✓
- [ ] ビルド・静的チェック成功か ✓
- [ ] 承認ステータスが「承認済」か ✓

---

## Phase 3 完了チェックリスト

### 各段階の完了確認
- [ ] 段階10: AI がチェック項目を生成（9項目、実施方法明示）
- [ ] 段階11: 開発者がチェック項目をレビュー・承認
- [ ] 段階12: AI がテスト実施（全チェック PASSED）
- [ ] 段階13: 開発者がテスト結果を最終承認

### ゲート条件「段階11」の満たし方
- [ ] チェック項目が改修点を網羅しているか
- [ ] 実施方法（自動/ダミー/手動）が実現可能か

### ゲート条件「段階13」の満たし方
- [ ] 全チェック項目が「合格」状態か
- [ ] ビルド/静的チェック成功か
- [ ] ダミー実装が妥当に実施されているか

### テスト質務確認
- [ ] 正常系 + 境界値 + 異常系 がカバーされているか
- [ ] 回帰テスト実行 PASSED か
- [ ] 合格率 100% か

### ログ記録
- [ ] 段階10-13 全実施内容が AI改善/ ログに記録されているか
- [ ] ゲート条件（段階11, 13）の判定結果が明示されているか
- [ ] テスト結果の詳細（各TRK のPASS/FAIL）が記録されているか

---

**次**: Phase 4（報告）のサブスキル へ
