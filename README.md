# sample_GithubCopilot

GitHub Copilotの効果的な使い方を学ぶためのサンプルリポジトリです。
人間は、「README.md」に加えて「SETUP.md」を見てください。

---

## 概要

このリポジトリは、GitHub Copilotを最大限に活用するための設定ファイルとサンプルコードをまとめます。

### 目的

- GitHub Copilotの設定方法を理解する
- プロジェクト固有のコーディング規約をCopilotに学習させる
- より精度の高いコード提案を受けるための環境を構築する

---

## はじめに

### 前提条件

- GitHub Copilotのサブスクリプション（個人版またはビジネス版）
- サポートされているエディタ（VS Code, JetBrains IDEs, Neovim など）
- Git

### セットアップ

1. リポジトリをクローン

```bash
git clone https://github.com/[your-username]/sample_GithubCopilot.git
cd sample_GithubCopilot
```

2. エディタでプロジェクトを開く

```bash
code .
```

3. GitHub Copilot 拡張機能を有効化 

---

## プロジェクト構成

```
sample_GithubCopilot/
├── .github/
│   ├── copilot-instructions.md    # Copilot向けプロジェクト規約
│   └── PROMPTS.md                  # カスタムプロンプトテンプレート
├── .copilotignore                  # Copilotから除外するファイル
├── .editorconfig                   # エディタ設定
├── .vscode/
│   └── settings.json               # VS Code設定
├── examples/                       # サンプルコード
├── docs/                          # ドキュメント
└── README.md                      # このファイル
```

---

## 設定ファイルの説明

### 1. `.github/copilot-instructions.md`

**目的**: プロジェクト全体のコーディング規約とベストプラクティスをCopilotに伝える

**含まれる情報**:
- 命名規則
- 優先ライブラリ
- エラーハンドリング方針
- コメント記述ルール
- セキュリティガイドライン

### 2. `.github/PROMPTS.md`

**目的**: よく使うプロンプトをテンプレート化して再利用性を高める

**含まれるテンプレート**:
- 関数生成
- リファクタリング
- テストコード生成
- コードレビュー
- パフォーマンス最適化

### 3. `.copilotignore`

**目的**: 機密情報や不要なファイルをCopilotのコンテキストから除外する

**除外する対象**:
- 環境変数ファイル（`.env`）
- APIキー、認証情報
- 大容量ファイル
- ビルド成果物

### 4. `.editorconfig`

**目的**: エディタに依存しないコード整形ルールを定義する

**設定内容**:
- インデント方式（スペース/タブ）
- インデント幅
- 改行コード
- 文字エンコーディング

### 5. `.vscode/settings.json`

**目的**: VS Code固有の設定とCopilot設定を統一する

**設定内容**:
- Copilot有効化
- インラインサジェスト設定
- 自動保存設定

---

## 💡 GitHub Copilotの使い方

### 基本的な使い方

#### 1. インライン補完

コードを書き始めると、Copilotが自動的に続きを提案します。

```python
# "ユーザーデータを取得" とコメントを書くと...
# Copilotが関数を提案
async def fetch_user_data(user_id: str) -> dict:
    """ユーザーIDからユーザー情報を取得する"""
    response = requests.get(f'/api/users/{user_id}')
    response.raise_for_status()
    return response.json()
```

**ショートカット**:
- `Tab`: 提案を受け入れる
- `Esc`: 提案を却下する
- `Alt + ]`: 次の提案を表示
- `Alt + [`: 前の提案を表示

#### 2. Copilot Chat

**開き方**:
- VS Code: `Ctrl+Shift+I` (Windows/Linux) / `Cmd+Shift+I` (Mac)

**使用例**:

```
# コードの説明を求める
"このコードを説明してください"

# リファクタリング
"このコードをリファクタリングしてパフォーマンスを改善してください"

# テスト生成
"この関数のユニットテストを作成してください"

# バグ修正
"このコードのバグを見つけて修正してください"
```

#### 3. スラッシュコマンド

Copilot Chatで使える便利なコマンド:

- `/explain`: コードの説明
- `/fix`: バグ修正の提案
- `/tests`: テストコード生成
- `/help`: ヘルプの表示

---

## 🎯 効果的な使い方のコツ

### 1. 詳細なコメントを書く

```python
# ❌ 悪い例
# データ取得

# ✅ 良い例
# ユーザーIDからユーザー情報を取得し、エラー時は404を返す
```

### 2. 関数名を明確にする

```python
# ❌ 悪い例
def process(data):
    pass

# ✅ 良い例
def validate_and_save_user_data(user_data: dict) -> bool:
    pass
```

### 3. 例を示す

```python
# ユーザーオブジェクトを検証する
# 例: {"name": "田中太郎", "email": "tanaka@example.com", "age": 30}
def validate_user(user: dict) -> bool:
    # Copilotがバリデーションロジックを提案
    pass
```

### 4. プロジェクト固有の設定を活用

- `.github/copilot-instructions.md` にプロジェクト規約を記述
- `.copilotignore` で不要なファイルを除外
- `PROMPTS.md` でよく使うプロンプトをテンプレート化

---

## 📚 サンプルコード

`examples/` ディレクトリに、以下のサンプルコードを用意しています:

- `rest-api.py`: REST API
---

## 🔒 セキュリティ

### 機密情報の取り扱い

1. **環境変数を使用**

```python
# ❌ ハードコードしない
API_KEY = "sk-1234567890abcdef"

# ✅ 環境変数から読み込む
import os
API_KEY = os.environ.get('API_KEY')
```

2. **`.copilotignore` に追加**

```
.env
**/credentials.json
**/secrets.json
```

3. **コードレビュー**

機密情報が含まれていないか、必ずレビューを実施してください。

---

## 🤝 コントリビューション

プルリクエストやイシューの報告を歓迎します！

### 貢献方法

1. このリポジトリをフォーク
2. フィーチャーブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'Add some amazing feature'`)
4. ブランチをプッシュ (`git push origin feature/amazing-feature`)
5. プルリクエストを作成

---

## 📝 ライセンス

このプロジェクトはMITライセンスの下で公開されています。

---

## 🔗 参考リンク

- [GitHub Copilot 公式ドキュメント](https://docs.github.com/ja/copilot)
- [GitHub Copilot ベストプラクティス](https://github.blog/2023-06-20-how-to-write-better-prompts-for-github-copilot/)
- [EditorConfig 公式サイト](https://editorconfig.org/)

---

## 📧 お問い合わせ

質問や提案がある場合は、Issuesセクションで報告してください。

---

**Happy Coding with GitHub Copilot! 🚀**
