# セットアップガイド

このガイドでは、プロジェクトをクローンしてから実際に使えるようになるまでの手順を説明します。

---

## 📋 必要なもの

- **Python 3.10以降**
- **Git**
- **VS Code**（推奨）
- **GitHubアカウント**（Copilot用）
- **GitHub Copilotサブスクリプション**

---

## 🚀 クイックスタート

### 1. リポジトリをクローン

```bash
git clone https://github.com/[your-username]/sample_GithubCopilot.git
cd sample_GithubCopilot
```

### 2. 仮想環境を作成・有効化

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. 依存パッケージをインストール

```bash
pip install -r requirements.txt
```

または開発用パッケージも含めて:

```bash
pip install -e ".[dev]"
```

### 4. VS Codeで開く

```bash
code .
```

### 5. Pythonインタープリタを選択

1. `Ctrl+Shift+P` (Windows/Linux) または `Cmd+Shift+P` (Mac)
2. "Python: Select Interpreter"を選択
3. `./venv/bin/python` を選択

### 6. GitHub Copilotを有効化

1. VS Codeで拡張機能ビューを開く（`Ctrl+Shift+X`）
2. "GitHub Copilot"をインストール
3. "GitHub Copilot Chat"もインストール
4. GitHubアカウントでサインイン

---

## 📁 プロジェクト構成

クローン後、以下のような構成になります:

```
sample_GithubCopilot/
├── .github/
│   ├── copilot-instructions.md    # Copilot向けプロジェクト規約
│   └── PROMPTS.md                  # カスタムプロンプト集
│
├── .vscode/
│   └── settings.json               # VS Code設定
│
├── examples/                       # サンプルコード
│   └── rest-api.py       　       # リファクタリング後
│
├── tests/                          # テストコード
│   └── test_rest-api.py            # サンプルテスト
│
├── docs/                           # ドキュメント
│   └── getting-started.md         # 導入ガイド
│
├── .copilotignore                  # Copilot除外設定
├── .editorconfig                   # エディタ設定
├── .gitignore                      # Git除外設定
├── pyproject.toml                  # プロジェクト設定
├── requirements.txt                # 依存パッケージ
├── LICENSE                         # ライセンス
└── README.md                       # このファイル
```

---

## ✅ 動作確認

### 1. サンプルコードを作成

```bash
examples/rest-api.py
```

### 2. サンプルコード用のプロンプトを実行

rest-api.py を選択して、次のようなプロンプトをCopilot Chatに入力します。

```bash
REST APIを作成します。
エンドポイントは、次の３つです。

* ヘルスチェック
* メッセージの受信。受信したメッセージをメモリに保存します。
* メッセージの返却。メモリに保存しているメッセージを返却します。
```

### 3. テストコードを作成
```bash
tests/test_rest-api.py
```

### 3. テストコード用のプロンプトを実行

test_rest-api.py を選択して、次のようなプロンプトをCopilot Chatに入力します。

```bash
テストコードを作成します。
テスト対象は、examples/rest-api.pyです。

MC/DCカバレッジを基準にテストコードを作成してください。
```

### 4 テストを実行

```bash
# 全テストを実行
pytest

# カバレッジ付きで実行
pytest --cov=. --cov-report=html

# 詳細出力
pytest -v
```

### 5. コード品質チェック

```bash
# フォーマット
black .

# インポート整理
isort .

# リント
ruff check .

# 型チェック
mypy examples/
```

---

## 🎯 GitHub Copilotの使い方

### インライン補完

1. 新しいPythonファイルを作成
2. コメントを書く: `# ユーザーデータを取得する関数`
3. 次の行で `def` と入力
4. Copilotが関数を提案するので、`Tab`で受け入れ

### Copilot Chat

1. `Ctrl+Shift+I` (Windows/Linux) または `Cmd+Shift+I` (Mac)
2. 質問を入力:
   ```
   このコードを説明してください
   この関数のテストを書いてください
   パフォーマンスを改善してください
   ```

### カスタムプロンプトの使用

1. `.github/PROMPTS.md` を開く
2. 使いたいプロンプトをコピー
3. Copilot Chatに貼り付けて使用

---

## 🛠️ カスタマイズ

### プロジェクト規約の設定

`.github/copilot-instructions.md` を編集:

```markdown
## プロジェクト概要
[あなたのプロジェクトの説明]

## 優先ライブラリ
- HTTP通信: httpx
- ORM: SQLAlchemy
- テスト: pytest
```

### VS Code設定の調整

`.vscode/settings.json` を編集してエディタ設定をカスタマイズできます。

---

## 🔧 トラブルシューティング

### 仮想環境が有効化されない

**Windows:**
PowerShellで実行ポリシーを変更:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### パッケージのインストールエラー

```bash
# pipをアップグレード
python -m pip install --upgrade pip

# 再度インストール
pip install -r requirements.txt
```

### Copilotが動作しない

1. GitHubにサインインしているか確認
2. VS Codeを再起動
3. 拡張機能が有効か確認

### テストが失敗する

```bash
# パッケージを編集可能モードでインストール
pip install -e .

# 再度テスト
pytest
```

---

## 📚 次のステップ

1. **サンプルコードを試す**
   ```bash
   python examples/api_integration.py
   python examples/error_handling.py
   ```

2. **テストを実行**
   ```bash
   pytest -v
   ```

3. **独自のコードを書く**
   - Copilotに提案させながらコーディング
   - `.github/PROMPTS.md` のテンプレートを活用

4. **プロジェクト規約をカスタマイズ**
   - `.github/copilot-instructions.md` を編集
   - チーム固有のルールを追加

---

## 🤝 コントリビューション

プルリクエストを歓迎します！

1. このリポジトリをフォーク
2. フィーチャーブランチを作成
3. 変更をコミット
4. プッシュしてプルリクエストを作成

---

## 📞 サポート

- **GitHub Issues**: バグ報告や機能リクエスト
- **ドキュメント**: `docs/` ディレクトリを参照
- **公式ドキュメント**: [GitHub Copilot Docs](https://docs.github.com/ja/copilot)

---

## 📝 ライセンス

MIT License - 詳細は [LICENSE](LICENSE) を参照

---

**Happy Coding! 🚀**
