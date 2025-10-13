# 環境構築ガイド

FullStack-Logic-AtoZの開発環境をセットアップするための完全ガイドです。

---

## 📑 目次

1. [前提条件](#前提条件)
2. [必要なツールのインストール](#必要なツールのインストール)
3. [プロジェクトのセットアップ](#プロジェクトのセットアップ)
4. [IDEの設定](#ideの設定)
5. [動作確認](#動作確認)
6. [トラブルシューティング](#トラブルシューティング)

---

## 前提条件

### OS要件

- **Windows**: Windows 10/11 (64-bit)
- **macOS**: macOS 12 (Monterey) 以降
- **Linux**: Ubuntu 20.04 LTS 以降

### 必要な知識

- コマンドライン（ターミナル/PowerShell）の基本操作
- テキストエディタの使用経験
- 英語の技術文書を読める程度の英語力（推奨）

---

## 必要なツールのインストール

### 1. Git

バージョン管理システム。

#### Windows:
```powershell
# 公式サイトからダウンロード
https://git-scm.com/download/win

# インストール後、確認
git --version
# 出力例: git version 2.40.0
```

#### macOS:
```bash
# Homebrewでインストール（推奨）
brew install git

# または Xcode Command Line Tools
xcode-select --install

# 確認
git --version
# 出力例: git version 2.40.0
```

#### Linux (Ubuntu):
```bash
sudo apt update
sudo apt install git

# 確認
git --version
```

#### Git初期設定:
```bash
# ユーザー名設定
git config --global user.name "Your Name"

# メールアドレス設定
git config --global user.email "your.email@example.com"

# デフォルトブランチ名
git config --global init.defaultBranch main

# 設定確認
git config --list
```

---

### 2. Node.js (v18以降)

JavaScript/TypeScript実行環境。

#### Windows/macOS/Linux:

**推奨: nvm（Node Version Manager）を使用**

**Windows:**
```powershell
# nvm-windows をダウンロード
https://github.com/coreybutler/nvm-windows/releases

# インストール後
nvm install 18
nvm use 18
node --version
# 出力例: v18.17.0
```

**macOS/Linux:**
```bash
# nvmインストール
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# ターミナル再起動後
nvm install 18
nvm use 18
node --version
# 出力例: v18.17.0

# npm確認
npm --version
# 出力例: 9.6.7
```

**または公式サイトから直接インストール:**
```
https://nodejs.org/
```

---

### 3. Python (v3.10以降)

バックエンド開発、データ分析用。

#### Windows:
```powershell
# 公式サイトからダウンロード
https://www.python.org/downloads/

# インストール時に「Add Python to PATH」をチェック

# 確認
python --version
# 出力例: Python 3.11.0

pip --version
# 出力例: pip 23.0.1
```

#### macOS:
```bash
# Homebrewでインストール
brew install python@3.11

# 確認
python3 --version
pip3 --version
```

#### Linux (Ubuntu):
```bash
sudo apt update
sudo apt install python3.11 python3-pip

# 確認
python3 --version
pip3 --version
```

---

### 4. Docker Desktop

コンテナ環境。

#### Windows/macOS:
```
# 公式サイトからダウンロード
https://www.docker.com/products/docker-desktop/

# インストール後、確認
docker --version
# 出力例: Docker version 24.0.0

docker-compose --version
# 出力例: Docker Compose version v2.20.0
```

#### Linux (Ubuntu):
```bash
# Dockerインストール
sudo apt update
sudo apt install docker.io docker-compose

# ユーザーをdockerグループに追加
sudo usermod -aG docker $USER

# 再ログイン後、確認
docker --version
docker-compose --version
```

---

### 5. VSCode（推奨エディタ）

#### インストール:
```
https://code.visualstudio.com/
```

#### 推奨拡張機能:

**必須:**
- **ESLint** - JavaScript/TypeScriptのリント
- **Prettier** - コードフォーマッター
- **GitLens** - Git統合
- **Docker** - Docker統合

**推奨:**
- **Thunder Client** - API テストツール
- **Error Lens** - エラー表示強化
- **Auto Rename Tag** - HTMLタグ自動リネーム
- **Path Intellisense** - パス補完
- **Todo Tree** - TODOコメント管理
- **Better Comments** - コメント装飾

**言語別:**
- **Python** - Python拡張
- **Pylance** - Python言語サーバー
- **React Developer Tools** - React開発

#### VSCode設定:

`.vscode/settings.json` を作成:
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,
  "editor.tabSize": 2,
  "editor.fontSize": 14,
  "terminal.integrated.fontSize": 13,
  "workbench.colorTheme": "One Dark Pro",
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  }
}
```

---

## プロジェクトのセットアップ

### 1. リポジトリのクローン

```bash
# HTTPSでクローン
git clone https://github.com/YOUR_USERNAME/FullStack-Logic-AtoZ.git

# またはSSH
git clone git@github.com:YOUR_USERNAME/FullStack-Logic-AtoZ.git

# ディレクトリに移動
cd FullStack-Logic-AtoZ
```

---

### 2. 環境変数の設定

```bash
# .env.exampleをコピー
cp .env.example .env

# .envファイルを編集
```

**.env ファイル例:**
```env
# アプリケーション設定
NODE_ENV=development
PORT=3000

# データベース
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
MONGODB_URI=mongodb://localhost:27017/myapp

# API キー
OPENAI_API_KEY=your_openai_api_key_here
NEXT_PUBLIC_API_URL=http://localhost:8000

# 認証
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRES_IN=7d

# その他
LOG_LEVEL=debug
```

**重要:** `.env` ファイルは `.gitignore` に含まれているため、Gitにコミットされません。

---

### 3. フロントエンドのセットアップ

```bash
# フロントエンドディレクトリへ移動
cd 02_implementation/frontend

# 依存関係をインストール
npm install

# 開発サーバー起動
npm run dev
```

**アクセス:**
```
http://localhost:3000
```

#### package.json の主なスクリプト:
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

---

### 4. バックエンドのセットアップ

```bash
# バックエンドディレクトリへ移動
cd 02_implementation/backend

# 依存関係をインストール
npm install

# 開発サーバー起動
npm run dev
```

**アクセス:**
```
http://localhost:8000
API ドキュメント: http://localhost:8000/api/docs
```

---

### 5. AIサービスのセットアップ

```bash
# AIサービスディレクトリへ移動
cd 02_implementation/ai_service

# Pythonパッケージをインストール
pip install -r requirements.txt

# または仮想環境を使用（推奨）
python3 -m venv venv

# 仮想環境を有効化
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# パッケージインストール
pip install -r requirements.txt

# 開発サーバー起動
python main.py
# または
uvicorn main:app --reload
```

**アクセス:**
```
http://localhost:8001
API ドキュメント: http://localhost:8001/docs
```

---

### 6. Docker環境のセットアップ

**すべてのサービスを一度に起動:**

```bash
# プロジェクトルートで
docker-compose up -d

# ログ確認
docker-compose logs -f

# サービス一覧
docker-compose ps

# 停止
docker-compose down
```

**docker-compose.yaml の構成:**
- **frontend**: Next.jsアプリ (port 3000)
- **backend**: Node.js API (port 8000)
- **ai_service**: Python FastAPI (port 8001)
- **postgres**: PostgreSQL データベース (port 5432)
- **mongodb**: MongoDB (port 27017)
- **redis**: Redis キャッシュ (port 6379)

---

## IDEの設定

### VSCodeワークスペース設定

`.vscode/launch.json` でデバッグ設定:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev",
      "cwd": "${workspaceFolder}/02_implementation/frontend"
    },
    {
      "name": "Backend: debug",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/02_implementation/backend/src/index.js",
      "cwd": "${workspaceFolder}/02_implementation/backend"
    },
    {
      "name": "Python: FastAPI",
      "type": "python",
      "request": "launch",
      "module": "uvicorn",
      "args": ["main:app", "--reload"],
      "cwd": "${workspaceFolder}/02_implementation/ai_service"
    }
  ]
}
```

---

## 動作確認

### 1. フロントエンドの確認

```bash
# ブラウザで開く
http://localhost:3000

# 正常に表示されればOK
```

### 2. バックエンドAPIの確認

```bash
# curlでテスト
curl http://localhost:8000/api/health

# 期待される応答:
{
  "status": "healthy",
  "timestamp": "2025-10-13T12:00:00Z"
}
```

### 3. AIサービスの確認

```bash
curl http://localhost:8001/health

# Swagger UIでAPIドキュメント確認
http://localhost:8001/docs
```

### 4. データベース接続確認

```bash
# PostgreSQL
docker exec -it fullstack-postgres psql -U postgres

# MongoDB
docker exec -it fullstack-mongodb mongosh
```

---

## トラブルシューティング

### 問題1: ポートが既に使用されている

**エラー:**
```
Error: listen EADDRINUSE: address already in use :::3000
```

**解決策:**

**Windows:**
```powershell
# ポート使用中のプロセスを確認
netstat -ano | findstr :3000

# プロセスを終了
taskkill /PID <プロセスID> /F
```

**macOS/Linux:**
```bash
# ポート使用中のプロセスを確認
lsof -i :3000

# プロセスを終了
kill -9 <PID>
```

---

### 問題2: npm install でエラー

**解決策:**
```bash
# キャッシュクリア
npm cache clean --force

# node_modules削除
rm -rf node_modules package-lock.json

# 再インストール
npm install
```

---

### 問題3: Dockerコンテナが起動しない

**解決策:**
```bash
# すべてのコンテナを停止
docker-compose down

# ボリュームも削除して再起動
docker-compose down -v
docker-compose up -d

# ログ確認
docker-compose logs
```

---

### 問題4: Python仮想環境の問題

**解決策:**
```bash
# 仮想環境を削除
rm -rf venv

# 再作成
python3 -m venv venv
source venv/bin/activate  # macOS/Linux
venv\Scripts\activate     # Windows

# パッケージ再インストール
pip install -r requirements.txt
```

---

### 問題5: Git認証エラー

**解決策:**

**HTTPS:**
```bash
# 認証情報をキャッシュ
git config --global credential.helper cache
```

**SSH:**
```bash
# SSH鍵生成
ssh-keygen -t ed25519 -C "your.email@example.com"

# 公開鍵をGitHubに登録
cat ~/.ssh/id_ed25519.pub
# GitHubのSettings → SSH and GPG keys で登録
```

---

## 次のステップ

環境構築が完了したら:

1. **[基礎知識を学ぶ](../a_knowledge_base/index.md)**
   - デザインロジック
   - マーケティングロジック

2. **[サンプルコードを試す](../../02_implementation/examples/)**
   - 基本的な実装例

3. **[演習問題に挑戦](../../02_implementation/exercises/)**
   - 実践的な課題

---

## 参考リソース

### 公式ドキュメント
- [Git Documentation](https://git-scm.com/doc)
- [Node.js Documentation](https://nodejs.org/docs/)
- [Python Documentation](https://docs.python.org/)
- [Docker Documentation](https://docs.docker.com/)
- [Next.js Documentation](https://nextjs.org/docs)
- [React Documentation](https://react.dev/)

### チュートリアル
- [freeCodeCamp](https://www.freecodecamp.org/)
- [MDN Web Docs](https://developer.mozilla.org/)
- [Real Python](https://realpython.com/)

---

**最終更新**: 2025-10-13
