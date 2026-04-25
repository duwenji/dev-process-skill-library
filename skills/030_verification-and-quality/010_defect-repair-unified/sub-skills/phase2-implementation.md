# Phase 2 Sub-Skill: 実装決定・実施（段階7-9）

省略用語（RACI, KPI, ADR, DDL, SLO, QA, PM, TRK, EX）は [../../../shared-references/glossary.md](../../../shared-references/glossary.md) の『略語・日本語対応表』を参照してください。


## 概要
Phase 2 では、開発者が対応案を決定し、AIがその案に基づいてソースコードを実装、その後開発者がコード品質をレビューします。

## 責務
- **開発者**: 対応案決定（段階7）、コードレビュー・承認（段階9）
- **AI**: 実装実施（段階8）、変更内容報告

## ゲート条件 #1: 段階7 進行可否判定

**判定項目**:
- [ ] Phase 1 の成果（調査報告、フロー図、複数案）が十分な品質か
- [ ] 対応案が客観的に比較可能か（メリット/デメリット明示）
- [ ] 開発者が決定できる情報が揃っているか
- [ ] 情報不足や矛盾がないか

**判定結果**:
- **OK**: 段階8 へ進行可能
- **NG**: 段階6 へ返却（対応案の見直し、追加検討）

---

## 段階ごとの入出力

### 段階7: 開発者が対応案をレビュー・決定

**開発者の作業**:
1. Phase 1 の成果を確認（フロー図、複数案のメリット/デメリット）
2. 技術的・運用的に最適な案を選定
3. 決定根拠をログに記録

**記录フォーマット例**:
```markdown
## 段階7: 開発者が対応案をレビュー・決定（2026-03-27 15:00:00 +09:00）

### レビュー内容

#### 対案1: DataRecordExtensions置換
- メリット: 最小規模、リスク低、既存パターン
- デメリット: NULL時の業務規則確認必要
- 評価: ✓ 妥当

#### 対案2: 層厚いNULL判定
- メリット: 保守性高、再発防止
- デメリット: 変更規模大、既存テスト修正多い
- 評価: △ 検討するが、案1の方が即応性優先

#### 対案3: DB側での既定値設定
- メリット: DB整合性確保
- デメリット: マイグレーション複雑、運用承認必要
- 評価: ✗ 本件は即応性を優先し、案1採用

### 選定結果

**採用案**: 対応案 1（DataRecordExtensions置換）

**決定根拠**:
1. 【技術的】最小限の変更で問題解決。既存パターン（null-safety-pattern）と一致
2. 【運用的】本番導入リスクが最小。今週中に検証完了可能
3. 【保守的】NULL判定の標準化により、類似問題の再発防止が期待できる

### 実装条件の確認

#### NULL値の業務規則確認
**確認済**: DISPLAY_NAME は既定値「(未設定)」で問題ない（既存業務ルール確認済）

#### テスト環境の確認
**確認済**: ProcDataSyncTest が存在、自動テスト可能

#### 本番環境への展開計画
**確認済**: 現在の maintenance window を利用、影響範囲矮小

### 承認ステータス
✓ **承認済** — 段階8 へ進行可能

---
```

**ゲート条件チェック**:
- [ ] 決定が客観的根拠に基づいているか
- [ ] 実装条件（テスト環境、本番展開計画）が確認されているか
- [ ] 承認ステータスが「承認済」に更新されているか

**AI への確認**:
- 開発者決定内容を確認
- 段階8（実装）の対象ファイル・メソッド・変更範囲を再確認
- 変更前コードを読込準備

---

### 段階8: AI が改修を実施

**AI の実装フロー**:

#### 1. 実装前準備
- 段階7 の選定案を確認
- 対象ファイル（csproj），変更メソッド，変更シーケンスを整理

#### 2. コード変更実施

**Before（問題あり）**:
```csharp
public void ProcessDataRecord(DataRecord dataRecord)
{
    try
    {
        string displayName = (string)dataRecord["DISPLAY_NAME"];  // ❌ DBNull時Exception
        string recordId = (string)dataRecord["RECORD_ID"];
        
        var dataEntry = new DataEntry
        {
            DisplayName = displayName,
            RecordId = recordId,
            CategoryCode = (int)dataRecord["CATEGORY_CODE"],
        };
        
        ProcessItem(dataEntry);
    }
    catch (Exception ex)
    {
        _logger.Error($"Failed to process: {ex.Message}");
        throw;
    }
}
```

**After（対応後）**:
```csharp
public void ProcessDataRecord(DataRecord dataRecord)
{
    try
    {
        // DataRecordExtensions を使用して NULL判定を強化
        string displayName = dataRecord.GetStringOrNull("DISPLAY_NAME") ?? "(未設定)";
        string recordId = dataRecord.GetStringOrNull("RECORD_ID") ?? "";
        
        var dataEntry = new DataEntry
        {
            DisplayName = displayName,
            RecordId = recordId,
            CategoryCode = dataRecord.GetInt32OrNull("CATEGORY_CODE") ?? 0,
        };
        
        ProcessItem(dataEntry);
    }
    catch (Exception ex)
    {
        _logger.Error($"Failed to process dataEntry: {ex.Message}");
        throw;
    }
}
```

#### 3. 関連ファイル確認
- LibDataCommon.cs で同パターンが他にもないか確認
  - 複数箇所あれば全て置換

#### 4. ビルド確認
```powershell
cd DataMgmtSv
devenv DataMgmtSv.sln /Build "Release|x64" /Project "ProcDataSync\ProcDataSync.csproj"
# または
dotnet build ProcDataSync.csproj
```

**実装完了後の出力**:
```markdown
## 段階8: AI が改修を実施（2026-03-27 15:30:00 +09:00）

### 変更ファイル一覧

#### ファイル 1: LibDataCommon.cs (Proc/LibCommon配下)

**変更メソッド**: ProcessDataRecord()

**変更概要**: 直接キャスト（`(string)dataRecord["DISPLAY_NAME"]`）を DataRecordExtensions 
で置換。NULL時に既定値を適用。

**変更行数**: +5行（削除0, 追加5）

**Before**:
\`\`\`csharp
string displayName = (string)dataRecord["DISPLAY_NAME"];
string recordId = (string)dataRecord["RECORD_ID"];
int categoryCode = (int)dataRecord["CATEGORY_CODE"];
\`\`\`

**After**:
\`\`\`csharp
string displayName = dataRecord.GetStringOrNull("DISPLAY_NAME") ?? "(未設定)";
string recordId = dataRecord.GetStringOrNull("RECORD_ID") ?? "";
int categoryCode = dataRecord.GetInt32OrNull("CATEGORY_CODE") ?? 0;
\`\`\`

**変更理由**: 対応案1（DataRecordExtensions置換）に基づき、NULL判定を強化

**関連変更**: 同一ファイル内に類似パターンがないか確認 → 本メソッドのみ

---

#### ファイル 2: ProcDataSyncTest.cs

**追加テストケース**: ProcessDataRecord_NullDisplayName()

**テストコード**:
\`\`\`csharp
[Test]
public void ProcessDataRecord_NullDisplayName()
{
    // Arrange
    var mockRecord = new MockDataRecord();
    mockRecord.SetStringNull("DISPLAY_NAME");
    mockRecord.SetString("RECORD_ID", "ID001");
    mockRecord.SetInt("CATEGORY_CODE", 100);
    
    var processor = new DataProcessor(_logger);
    
    // Act
    processor.ProcessDataRecord(mockRecord);
    
    // Assert
    // 既定値「(未設定)」で処理されることを確認
    Assert.That(processor.LastProcessedEntry.DisplayName, Is.EqualTo("(未設定)"));
}
\`\`\`

**追加行数**: +15行

**変更理由**: NULL値境界条件をテストカバー

---

### ビルド確認結果

**対象プロジェクト**: ProcDataSync.csproj, ProcDataSyncTest.csproj

**ビルド結果**: ✓ 成功

**エラー/警告**: なし

**コメント**: 依存プロジェクト (LibComMng, LibComUnit) も含めて確認済み

---

### 実装根拠

| 変更項目 | 対応案との対応 | 根拠 |
|---------|--------------|------|
| DataRecordExtensions置換 | 対案1指定 | NULL判定により InvalidCastException を防止 |
| 既定値「(未設定)」適用 | 対案1テストメソッド | 既存業務ルール（段階7確認済） |
| テストケース追加 | 対案1テスト対象 | 境界値テストでカバーする |

**次: 段階9（コードレビュー）へ進む**
```

---

### 段階9: 開発者がソースコード（差分）をレビュー・承認

**開発者の作業**:
1. 段階8 の変更ファイル一覧を詳細レビュー
2. コード品質・正確性を評価
3. 既存テストとの整合性を確認
4. ビルド成功を確認

**レビューチェックリスト**:
```
## 段階9: 開発者がソースコード（差分）をレビュー・承認（2026-03-27 16:00:00 +09:00）

### レビュー項目

#### 1. 変更スコープの確認
- [ ] 対応案と一致しているか（データRecordExtensions置換のみ）
- [ ] 意図しない変更（他メソッド、他ファイル）がないか
- [ ] 関連ライブラリ（DataRecordExtensions）が存在するか

#### 2. エラーハンドリング
- [ ] NULL判定が正確か（GetStringOrNull() + ?? で既定値）
- [ ] 既定値が業務規則と一致しているか（「(未設定)」の確認）
- [ ] try-catch の範囲は適切か

#### 3. ログ出力・デバッグ情報
- [ ] 既存ログ出力が保持されているか
- [ ] 新しく追加すべきログがないか（NULL判定時の情報出力）

#### 4. コードスタイル・命名規則
- [ ] 既存コードスタイルに準拠しているか
- [ ] 変数名・メソッド名が明確か

#### 5. テストカバー
- [ ] 新規テストケース（ProcessDataRecord_NullDisplayName）が適切か
- [ ] 既存テストの回帰リスクはないか（既存テスト名で実行）

#### 6. ビルド・静的チェック
- [ ] ビルドエラー / WARNING がないか
- [ ] StyleCop / Code Analyzers を通すか

### レビュー結果

#### 変更スコープ確認
✓ OK — 対応案1と完全一致、意図しない変更なし

#### エラーハンドリング
✓ OK — NULL判定&既定値の適用が正確、業務ルール確認済

#### ログ出力
✓ OK — 既存ログ保持、新規ログ不要（デバッグ情報は既存で十分）

#### コードスタイル
✓ OK — 既存規則準拠、明確で可読性高い

#### テストカバー
✓ OK — 境界値テスト（NULL）が追加され、既存テストも並行確認

#### ビルド確認
✓ OK — エラー / WARNING なし

### 指摘事項
[あれば記載。ない場合は「特になし」]

### 最終判定

**承認ステータス**: ✓ **承認済** — Phase 3 へ進行可能

**確認日時**: 2026-03-27 16:00:00
**確認者**: [開発者名]

---
```

**ゲート条件チェック** (Phase 3 進行可否):
- [ ] 変更が対応案と合致しているか ✓
- [ ] 技術的に問題がないか ✓
- [ ] テスト実行可能な状態か ✓
- [ ] ビルド成功か ✓

**完了判定**:
- [ ] 承認ステータスが「承認済」に更新されているか
- [ ] 次 Phase（テスト・チェック, 段階10）へ進む準備は整っているか

---

## Phase 2 完了チェックリスト

### 各段階の完了確認
- [ ] 段階7: 開発者が対応案を決定、根拠を記録
- [ ] 段階8: AI が改修を実装、変更ファイル・差分を明示
- [ ] 段階9: 開発者がコードレビュー・承認

### ゲート条件「段階7」の満たし方
- [ ] Phase 1 成果（調査報告、フロー図、複数案）が明示されているか
- [ ] 開発者の決定根拠が記録されているか

### コード品質確認
- [ ] 変更が最小限で対応案と一致しているか
- [ ] エラーハンドリング（NULL判定）が正確か
- [ ] 既存テストの回帰リスクはないか
- [ ] ビルド成功、エラー/警告なし

### ログ記録
- [ ] 段階7-9 の実施内容すべてが docs/skill-logs/ ログファイルに記録されているか
- [ ] ゲート条件の判定結果が明示されているか

---

**次**: Phase 3（テスト・チェック）のサブスキル へ
