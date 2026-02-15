# GitHub Copilot Instructions

このファイルは、GitHub Copilotがプロジェクト固有のコーディング規約やベストプラクティスを理解するための設定ファイルです。

---

## プロジェクト概要

このリポジトリは、GitHub Copilotの効果的な使い方を学ぶためのPythonサンプル集です。

---

## コーディング規約

### 命名規則

- **変数名**: snake_case を使用（例: `user_name`, `item_count`）
- **定数名**: UPPER_SNAKE_CASE を使用（例: `MAX_RETRY_COUNT`, `API_ENDPOINT`）
- **関数名**: snake_case を使用し、動詞で始める（例: `get_user_data()`, `calculate_total()`）
- **クラス名**: PascalCase を使用（例: `UserManager`, `DataProcessor`）
- **プライベート変数/関数**: 先頭にアンダースコアを付ける（例: `_internal_method()`）
- **プライベートクラス変数**: 先頭に二重アンダースコアを付ける（例: `__private_var`）

### ファイル名とディレクトリ構成

- **ファイル名**: snake_case を使用（例: `user_service.py`, `data_processor.py`）
- **テストファイル**: `test_` プレフィックスを付ける（例: `test_user_service.py`）

### 型ヒント

- すべての関数に型ヒントを付ける
- 戻り値の型も必ず指定する
- Python 3.10以降の新しい型構文を使用する

```python
# Good example
def get_user_data(user_id: int) -> dict[str, any]:
    """ユーザーデータを取得する"""
    pass

# Bad example
def get_user_data(user_id):
    pass
```

### Docstring

- すべてのpublic関数とクラスにdocstringを記述
- Google Style Docstring形式を使用

```python
def fetch_user_data(user_id: int) -> dict[str, any]:
    """ユーザーIDからユーザー情報を取得する

    Args:
        user_id: ユーザーの一意識別子

    Returns:
        ユーザー情報を含む辞書

    Raises:
        ValueError: user_idが不正な場合
        HTTPError: API通信エラーの場合
    """
    pass
```

### コメント

- 日本語でコメントを記述
- 複雑なロジックには必ず説明コメントを追加
- TODOコメントには担当者名と日付を記載

```python
# Good example
# ユーザーデータをキャッシュから取得し、存在しない場合はAPIから取得する
def get_user_data(user_id: int) -> dict[str, any]:
    # TODO: キャッシュ機能を実装 (Tanaka, 2025-02-15)
    pass
```

---

## 優先ライブラリ

### Python標準ライブラリ
- **日付処理**: `datetime`, `zoneinfo`
- **非同期処理**: `asyncio`
- **環境変数**: `os.environ` または `python-dotenv`

### サードパーティライブラリ
- **HTTP通信**: `requests`（同期）または `httpx`（非同期）
- **データ処理**: `pandas` + `numpy`
- **テスト**: `pytest` + `pytest-cov`
- **型チェック**: `mypy`
- **コード整形**: `black` + `isort` + `ruff`
- **バリデーション**: `pydantic`

---

## エラーハンドリング

### 原則

1. すべての外部API呼び出しにはエラーハンドリングを実装
2. エラーメッセージは日本語で記述
3. ログレベルを適切に設定（DEBUG, INFO, WARNING, ERROR）

```python
import logging

logger = logging.getLogger(__name__)

# Good example
def fetch_data(url: str) -> dict[str, any]:
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.Timeout:
        logger.error(f"タイムアウト: {url}")
        raise
    except requests.exceptions.HTTPError as e:
        logger.error(f"HTTP エラー: {e.response.status_code}")
        raise
    except Exception as e:
        logger.error(f"予期しないエラー: {str(e)}")
        raise
```

### カスタム例外

プロジェクト固有の例外クラスを定義する

```python
class UserNotFoundError(Exception):
    """ユーザーが見つからない場合の例外"""
    pass

class ValidationError(Exception):
    """バリデーションエラーの例外"""
    pass
```

---

## セキュリティ

- APIキーや秘密情報は環境変数から読み込む
- `.env` ファイルは `.gitignore` に追加
- 機密情報を含むファイルは `.copilotignore` に追加

```python
# Good example
import os
from dotenv import load_dotenv

load_dotenv()
API_KEY = os.environ.get('API_KEY')

# Bad example
API_KEY = "sk-1234567890abcdef"  # ハードコードしない
```

---

## テストカバレッジ

- 新規関数には必ずユニットテストを作成
- カバレッジ目標: 80%以上
- 重要なビジネスロジックは100%カバレッジを目指す

```python
# test_user_service.py
import pytest
from user_service import get_user_data

def test_get_user_data_success():
    """正常系: ユーザーデータが正しく取得できる"""
    user_data = get_user_data(user_id=1)
    assert user_data is not None
    assert user_data['id'] == 1

def test_get_user_data_not_found():
    """異常系: 存在しないユーザーIDの場合"""
    with pytest.raises(UserNotFoundError):
        get_user_data(user_id=99999)
```

---

## ドキュメント

- パブリックAPIにはdocstringを必ず記載
- READMEは日本語で記述
- 複雑な機能には使用例を追加

---

## Git コミットメッセージ

Conventional Commits形式を使用:

```
<type>: <subject>

<body>
```

**Types:**
- `feat`: 新機能
- `fix`: バグ修正
- `docs`: ドキュメント
- `style`: フォーマット
- `refactor`: リファクタリング
- `test`: テスト
- `chore`: その他

**Example:**
```
feat: ユーザー認証機能を追加

- JWTトークンベースの認証を実装
- ログイン/ログアウトエンドポイントを追加
```

---

## パフォーマンス

- 大量データ処理にはジェネレータを使用
- 不要なループのネストを避ける
- リスト内包表記を積極的に使用

```python
# Good example
user_ids = [user['id'] for user in users if user['active']]

# Bad example
user_ids = []
for user in users:
    if user['active']:
        user_ids.append(user['id'])
```

---

## 非同期処理

- I/O処理は可能な限り非同期化する
- `asyncio` を使用する場合は型ヒントも対応させる

```python
import asyncio
import httpx

async def fetch_user_data_async(user_id: int) -> dict[str, any]:
    """非同期でユーザーデータを取得する"""
    async with httpx.AsyncClient() as client:
        response = await client.get(f'/api/users/{user_id}')
        response.raise_for_status()
        return response.json()
```

---

## コード品質

### 使用するツール

- **フォーマッター**: `black` (line-length=100)
- **インポート整理**: `isort`
- **リンター**: `ruff`
- **型チェック**: `mypy`

### 実行コマンド

```bash
# フォーマット
black .
isort .

# リント
ruff check .

# 型チェック
mypy .

# テスト
pytest --cov=. --cov-report=html
```
