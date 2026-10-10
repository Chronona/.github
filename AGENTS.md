# エージェント向け開発ガイド

Chronona 組織のリポジトリで作業するエージェントは、目的が一致し、第三者評価の高い外部スキルを組み合わせて利用する。詳細ルールは外部スキルに委ね、Chronona 固有の最小ルールだけをここに残す。スキルはインストール済みを前提とする。

## 基本方針

- 変更前に影響範囲と関連リポジトリを確認する
- リポジトリ固有の `DEVELOPMENT.md` / `CONTRIBUTING.md` があればそれを優先する
- TDD を基本とし、実装前に失敗するテストを書く
- 最小限の変更に留め、Tidy First を守る

## 参照スキル

- TDD: https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md
- Issue / 要件整理: `to-tickets`, `to-spec`
- 調査 / 設計: `grill-with-docs`, `domain-modeling`
- コードレビュー: `code-review`
- Git / コミットの安全性: `git-guardrails-claude-code`

## Chronona の最小ルール

### 1. Issue と PR の連携
- 既存 Issue の重複確認を行う
- テンプレート（`bug_report.yml` / `feature_request.yml` / `task.yml`）を使う
- 完了条件を明確に記載する
- PR は 1 Issue = 1 PR を原則とする
- PR 本文に `Closes #123` を含める
- PR マージ前に全テストが通ることを確認する

### 2. コミットメッセージ規約
```text
<type>(<scope>): <subject>

<body>

Closes #<issue-number>
```

`type` は以下を使う:
- `feat:` 新機能
- `fix:` バグ修正
- `test:` テスト追加・修正
- `refactor:` リファクタリング
- `docs:` ドキュメント更新
- `ci:` CI/CD 関連

### 3. コード規約
- 可読性を優先し、変数名・関数名を機能が分かる名前にする
- 重複コードは関数やモジュールにまとめる
- コメントは「何をするか」ではなく「なぜそうするか」を書く
- 型安全に配慮する

## 実装前チェックリスト

- [ ] 対応する Issue または要件があるか
- [ ] リポジトリ固有ガイドを確認したか
- [ ] テスト環境が動作しているか
- [ ] 失敗するテストを先に追加したか
- [ ] テストを通す最小限の実装に留めたか
- [ ] すべてのテストが通ったか

## リポジトリ構成の理解

このリポジトリ（`.github`）は組織全体のデフォルト設定を置く場所であり、変更は全リポジトリへ影響する。変更前に影響範囲を十分に確認する。
