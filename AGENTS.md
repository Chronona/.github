# エージェント向け開発ガイド

このドキュメントは、Chronona 組織のリポジトリで作業するエージェント（AI アシスタント等）が従うべき開発手法と基準を示します。

## 核となる原則

### 1. テスト駆動開発（TDD）

TDD は **Red → Green → Refactor** の厳密なループです。詳細な実装ガイドは以下を参照してください：

👉 **[mattpocock/skills - TDD スキル](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md)**

**Chronona での TDD 実践ポイント：**
- テスト駆動で実装し、公開インターフェース（seams）を通じて振る舞いを検証する
- 実装詳細ではなく、ユーザーが観測できる動作をテストする
- 重要なロジックやバグ修正では、必ず再現テストから始める
- カバレッジ%は目標にしない。動作確認できるテストを優先する

**Tidy First の原則**
- First: 今回の変更に必要な最小限の整理を先に行う
- After: 振る舞い変更が通った後に、近傍の簡単な整理を行う
- Later: 大きな構造変更や複数領域にまたがる整理は後で行う
- Never: 変更とは無関係な書式修正や大規模リファクタを行わない

### 2. Issue と PR の連携

**Issue → PR → Merge** の流れを保つ

#### Issue 作成時
- 既存 Issue の重複確認
- テンプレート（`bug_report.yml` / `feature_request.yml` / `task.yml`）を使用
- 完了条件チェックリストを明確に記載

#### PR 作成時
- **1 Issue = 1 PR**（対応関係を明確に）
- PR タイトル・説明は「何を」「なぜ」を簡潔に
- `Closes #123` を PR 本文に記載（Issue 自動クローズ）
- PR マージ前に全テストが通ること

### 3. コミットメッセージ規約

```text
<type>(<scope>): <subject>

<body>

Closes #<issue-number>
```

**type（必須）：**
- `feat:` 新機能
- `fix:` バグ修正
- `test:` テスト追加・修正
- `refactor:` リファクタリング
- `docs:` ドキュメント更新
- `ci:` CI/CD 関連

**例：**
```text
test(auth): add login validation tests

Add test cases for email format and empty password scenarios
Closes #42
```

### 4. コード規約の基本

- **可読性第一*：変数名・関数名は機能を明確に
- **DRY 原則**：重複コードは関数・モジュール化
- **コメント**：「何をするか」より「なぜそうするか」を記載
- **型安全**：言語の型システムを活用

### 5. リポジトリ固有ガイドがあれば優先

各リポジトリに `DEVELOPMENT.md` や `CONTRIBUTING.md` がある場合、それが最優先です。

---

## チェックリスト（実装時）

エージェントが作業を始める前に、以下を確認：

- [ ] 対応する Issue が存在するか（ない場合は先に Issue 作成）
- [ ] リポジトリに `DEVELOPMENT.md` がないか確認
- [ ] テスト環境が動作するか（`npm test` / `python -m pytest` など）
- [ ] テストコード → 実装コード の順に進める
- [ ] すべてのテストが通ったことを確認

---

## リポジトリ構成の理解

このリポジトリ（`.github`）は **組織全体のデフォルトファイル** を配置しています。
変更すると全リポジトリに影響するため、変更前に複数リポジトリへの影響を検討してください。
