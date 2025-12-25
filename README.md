# TemplatePython
Pythonの開発環境用のテンプレートリポジトリ

# Python環境構築

このプロジェクトは[uv](https://docs.astral.sh/uv/)を使用した高速なPython環境管理を前提としています。

## uvのインストール

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

## 仮想環境の構築

プロジェクトには`pyproject.toml`が既に設定されています。以下のコマンドで依存関係をインストールしてください。

### 仮想環境の作成と依存関係のインストール
```bash
uv sync
```

### 仮想環境の有効化
```bash
source .venv/bin/activate  # macOS/Linux
.venv\Scripts\activate     # Windows
```

### pre-commitのセットアップ
```bash
uv run pre-commit install
```

## インストールされているライブラリ

### 全体ライブラリ（dependencies）
- **pydantic**: データバリデーションライブラリ
- **pydantic-settings**: 設定管理ライブラリ
- **python-dotenv**: 環境変数管理ライブラリ

### 開発用ライブラリ（dev）
- **ruff**: 高速なPythonリンター・フォーマッター
- **black**: コードフォーマッター
- **mypy**: 静的型チェッカー
- **pre-commit**: Git コミット前の自動チェックフレームワーク
- **ipykernel**: Jupyter Notebookカーネル

# Pythonフォーマッター

## Black設定
```toml
[tool.black]
line-length = 88           # 1行の最大文字数を88文字に設定
target-version = ["py312"] # Python 3.12をターゲットバージョンとして設定
```

## Ruff設定
```toml
[tool.ruff]
exclude = ["node_modules", ".venv", "venv", "__pypackages__"]  # チェック対象から除外するディレクトリ
line-length = 88           # 1行の最大文字数を88文字に設定
target-version = "py312"   # Python 3.12をターゲットバージョンとして設定

[tool.ruff.lint]
select = ["E", "W", "F", "I", "N", "UP", "B", "C4", "SIM"]  # 有効化するルール
  # E: pycodestyle エラー
  # W: pycodestyle 警告
  # F: Pyflakes
  # I: isort (import文の並び替え)
  # N: pep8-naming (命名規則)
  # UP: pyupgrade (Python構文の更新)
  # B: flake8-bugbear (バグを引き起こしやすいコードの検出)
  # C4: flake8-comprehensions (内包表記の改善)
  # SIM: flake8-simplify (コードの簡略化)
ignore = ["E501", "UP007"]  # 無視するルール
  # E501: 行の長さ超過 (line-lengthで管理)
  # UP007: Union型の記法 (X | Y の強制を無効化)
fixable = ["I", "B", "F401"]  # 自動修正可能なルール
  # I: import文の並び替え
  # B: flake8-bugbear の一部
  # F401: 未使用のインポート

[tool.ruff.format]
quote-style = "double"              # ダブルクォートを使用
indent-style = "space"              # インデントにスペースを使用
skip-magic-trailing-comma = false  # マジックトレーリングカンマを有効化
line-ending = "auto"                # 改行コードを自動検出
```

## Mypy設定
```toml
[tool.mypy]
python_version = "3.12"           # Python 3.12をターゲットバージョンとして設定
explicit_package_bases = true     # パッケージベースを明示的に設定
ignore_missing_imports = true     # インポート先の型情報がない場合エラーを無視
disallow_untyped_defs = true      # 型アノテーションのない関数定義を禁止
warn_return_any = true            # Any型の返り値に警告
strict_optional = true            # Optional型のチェックを厳格化
```
