# TemplatePython

Pythonの開発環境用のテンプレートリポジトリ

## 変更履歴

- 2025-12-25: プロジェクト作成
- 2026-03-16: Devcontainerによる環境開発設定を追記

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

> **注意**:\
> 初回のコミット時は、pre-commitの各ツール（ruff、black、mypy）の環境が初期化\
> 初期化には、数十秒程度の時間がかかる
>
> 初回コミット時のログ例:
>
> ```
> [INFO] Initializing environment for https://github.com/charliermarsh/ruff-pre-commit.
> [INFO] Initializing environment for https://github.com/psf/black.
> [INFO] Initializing environment for https://github.com/pre-commit/mirrors-mypy.
> [INFO] Installing environment for https://github.com/charliermarsh/ruff-pre-commit.
> [INFO] Once installed this environment will be reused.
> [INFO] This may take a few minutes...
> ```

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

# Devcontainerによる開発環境構築

このプロジェクトには、[Dev Containers](https://containers.dev/)を利用した開発環境の設定が含まれています。Devcontainerを使うことで、ローカル環境に依存しない一貫した開発環境を簡単に構築可能

## フォルダ構成

```
.devcontainer/
├── devcontainer_sample.json  # サンプル設定ファイル
```

## 利用方法

1. `.devcontainer/devcontainer_sample.json` を `.devcontainer/devcontainer.json` にコピー

```bash
cp .devcontainer/devcontainer_sample.json .devcontainer/devcontainer.json
```

2. 必要に応じて `devcontainer.json` を編集し、プロジェクトに合わせて設定を調整。[設定のドキュメント](https://containers.dev/implementors/json_reference/)

3. [VS Code Dev Containers拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)などを利用して、`Reopen in Container` を実行すると、定義された環境で開発可能。

> **補足**: サンプルファイルを直接編集せず、必ずコピーしてから利用sすること。
