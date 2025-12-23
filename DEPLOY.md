# Render デプロイ手順

## 1. GitHubリポジトリの準備

### リポジトリを初期化（まだの場合）

```bash
git init
git add .
git commit -m "Initial commit"
```

### GitHubにリポジトリを作成

1. [GitHub](https://github.com) にログイン
2. 右上の「+」→「New repository」をクリック
3. リポジトリ名を入力（例: `kintai-check-app`）
4. 「Create repository」をクリック

### リポジトリにプッシュ

```bash
git remote add origin https://github.com/your-username/your-repo-name.git
git branch -M main
git push -u origin main
```

## 2. Renderでデプロイ

### アカウント作成

1. [Render](https://render.com) にアクセス
2. 「Get Started for Free」をクリック
3. GitHubアカウントでサインアップ

### Webサービスの作成

Renderのダッシュボードで以下のいずれかの方法で作成：

**方法1: 直接作成**
1. Renderダッシュボードで「New +」ボタンをクリック
2. 以下のいずれかを選択：
   - 「Web Service」（表示されている場合）
   - 「Blueprint」（表示されている場合、こちらを選択）
   - 「From Git Repository」（表示されている場合）

**方法2: リポジトリから自動検出**
1. 「New +」→「From Git Repository」をクリック
2. GitHubリポジトリを選択
3. Renderが自動的にFlaskアプリを検出します

**方法3: 手動設定**
1. 「New +」→「Web Service」または「New Web Service」をクリック
2. 「Connect GitHub」または「Connect account」をクリック
3. GitHubアカウントを接続（初回のみ）
4. リポジトリを選択

### 設定

以下の設定を入力：

- **Name**: `kintai-check-app`（任意の名前）
- **Region**: `Singapore` または `Oregon`（日本に近い地域）
- **Branch**: `main`
- **Root Directory**: （空白のまま）
- **Runtime**: `Python 3`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn app:app`

### 環境変数（オプション）

必要に応じて以下を追加：

- `PYTHON_VERSION`: `3.11.0`（runtime.txtで指定済みのため不要）

### デプロイ開始

1. 「Create Web Service」をクリック
2. ビルドとデプロイが自動的に開始されます
3. 数分待つとデプロイが完了します

## 3. デプロイ後の確認

- デプロイが完了すると、`https://your-app-name.onrender.com` のようなURLが表示されます
- このURLにアクセスしてアプリが正常に動作するか確認してください

## 4. 注意事項

### ファイルストレージ

- Renderの無料プランでは、ファイルシステムは一時的です
- 生成されたエクセルファイルは、一定時間後に自動的に削除される可能性があります
- 永続的な保存が必要な場合は、S3などの外部ストレージサービスを統合することを検討してください

### タイムアウト

- 無料プランでは、15分間アクセスがないとスリープします
- 次回アクセス時に自動的に起動しますが、初回起動に時間がかかることがあります

### ログの確認

- Renderダッシュボードの「Logs」タブでログを確認できます
- エラーが発生した場合は、ログを確認して問題を特定してください

## トラブルシューティング

### ビルドエラー

- `requirements.txt` の依存関係を確認
- Pythonバージョンが正しいか確認（runtime.txt）

### 起動エラー

- `gunicorn` がインストールされているか確認
- `Procfile` の内容を確認

### ファイルアップロードエラー

- ファイルサイズが16MB以下か確認
- エラーログを確認

## カスタムドメイン（オプション）

1. Renderダッシュボードで「Settings」→「Custom Domains」
2. ドメインを追加
3. DNS設定を更新

