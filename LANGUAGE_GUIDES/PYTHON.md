# Python 開発ガイド

Chronona 組織の Python プロジェクトにおける開発手法と環境構築の標準です。

## 環境構築

### パッケージマネージャー: uv

**すべての Python プロジェクトは `uv` を使用します。**

- `uv` は高速で信頼性の高い Python パッケージマネージャー
- `pip` や `poetry` の代替として機能
- ロック機構により環境の再現性を保証

#### インストール

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

#### 基本コマンド

```bash
# プロジェクト初期化
uv init

# 依存関係をインストール
uv sync

# パッケージを追加
uv add requests

# 開発用パッケージを追加
uv add --dev pytest

# ロックファイルを更新
uv lock --upgrade
```

### ファイル構成

```text
project/
├── pyproject.toml       # プロジェクト設定・依存関係定義
├── uv.lock              # ロックファイル（Git に含める）
├── src/
│   └── myproject/
│       ├── __init__.py
│       └── main.py
└── tests/
    └── test_main.py
```

**重要:** `uv.lock` は Git にコミットして、チーム全体で同じ環境を使う。

## テスト

### テストフレームワーク: pytest

```bash
# テスト実行
uv run pytest

# 特定のテストファイルを実行
uv run pytest tests/test_auth.py

# 詳細表示
uv run pytest -v

# カバレッジ確認（必須ではない）
uv run pytest --cov=src
```

### テスト作成の指針

- TDD を基本にする（AGENTS.md 参照）
- テストファイルは `tests/` 配下に置く
- ファイル名は `test_*.py` または `*_test.py`
- 各テストは独立して実行可能にする

## 型チェック

### pyright / mypy の使用

```bash
# pyright でチェック
uv run pyright

# または mypy
uv run mypy src/
```

**設定ファイル：** `pyproject.toml` に記載

```toml
[tool.pyright]
typeCheckingMode = "standard"

[tool.mypy]
strict = true
```

## コード品質

### Linting / Formatting

```bash
# ruff でリント・フォーマット
uv run ruff check src/
uv run ruff format src/

# 修正を自動適用
uv run ruff check --fix src/
uv run ruff format src/
```

**設定ファイル：** `pyproject.toml`

```toml
[tool.ruff]
line-length = 100
target-version = "py312"
```

## 実行と開発ワークフロー

### スクリプト実行

```bash
# main.py を実行
uv run python src/myproject/main.py

# または pyproject.toml でスクリプトを定義
uv run mycommand
```

**pyproject.toml の例：**

```toml
[project.scripts]
mycommand = "myproject.main:main"
```

### 開発モード

```bash
# 編集可能インストール（開発中のコード変更が即反映）
uv sync

# この後、コードを編集すればすぐに反映される
```

## チェックリスト（実装時）

Python プロジェクトで作業を始める前に：

- [ ] `uv sync` で環境構築済みか
- [ ] テストが実行できるか（`uv run pytest`）
- [ ] linter が実行できるか（`uv run ruff check`）
- [ ] 型チェックが実行できるか（`uv run pyright`）
- [ ] TODO リスト（テストケース）を作成したか
- [ ] TDD で進めるか（テスト → 実装）
- [ ] PR マージ前に全テストが通っているか

## 参考

- [uv 公式ドキュメント](https://docs.astral.sh/uv/)
- [pytest 公式ドキュメント](https://docs.pytest.org/)
- [Ruff ドキュメント](https://docs.astral.sh/ruff/)
- [Pyright ドキュメント](https://github.com/microsoft/pyright)
