# Render デプロイ手順（詳細版）

## RenderのUIについて

RenderのUIは定期的に更新されるため、表示される項目が異なる場合があります。以下のいずれかの方法でデプロイできます。

## 方法1: リポジトリから直接デプロイ（推奨）

### ステップ1: GitHubにプッシュ

```bash
# まだGitリポジトリを初期化していない場合
git init
git add .
git commit -m "Initial commit"

# GitHubにリポジトリを作成（GitHubのウェブサイトで作成）
# その後、以下を実行
git remote add origin https://github.com/your-username/your-repo-name.git
git branch -M main
git push -u origin main
```

### ステップ2: Renderでデプロイ

1. [Render](https://render.com) にアクセスしてログイン
2. ダッシュボードで「New +」ボタンをクリック
3. 表示されるオプションから以下を選択：
   - **「From Git Repository」** ← これが最も簡単
   - または **「Web Service」**
   - または **「Blueprint」**

4. GitHubアカウントを接続（初回のみ）
5. リポジトリを選択
6. Renderが自動的にFlaskアプリを検出します

### ステップ3: 設定を確認・調整

自動検出された設定を確認し、必要に応じて調整：

- **Name**: アプリ名（例: `kintai-check-app`）
- **Region**: `Singapore` または `Oregon`（日本に近い地域）
- **Branch**: `main`
- **Runtime**: `Python 3`
- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `gunicorn app:app`

### ステップ4: デプロイ開始

「Create Web Service」または「Deploy」ボタンをクリック

## 方法2: render.yamlを使用（自動設定）

`render.yaml` ファイルがリポジトリに含まれている場合：

1. 「New +」→「Blueprint」を選択
2. GitHubリポジトリを選択
3. Renderが自動的に `render.yaml` を読み込んで設定します

## 方法3: 手動で作成

もし上記のオプションが表示されない場合：

1. 「New +」をクリック
2. 表示されるメニューから：
   - 「Web Service」を探す
   - または「Create new」→「Web Service」
   - または「Services」→「New Web Service」

3. GitHubリポジトリを接続
4. 上記の設定を手動で入力

## よくある問題と解決方法

### 「Web Service」が見つからない場合

RenderのUIは更新されるため、以下の代替方法を試してください：

1. **「From Git Repository」を選択**
   - これが最も確実な方法です
   - リポジトリを選択すると、自動的にFlaskアプリとして認識されます

2. **「Blueprint」を選択**
   - `render.yaml` ファイルがある場合、自動的に設定されます

3. **「Static Site」ではないことに注意**
   - Flaskアプリは「Web Service」または「Web App」です
   - 「Static Site」は静的サイト用です

### リポジトリが表示されない場合

1. GitHubアカウントが正しく接続されているか確認
2. Renderの設定でGitHubアカウントを再接続
3. リポジトリがプライベートの場合、Renderがアクセスできるか確認

## デプロイ後の確認

1. デプロイが完了すると、URLが表示されます（例: `https://your-app-name.onrender.com`）
2. このURLにアクセスしてアプリが動作するか確認
3. エラーがある場合は、「Logs」タブでログを確認

## トラブルシューティング

### ビルドエラー

- `requirements.txt` が正しいか確認
- Pythonバージョンが `runtime.txt` と一致しているか確認

### 起動エラー

- `gunicorn` が `requirements.txt` に含まれているか確認
- `Procfile` の内容が正しいか確認（`web: gunicorn app:app`）

### 404エラー

- ルートパス（`/`）が正しく設定されているか確認
- `app.py` の `@app.route('/')` が存在するか確認

## サポート

問題が解決しない場合：
1. Renderのドキュメント: https://render.com/docs
2. Renderのサポート: support@render.com
3. ログを確認してエラーメッセージを特定

