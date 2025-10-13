# 基本設計ガイド

**目的**: 要件定義をもとに、システムの全体構成、アーキテクチャ、データ構造を設計する

**学習時間**: 約8時間
**前提知識**: 要件定義、データベースの基礎、REST APIの基本
**関連ドキュメント**:
- [要件定義](../b_requirements/README.md)
- [詳細設計](../d_detailed_design/README.md)
- [セキュリティ設計](../f_security_and_compliance/README.md)

---

## 目次

1. [基本設計とは](#1-基本設計とは)
2. [システムアーキテクチャ設計](#2-システムアーキテクチャ設計)
3. [データベース設計](#3-データベース設計)
4. [API設計](#4-api設計)
5. [UI/UX設計](#5-uiux設計)
6. [実践演習](#6-実践演習)

---

## 1. 基本設計とは

### 1.1 基本設計の目的

基本設計は、**「どのように作るのか」** の全体像を定義するプロセスです。

**主な目的**:
- システム全体のアーキテクチャを決定
- 技術スタックの選定
- データ構造の設計
- モジュール間の連携方法の定義
- 開発チームへの設計指針の提供

### 1.2 基本設計 vs 詳細設計

| 観点 | 基本設計 | 詳細設計 |
|------|---------|---------|
| **粒度** | 全体像・構造 | 実装レベルの詳細 |
| **対象** | システム全体 | 個別のモジュール・関数 |
| **成果物** | アーキテクチャ図、ER図、API仕様 | クラス図、シーケンス図、アルゴリズム |
| **読者** | プロジェクトマネージャー、アーキテクト | 開発者 |
| **例** | 「3層アーキテクチャを採用」 | 「UserService.createUser()の処理フロー」 |

### 1.3 基本設計のプロセス

```
┌────────────────────────────────────────────────────────────┐
│ Step 1: アーキテクチャ設計                                  │
│ システム構成、技術スタック選定                              │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 2: データベース設計                                    │
│ ER図、テーブル定義、インデックス設計                        │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 3: API設計                                             │
│ エンドポイント定義、リクエスト/レスポンス仕様               │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 4: UI/UX設計                                           │
│ ワイヤーフレーム、画面遷移図、デザインシステム             │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 5: セキュリティ設計                                    │
│ 認証・認可、データ保護、脆弱性対策                          │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 6: インフラ設計                                        │
│ サーバー構成、ネットワーク、監視                            │
└────────────────────────────────────────────────────────────┘
```

---

## 2. システムアーキテクチャ設計

### 2.1 アーキテクチャパターンの選択

#### 3層アーキテクチャ（レイヤードアーキテクチャ）

**構成**:
```
┌─────────────────────────────────────────┐
│         プレゼンテーション層            │
│    (フロントエンド: React/Next.js)      │
└────────────┬────────────────────────────┘
             │ HTTP/REST API
             ▼
┌─────────────────────────────────────────┐
│         ビジネスロジック層              │
│   (バックエンド: Node.js/Express)       │
└────────────┬────────────────────────────┘
             │ SQL/ORM
             ▼
┌─────────────────────────────────────────┐
│         データアクセス層                │
│   (データベース: PostgreSQL)            │
└─────────────────────────────────────────┘
```

**特徴**:
- シンプルで理解しやすい
- 中小規模アプリに適している
- モジュール分離が明確

**適用例**: タスク管理SaaS、ECサイト、社内ツール

---

#### マイクロサービスアーキテクチャ

**構成**:
```
┌───────────────┐
│  フロント     │
│  エンド       │
└───┬───────────┘
    │
    │ API Gateway
    ▼
┌───────────────────────────────────────────────┐
│            API Gateway / BFF                  │
└───┬───────────┬───────────┬───────────────────┘
    │           │           │
    ▼           ▼           ▼
┌─────────┐ ┌─────────┐ ┌─────────┐
│ユーザー │ │タスク   │ │通知     │
│サービス │ │サービス │ │サービス │
└────┬────┘ └────┬────┘ └────┬────┘
     │           │           │
     ▼           ▼           ▼
┌─────────┐ ┌─────────┐ ┌─────────┐
│  DB 1   │ │  DB 2   │ │  DB 3   │
└─────────┘ └─────────┘ └─────────┘
```

**特徴**:
- サービスごとに独立した開発・デプロイ
- スケーラビリティが高い
- 複雑性が増す

**適用例**: 大規模Webサービス、Netflix、Uber

---

#### サーバーレスアーキテクチャ

**構成**:
```
┌────────────────┐
│  フロントエンド │
│  (Next.js)     │
└───┬────────────┘
    │
    ▼
┌────────────────┐
│  API Gateway   │
└───┬────────────┘
    │
    ├──► Lambda Function 1 (ユーザー認証)
    │
    ├──► Lambda Function 2 (タスク管理)
    │
    └──► Lambda Function 3 (通知)
         │
         ▼
    ┌────────────────┐
    │  DynamoDB /    │
    │  RDS           │
    └────────────────┘
```

**特徴**:
- インフラ管理不要
- 自動スケーリング
- 従量課金（使った分だけ）

**適用例**: スタートアップMVP、イベント駆動型アプリ

---

### 2.2 技術スタックの選定

#### フロントエンド技術スタック

**タスク管理SaaSの例**:

```markdown
## フロントエンド技術スタック

| カテゴリ | 技術 | 選定理由 |
|---------|------|----------|
| **フレームワーク** | Next.js 14 (App Router) | SSR/SSG対応、SEO最適化、TypeScript標準 |
| **言語** | TypeScript | 型安全、大規模開発に適している |
| **スタイリング** | Tailwind CSS | ユーティリティファースト、カスタマイズ容易 |
| **状態管理** | Zustand | シンプル、軽量、学習コスト低い |
| **フォーム管理** | React Hook Form | パフォーマンス、バリデーション統合 |
| **データフェッチ** | TanStack Query (React Query) | キャッシュ、自動再取得、楽観的更新 |
| **UI コンポーネント** | Radix UI + shadcn/ui | アクセシビリティ、カスタマイズ性 |
| **アイコン** | Lucide React | 軽量、一貫性 |
| **バリデーション** | Zod | TypeScript統合、スキーマ定義 |
| **日付処理** | date-fns | 軽量、tree-shaking対応 |
| **チャート** | Recharts | React統合、カスタマイズ可能 |
| **テスト** | Vitest + Testing Library | 高速、Jest互換 |
| **E2Eテスト** | Playwright | 信頼性、並列実行 |
```

#### バックエンド技術スタック

```markdown
## バックエンド技術スタック

| カテゴリ | 技術 | 選定理由 |
|---------|------|----------|
| **ランタイム** | Node.js 20 LTS | JavaScript統合、エコシステム豊富 |
| **フレームワーク** | Express.js | シンプル、柔軟、実績豊富 |
| **言語** | TypeScript | 型安全、フロントと統一 |
| **ORM** | Prisma | 型安全、マイグレーション管理 |
| **データベース** | PostgreSQL 16 | ACID、JSON対応、スケーラブル |
| **認証** | JWT + bcrypt | ステートレス、セキュア |
| **バリデーション** | Zod | フロントと統一 |
| **API ドキュメント** | Swagger (OpenAPI) | 自動生成、テスト可能 |
| **ロギング** | Winston | 構造化ログ、複数出力先 |
| **テスト** | Vitest + Supertest | API テスト、高速 |
| **キャッシュ** | Redis | インメモリ、高速 |
| **ジョブキュー** | BullMQ | Redis ベース、リトライ機能 |
```

#### インフラ技術スタック

```markdown
## インフラ技術スタック

| カテゴリ | 技術 | 選定理由 |
|---------|------|----------|
| **ホスティング (FE)** | Vercel | Next.js最適化、自動デプロイ |
| **ホスティング (BE)** | AWS ECS (Fargate) | コンテナ、自動スケーリング |
| **データベース** | AWS RDS (PostgreSQL) | マネージド、自動バックアップ |
| **ストレージ** | AWS S3 | オブジェクトストレージ、CDN連携 |
| **CDN** | CloudFront | 低レイテンシ、キャッシュ |
| **キャッシュ** | AWS ElastiCache (Redis) | マネージド Redis |
| **監視** | Datadog / CloudWatch | メトリクス、ログ集約 |
| **エラートラッキング** | Sentry | リアルタイム、スタックトレース |
| **CI/CD** | GitHub Actions | 自動テスト、デプロイ |
| **コンテナ** | Docker | 環境統一、ポータビリティ |
| **IaC** | Terraform | インフラコード化 |
```

### 2.3 システム構成図

#### タスク管理SaaSのシステム構成図

```
┌──────────────────────────────────────────────────────────────┐
│                         ユーザー                             │
└────────────┬─────────────────────────────────────────────────┘
             │
             │ HTTPS
             ▼
┌──────────────────────────────────────────────────────────────┐
│                      CloudFront (CDN)                         │
└────────────┬─────────────────────────────────────────────────┘
             │
      ┌──────┴──────┐
      │             │
      ▼             ▼
┌─────────────┐  ┌──────────────────────────┐
│   Vercel    │  │    AWS ALB               │
│  (Next.js)  │  │  (Load Balancer)         │
└─────────────┘  └──────────┬───────────────┘
                            │
                ┌───────────┴───────────┐
                │                       │
                ▼                       ▼
        ┌──────────────┐        ┌──────────────┐
        │  ECS Task 1  │        │  ECS Task 2  │
        │  (Express)   │        │  (Express)   │
        └──────┬───────┘        └──────┬───────┘
               │                       │
               └───────────┬───────────┘
                           │
           ┌───────────────┼───────────────┐
           │               │               │
           ▼               ▼               ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │   RDS    │    │  Redis   │    │   S3     │
    │(Postgres)│    │(キャッシュ)│    │(ファイル)│
    └──────────┘    └──────────┘    └──────────┘
```

---

## 3. データベース設計

### 3.1 ER図（Entity Relationship Diagram）

#### タスク管理SaaSのER図

```
┌─────────────────┐
│     users       │
├─────────────────┤
│ id (PK)         │
│ email (UNIQUE)  │
│ password_hash   │
│ name            │
│ avatar_url      │
│ created_at      │
│ updated_at      │
└────────┬────────┘
         │
         │ 1:N
         │
         ▼
┌─────────────────┐          ┌─────────────────┐
│  team_members   │ N:1      │     teams       │
├─────────────────┤◄─────────├─────────────────┤
│ id (PK)         │          │ id (PK)         │
│ team_id (FK)    │          │ name            │
│ user_id (FK)    │          │ description     │
│ role            │          │ created_by (FK) │
│ joined_at       │          │ created_at      │
└────────┬────────┘          └────────┬────────┘
         │                            │
         │                            │ 1:N
         │                            │
         │                            ▼
         │                   ┌─────────────────┐
         │                   │    projects     │
         │                   ├─────────────────┤
         │                   │ id (PK)         │
         │                   │ team_id (FK)    │
         │                   │ name            │
         │                   │ description     │
         │                   │ created_at      │
         │                   └────────┬────────┘
         │                            │
         │                            │ 1:N
         │                            │
         │                            ▼
         │                   ┌─────────────────┐
         └──────────────────►│     tasks       │
                             ├─────────────────┤
                             │ id (PK)         │
                             │ project_id (FK) │
                             │ title           │
                             │ description     │
                             │ assigned_to (FK)│
                             │ status          │
                             │ priority        │
                             │ due_date        │
                             │ created_by (FK) │
                             │ created_at      │
                             │ updated_at      │
                             └────────┬────────┘
                                      │
                                      │ 1:N
                                      │
                                      ▼
                             ┌─────────────────┐
                             │    comments     │
                             ├─────────────────┤
                             │ id (PK)         │
                             │ task_id (FK)    │
                             │ user_id (FK)    │
                             │ content         │
                             │ created_at      │
                             └─────────────────┘
```

### 3.2 テーブル定義

#### usersテーブル

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  name VARCHAR(100) NOT NULL,
  avatar_url VARCHAR(500),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- インデックス
CREATE INDEX idx_users_email ON users(email);

-- トリガー（updated_at自動更新）
CREATE TRIGGER update_users_updated_at
  BEFORE UPDATE ON users
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at_column();
```

#### teamsテーブル

```sql
CREATE TABLE teams (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  description TEXT,
  created_by INTEGER REFERENCES users(id) ON DELETE SET NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- インデックス
CREATE INDEX idx_teams_created_by ON teams(created_by);
```

#### team_membersテーブル（中間テーブル）

```sql
CREATE TYPE team_role AS ENUM ('owner', 'admin', 'member');

CREATE TABLE team_members (
  id SERIAL PRIMARY KEY,
  team_id INTEGER NOT NULL REFERENCES teams(id) ON DELETE CASCADE,
  user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  role team_role NOT NULL DEFAULT 'member',
  joined_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(team_id, user_id)  -- 同じユーザーが同じチームに2回入れない
);

-- インデックス
CREATE INDEX idx_team_members_team_id ON team_members(team_id);
CREATE INDEX idx_team_members_user_id ON team_members(user_id);
```

#### projectsテーブル

```sql
CREATE TABLE projects (
  id SERIAL PRIMARY KEY,
  team_id INTEGER NOT NULL REFERENCES teams(id) ON DELETE CASCADE,
  name VARCHAR(100) NOT NULL,
  description TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- インデックス
CREATE INDEX idx_projects_team_id ON projects(team_id);
```

#### tasksテーブル

```sql
CREATE TYPE task_status AS ENUM ('todo', 'in_progress', 'done', 'archived');
CREATE TYPE task_priority AS ENUM ('low', 'medium', 'high', 'urgent');

CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  project_id INTEGER NOT NULL REFERENCES projects(id) ON DELETE CASCADE,
  title VARCHAR(200) NOT NULL,
  description TEXT,
  assigned_to INTEGER REFERENCES users(id) ON DELETE SET NULL,
  status task_status NOT NULL DEFAULT 'todo',
  priority task_priority NOT NULL DEFAULT 'medium',
  due_date DATE,
  created_by INTEGER REFERENCES users(id) ON DELETE SET NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- インデックス
CREATE INDEX idx_tasks_project_id ON tasks(project_id);
CREATE INDEX idx_tasks_assigned_to ON tasks(assigned_to);
CREATE INDEX idx_tasks_status ON tasks(status);
CREATE INDEX idx_tasks_due_date ON tasks(due_date);

-- 複合インデックス（よく使うクエリパターン）
CREATE INDEX idx_tasks_project_status ON tasks(project_id, status);
```

#### commentsテーブル

```sql
CREATE TABLE comments (
  id SERIAL PRIMARY KEY,
  task_id INTEGER NOT NULL REFERENCES tasks(id) ON DELETE CASCADE,
  user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  content TEXT NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- インデックス
CREATE INDEX idx_comments_task_id ON comments(task_id);
CREATE INDEX idx_comments_user_id ON comments(user_id);
```

### 3.3 Prismaスキーマ定義

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id           Int      @id @default(autoincrement())
  email        String   @unique
  passwordHash String   @map("password_hash")
  name         String
  avatarUrl    String?  @map("avatar_url")
  createdAt    DateTime @default(now()) @map("created_at")
  updatedAt    DateTime @updatedAt @map("updated_at")

  // リレーション
  teamMemberships TeamMember[]
  createdTeams    Team[]       @relation("TeamCreator")
  assignedTasks   Task[]       @relation("TaskAssignee")
  createdTasks    Task[]       @relation("TaskCreator")
  comments        Comment[]

  @@map("users")
}

model Team {
  id          Int      @id @default(autoincrement())
  name        String
  description String?
  createdById Int?     @map("created_by")
  createdAt   DateTime @default(now()) @map("created_at")
  updatedAt   DateTime @updatedAt @map("updated_at")

  // リレーション
  createdBy User?         @relation("TeamCreator", fields: [createdById], references: [id], onDelete: SetNull)
  members   TeamMember[]
  projects  Project[]

  @@map("teams")
}

enum TeamRole {
  owner
  admin
  member
}

model TeamMember {
  id       Int      @id @default(autoincrement())
  teamId   Int      @map("team_id")
  userId   Int      @map("user_id")
  role     TeamRole @default(member)
  joinedAt DateTime @default(now()) @map("joined_at")

  // リレーション
  team Team @relation(fields: [teamId], references: [id], onDelete: Cascade)
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([teamId, userId])
  @@map("team_members")
}

model Project {
  id          Int      @id @default(autoincrement())
  teamId      Int      @map("team_id")
  name        String
  description String?
  createdAt   DateTime @default(now()) @map("created_at")
  updatedAt   DateTime @updatedAt @map("updated_at")

  // リレーション
  team  Team   @relation(fields: [teamId], references: [id], onDelete: Cascade)
  tasks Task[]

  @@map("projects")
}

enum TaskStatus {
  todo
  in_progress
  done
  archived
}

enum TaskPriority {
  low
  medium
  high
  urgent
}

model Task {
  id          Int          @id @default(autoincrement())
  projectId   Int          @map("project_id")
  title       String
  description String?
  assignedTo  Int?         @map("assigned_to")
  status      TaskStatus   @default(todo)
  priority    TaskPriority @default(medium)
  dueDate     DateTime?    @map("due_date") @db.Date
  createdBy   Int?         @map("created_by")
  createdAt   DateTime     @default(now()) @map("created_at")
  updatedAt   DateTime     @updatedAt @map("updated_at")

  // リレーション
  project  Project   @relation(fields: [projectId], references: [id], onDelete: Cascade)
  assignee User?     @relation("TaskAssignee", fields: [assignedTo], references: [id], onDelete: SetNull)
  creator  User?     @relation("TaskCreator", fields: [createdBy], references: [id], onDelete: SetNull)
  comments Comment[]

  @@index([projectId, status])
  @@map("tasks")
}

model Comment {
  id        Int      @id @default(autoincrement())
  taskId    Int      @map("task_id")
  userId    Int      @map("user_id")
  content   String
  createdAt DateTime @default(now()) @map("created_at")

  // リレーション
  task Task @relation(fields: [taskId], references: [id], onDelete: Cascade)
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@map("comments")
}
```

---

## 4. API設計

### 4.1 RESTful API設計原則

#### RESTの基本原則

1. **リソース指向**: URLはリソースを表す名詞
   - ✅ `GET /api/tasks`
   - ❌ `GET /api/getTasks`

2. **HTTPメソッドの適切な使用**:
   - `GET`: 取得
   - `POST`: 作成
   - `PUT`: 全体更新
   - `PATCH`: 部分更新
   - `DELETE`: 削除

3. **ステートレス**: 各リクエストは独立

4. **階層構造**: リソースの関係を表現
   - `GET /api/projects/:projectId/tasks`

#### HTTPステータスコードの使い分け

```markdown
| ステータスコード | 意味 | 使用例 |
|----------------|------|--------|
| **200 OK** | 成功 | GET, PUT, PATCH の成功 |
| **201 Created** | 作成成功 | POST でリソース作成 |
| **204 No Content** | 成功（レスポンスなし） | DELETE 成功 |
| **400 Bad Request** | リクエストエラー | バリデーションエラー |
| **401 Unauthorized** | 未認証 | トークンなし/無効 |
| **403 Forbidden** | 権限なし | アクセス権限なし |
| **404 Not Found** | リソースなし | 存在しないID |
| **409 Conflict** | 競合 | 一意制約違反 |
| **422 Unprocessable Entity** | バリデーションエラー | 入力値エラー |
| **500 Internal Server Error** | サーバーエラー | 予期せぬエラー |
```

### 4.2 API エンドポイント一覧

#### 認証API

```markdown
| メソッド | エンドポイント | 説明 | 認証 |
|---------|---------------|------|------|
| POST | /api/auth/register | ユーザー登録 | 不要 |
| POST | /api/auth/login | ログイン | 不要 |
| POST | /api/auth/logout | ログアウト | 必要 |
| POST | /api/auth/refresh | トークン更新 | 必要 |
| POST | /api/auth/forgot-password | パスワードリセット依頼 | 不要 |
| POST | /api/auth/reset-password | パスワードリセット実行 | 不要 |
```

#### ユーザーAPI

```markdown
| メソッド | エンドポイント | 説明 | 認証 |
|---------|---------------|------|------|
| GET | /api/users/me | 自分の情報取得 | 必要 |
| PATCH | /api/users/me | 自分の情報更新 | 必要 |
| DELETE | /api/users/me | アカウント削除 | 必要 |
```

#### チームAPI

```markdown
| メソッド | エンドポイント | 説明 | 認証 |
|---------|---------------|------|------|
| GET | /api/teams | チーム一覧取得 | 必要 |
| POST | /api/teams | チーム作成 | 必要 |
| GET | /api/teams/:id | チーム詳細取得 | 必要 |
| PATCH | /api/teams/:id | チーム更新 | 必要 |
| DELETE | /api/teams/:id | チーム削除 | 必要 |
| POST | /api/teams/:id/members | メンバー招待 | 必要 |
| DELETE | /api/teams/:id/members/:userId | メンバー削除 | 必要 |
```

#### プロジェクトAPI

```markdown
| メソッド | エンドポイント | 説明 | 認証 |
|---------|---------------|------|------|
| GET | /api/teams/:teamId/projects | プロジェクト一覧 | 必要 |
| POST | /api/teams/:teamId/projects | プロジェクト作成 | 必要 |
| GET | /api/projects/:id | プロジェクト詳細 | 必要 |
| PATCH | /api/projects/:id | プロジェクト更新 | 必要 |
| DELETE | /api/projects/:id | プロジェクト削除 | 必要 |
```

#### タスクAPI

```markdown
| メソッド | エンドポイント | 説明 | 認証 |
|---------|---------------|------|------|
| GET | /api/projects/:projectId/tasks | タスク一覧 | 必要 |
| POST | /api/projects/:projectId/tasks | タスク作成 | 必要 |
| GET | /api/tasks/:id | タスク詳細 | 必要 |
| PATCH | /api/tasks/:id | タスク更新 | 必要 |
| DELETE | /api/tasks/:id | タスク削除 | 必要 |
| POST | /api/tasks/:id/comments | コメント追加 | 必要 |
```

### 4.3 API仕様（OpenAPI）

#### タスク作成APIの例

```yaml
# openapi.yaml
openapi: 3.0.0
info:
  title: Task Management API
  version: 1.0.0
  description: タスク管理SaaS API

servers:
  - url: https://api.example.com/v1
    description: Production
  - url: http://localhost:8000/v1
    description: Development

paths:
  /projects/{projectId}/tasks:
    post:
      summary: タスク作成
      description: 新しいタスクを作成します
      tags:
        - Tasks
      security:
        - BearerAuth: []
      parameters:
        - name: projectId
          in: path
          required: true
          schema:
            type: integer
          description: プロジェクトID
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - title
              properties:
                title:
                  type: string
                  minLength: 1
                  maxLength: 200
                  example: "ログイン画面の実装"
                description:
                  type: string
                  maxLength: 5000
                  example: "メールアドレスとパスワードでログインできる画面を実装する"
                assignedTo:
                  type: integer
                  example: 123
                priority:
                  type: string
                  enum: [low, medium, high, urgent]
                  default: medium
                  example: "high"
                dueDate:
                  type: string
                  format: date
                  example: "2025-10-20"
      responses:
        '201':
          description: タスク作成成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Task'
        '400':
          description: バリデーションエラー
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '401':
          description: 未認証
        '403':
          description: 権限なし
        '404':
          description: プロジェクトが存在しない

    get:
      summary: タスク一覧取得
      description: プロジェクトのタスク一覧を取得します
      tags:
        - Tasks
      security:
        - BearerAuth: []
      parameters:
        - name: projectId
          in: path
          required: true
          schema:
            type: integer
        - name: status
          in: query
          schema:
            type: string
            enum: [todo, in_progress, done, archived]
          description: ステータスでフィルター
        - name: assignedTo
          in: query
          schema:
            type: integer
          description: 担当者IDでフィルター
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/Task'
                  pagination:
                    $ref: '#/components/schemas/Pagination'

components:
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  schemas:
    Task:
      type: object
      properties:
        id:
          type: integer
          example: 456
        projectId:
          type: integer
          example: 789
        title:
          type: string
          example: "ログイン画面の実装"
        description:
          type: string
          example: "メールアドレスとパスワードでログインできる画面を実装する"
        assignedTo:
          type: integer
          nullable: true
          example: 123
        status:
          type: string
          enum: [todo, in_progress, done, archived]
          example: "todo"
        priority:
          type: string
          enum: [low, medium, high, urgent]
          example: "high"
        dueDate:
          type: string
          format: date
          nullable: true
          example: "2025-10-20"
        createdBy:
          type: integer
          example: 123
        createdAt:
          type: string
          format: date-time
          example: "2025-10-13T10:30:00Z"
        updatedAt:
          type: string
          format: date-time
          example: "2025-10-13T10:30:00Z"

    Pagination:
      type: object
      properties:
        page:
          type: integer
          example: 1
        limit:
          type: integer
          example: 20
        total:
          type: integer
          example: 100
        totalPages:
          type: integer
          example: 5

    Error:
      type: object
      properties:
        error:
          type: string
          example: "Validation error"
        message:
          type: string
          example: "title is required"
        details:
          type: array
          items:
            type: object
            properties:
              field:
                type: string
                example: "title"
              message:
                type: string
                example: "Required field"
```

---

## 5. UI/UX設計

### 5.1 ワイヤーフレーム

#### ダッシュボード画面

```
┌─────────────────────────────────────────────────────────────┐
│  Logo    [検索]           [通知]  [ユーザー▼]               │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌─────────────┐  ┌───────────────────────────────────────┐│
│  │             │  │  今日のタスク                         ││
│  │  サイド     │  │  ┌─────────────────────────────────┐ ││
│  │  バー       │  │  │ □ ログイン画面の実装            │ ││
│  │             │  │  │   期限: 今日  優先度: 高        │ ││
│  │  - ダッシュ │  │  └─────────────────────────────────┘ ││
│  │    ボード   │  │  ┌─────────────────────────────────┐ ││
│  │  - タスク   │  │  │ □ APIテスト作成                 │ ││
│  │  - プロジェ │  │  │   期限: 明日  優先度: 中        │ ││
│  │    クト     │  │  └─────────────────────────────────┘ ││
│  │  - チーム   │  │                                       ││
│  │  - 設定     │  │  進捗グラフ                           ││
│  │             │  │  ┌─────────────────────────────────┐ ││
│  │             │  │  │  ■■■■■■░░░░  60%          ││
│  │             │  │  │  完了: 6/10タスク               │ ││
│  │             │  │  └─────────────────────────────────┘ ││
│  └─────────────┘  └───────────────────────────────────────┘│
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

#### タスク一覧画面

```
┌─────────────────────────────────────────────────────────────┐
│  プロジェクトA  ▼       [+ 新規タスク]                      │
├─────────────────────────────────────────────────────────────┤
│  フィルター: [全て▼] [担当者▼] [優先度▼]  並替: [期限▼]    │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ □ ログイン画面の実装            [高] [進行中]        │  │
│  │   担当: 山田太郎  期限: 2025-10-15                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ □ データベース設計              [中] [未着手]        │  │
│  │   担当: 佐藤花子  期限: 2025-10-20                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                               │
│  ┌──────────────────────────────────────────────────────┐  │
│  │ ✓ 要件定義書作成                [中] [完了]          │  │
│  │   担当: 山田太郎  完了: 2025-10-10                   │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 画面遷移図

```
[ログイン画面]
     │
     ├─ログイン成功
     │
     ▼
[ダッシュボード] ──────► [タスク一覧] ──────► [タスク詳細]
     │                       │                      │
     │                       │                      ├─ 編集
     │                       │                      ├─ 削除
     │                       │                      └─ コメント追加
     │                       │
     │                       └─ [タスク作成]
     │
     ├──► [プロジェクト一覧] ──► [プロジェクト詳細]
     │                               │
     │                               └─ [タスク一覧]
     │
     ├──► [チーム管理] ──► [メンバー招待]
     │
     └──► [設定]
           ├─ プロフィール編集
           ├─ パスワード変更
           └─ 通知設定
```

### 5.3 デザインシステム

#### カラーパレット

```typescript
// design-system/colors.ts
export const colors = {
  // Primary
  primary: {
    50: '#eff6ff',
    100: '#dbeafe',
    200: '#bfdbfe',
    300: '#93c5fd',
    400: '#60a5fa',
    500: '#3b82f6',  // メインカラー
    600: '#2563eb',
    700: '#1d4ed8',
    800: '#1e40af',
    900: '#1e3a8a',
  },
  // Neutral
  neutral: {
    50: '#f9fafb',
    100: '#f3f4f6',
    200: '#e5e7eb',
    300: '#d1d5db',
    400: '#9ca3af',
    500: '#6b7280',
    600: '#4b5563',
    700: '#374151',
    800: '#1f2937',
    900: '#111827',
  },
  // Status
  success: '#10b981',
  warning: '#f59e0b',
  error: '#ef4444',
  info: '#3b82f6',
}
```

#### タイポグラフィ

```typescript
// design-system/typography.ts
export const typography = {
  fontFamily: {
    sans: ['Inter', 'system-ui', 'sans-serif'],
    mono: ['Fira Code', 'monospace'],
  },
  fontSize: {
    xs: '0.75rem',    // 12px
    sm: '0.875rem',   // 14px
    base: '1rem',     // 16px
    lg: '1.125rem',   // 18px
    xl: '1.25rem',    // 20px
    '2xl': '1.5rem',  // 24px
    '3xl': '1.875rem',// 30px
    '4xl': '2.25rem', // 36px
  },
  fontWeight: {
    normal: 400,
    medium: 500,
    semibold: 600,
    bold: 700,
  },
}
```

#### スペーシング

```typescript
// design-system/spacing.ts
export const spacing = {
  0: '0',
  1: '0.25rem',  // 4px
  2: '0.5rem',   // 8px
  3: '0.75rem',  // 12px
  4: '1rem',     // 16px
  5: '1.25rem',  // 20px
  6: '1.5rem',   // 24px
  8: '2rem',     // 32px
  10: '2.5rem',  // 40px
  12: '3rem',    // 48px
  16: '4rem',    // 64px
}
```

---

## 6. 実践演習

### 演習1: ER図作成

**課題**: 「オンライン学習プラットフォーム」のER図を作成せよ。

**エンティティ**:
- ユーザー
- コース
- レッスン
- 受講履歴
- レビュー

**要件**:
- 1つのコースは複数のレッスンを持つ
- ユーザーは複数のコースを受講できる
- ユーザーはコースにレビューを投稿できる

---

### 演習2: API設計

**課題**: 「ブログサービス」のREST API エンドポイントを設計せよ。

**機能**:
- 記事のCRUD
- コメントのCRUD
- タグ機能
- いいね機能

**要件**:
- RESTful原則に従う
- HTTPステータスコードを適切に使用
- OpenAPI形式でドキュメント化

---

### 演習3: ワイヤーフレーム作成

**課題**: 「ECサイト」の主要画面のワイヤーフレームを作成せよ。

**画面**:
- 商品一覧
- 商品詳細
- カート
- 決済画面

---

## まとめ

このドキュメントでは、基本設計の実践的な手法を学習しました。

### 重要ポイント

1. **システムアーキテクチャ**
   - 3層アーキテクチャ、マイクロサービス、サーバーレス
   - 技術スタックの選定理由を明確に

2. **データベース設計**
   - ER図でリレーションを可視化
   - 正規化とインデックス設計
   - Prismaでスキーマ管理

3. **API設計**
   - RESTful原則に従う
   - OpenAPIでドキュメント化
   - 適切なステータスコード

4. **UI/UX設計**
   - ワイヤーフレームで構造を明確に
   - デザインシステムで一貫性
   - 画面遷移図でフロー可視化

### 次のステップ

- [詳細設計](../d_detailed_design/README.md) - 実装レベルの詳細設計
- [セキュリティ設計](../f_security_and_compliance/README.md) - セキュリティ対策
- [実装サンプル](../../02_implementation/examples/README.md) - コード実装

---

**学習時間**: 約8時間
**最終更新**: 2025-10-13
