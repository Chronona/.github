# Python 開発ガイド

Chronona の Python プロジェクトにおける開発標準です。

## 環境構築

### uv（パッケージマネージャー）

すべての Python プロジェクトは `uv` を使用します。

```bash
# インストール
curl -LsSf https://astral.sh/uv/install.sh | sh   # macOS/Linux
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"  # Windows

# プロジェクト初期化
uv init

# 依存関係をインストール
uv sync

# パッケージを追加
uv add requests
uv add --dev pytest
```

### ファイル構成

```text
project/
├── pyproject.toml       # 設定・依存関係
├── uv.lock              # ロックファイル（Git に含める）
├── src/myproject/
│   ├── __init__.py
│   └── main.py
└── tests/
    └── test_main.py
```

## テスト

### pytest

```bash
uv run pytest                    # すべて実行
uv run pytest tests/test_auth.py # 特定ファイル
uv run pytest -v                 # 詳細表示
```

TDD の原則に従う（AGENTS.md 参照）。テストファイルは `test_*.py` 形式で `tests/` 配下に置く。

## 型チェック・コード品質

```bash
uv run pyright          # 型チェック
uv run ruff check src/  # リント
uv run ruff format src/ # フォーマット
```

設定は `pyproject.toml` に記載する。

```toml
[tool.pyright]
typeCheckingMode = "standard"

[tool.ruff]
line-length = 100
target-version = "py312"
```

## 実装時のチェックリスト

- [ ] `uv sync` で環境構築完了
- [ ] `uv run pytest` で全テスト実行可能
- [ ] `uv run ruff check` と `uv run pyright` を実行
- [ ] AGENTS.md の TDD ルールに従う
