# 詳細設計ガイド

**目的**: 基本設計をもとに、実装レベルの詳細な設計を行い、開発者が迷わずコーディングできる状態にする

**学習時間**: 約10時間
**前提知識**: 基本設計、オブジェクト指向プログラミング、デザインパターン
**関連ドキュメント**:
- [基本設計](../c_basic_design/README.md)
- [要件定義](../b_requirements/README.md)
- [実装サンプル](../../02_implementation/examples/README.md)

---

## 目次

1. [詳細設計とは](#1-詳細設計とは)
2. [クラス設計](#2-クラス設計)
3. [シーケンス図](#3-シーケンス図)
4. [状態遷移図](#4-状態遷移図)
5. [処理フロー設計](#5-処理フロー設計)
6. [エラーハンドリング設計](#6-エラーハンドリング設計)
7. [実践演習](#7-実践演習)

---

## 1. 詳細設計とは

### 1.1 詳細設計の目的

詳細設計は、**「どのように実装するのか」** を具体的に定義するプロセスです。

**主な目的**:
- 開発者が実装に迷わない詳細度
- クラス・関数・メソッドの役割を明確化
- 処理フローとロジックの可視化
- エラーハンドリング戦略の定義
- コードレビューの基準

### 1.2 基本設計 vs 詳細設計（再確認）

| 観点 | 基本設計 | 詳細設計 |
|------|---------|---------|
| **粒度** | システム全体の構造 | クラス・関数レベル |
| **対象** | アーキテクチャ、DB、API | クラス図、シーケンス図、アルゴリズム |
| **成果物** | システム構成図、ER図 | クラス図、シーケンス図、疑似コード |
| **読者** | PM、アーキテクト | 開発者 |
| **例** | 「3層アーキテクチャ採用」 | 「UserService.createUser()の処理ロジック」 |

### 1.3 詳細設計のプロセス

```
┌────────────────────────────────────────────────────────────┐
│ Step 1: クラス設計                                          │
│ クラス図、責務の定義、SOLID原則の適用                       │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 2: シーケンス図作成                                    │
│ オブジェクト間のメッセージフロー                            │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 3: 状態遷移図作成                                      │
│ オブジェクトの状態変化                                      │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 4: 処理フロー設計                                      │
│ アルゴリズム、分岐・ループの詳細化                          │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 5: エラーハンドリング設計                              │
│ 例外処理、バリデーション、リトライロジック                  │
└────────────┬───────────────────────────────────────────────┘
             │
             ▼
┌────────────────────────────────────────────────────────────┐
│ Step 6: パフォーマンス最適化設計                            │
│ キャッシング、インデックス、N+1問題対策                     │
└────────────────────────────────────────────────────────────┘
```

---

## 2. クラス設計

### 2.1 SOLID原則

優れたクラス設計は **SOLID原則** に従います。

#### S - Single Responsibility Principle（単一責任の原則）

**定義**: 1つのクラスは1つの責務のみを持つ

**❌ 悪い例**:
```typescript
// UserクラスがDB操作とメール送信の両方を持つ（責務が多い）
class User {
  id: number;
  email: string;
  name: string;

  // ユーザー情報の管理
  updateProfile(name: string) {
    this.name = name;
  }

  // DB操作（別の責務）
  save() {
    // DBにユーザーを保存
  }

  // メール送信（さらに別の責務）
  sendWelcomeEmail() {
    // メール送信ロジック
  }
}
```

**✅ 良い例**:
```typescript
// 責務を分離
class User {
  id: number;
  email: string;
  name: string;

  updateProfile(name: string) {
    this.name = name;
  }
}

// DB操作はRepositoryクラスの責務
class UserRepository {
  async save(user: User): Promise<User> {
    // DBにユーザーを保存
    return await prisma.user.create({ data: user });
  }

  async findById(id: number): Promise<User | null> {
    return await prisma.user.findUnique({ where: { id } });
  }
}

// メール送信はMailServiceの責務
class MailService {
  async sendWelcomeEmail(user: User): Promise<void> {
    // メール送信ロジック
    await emailClient.send({
      to: user.email,
      subject: 'Welcome!',
      body: `Hello ${user.name}!`,
    });
  }
}
```

---

#### O - Open/Closed Principle（オープン・クローズドの原則）

**定義**: 拡張に対して開いていて、修正に対して閉じている

**❌ 悪い例**:
```typescript
// 新しい通知方法を追加するたびにクラスを修正する必要がある
class NotificationService {
  send(type: string, message: string) {
    if (type === 'email') {
      // メール送信
    } else if (type === 'sms') {
      // SMS送信
    } else if (type === 'push') {
      // プッシュ通知
    }
    // 新しい通知方法を追加するたびにif文が増える
  }
}
```

**✅ 良い例**:
```typescript
// 抽象インターフェース
interface NotificationChannel {
  send(message: string): Promise<void>;
}

// 各実装
class EmailChannel implements NotificationChannel {
  async send(message: string): Promise<void> {
    // メール送信
  }
}

class SmsChannel implements NotificationChannel {
  async send(message: string): Promise<void> {
    // SMS送信
  }
}

class PushChannel implements NotificationChannel {
  async send(message: string): Promise<void> {
    // プッシュ通知
  }
}

// 通知サービスは修正不要、新しいChannelを追加するだけ
class NotificationService {
  constructor(private channel: NotificationChannel) {}

  async notify(message: string): Promise<void> {
    await this.channel.send(message);
  }
}

// 使用例
const emailService = new NotificationService(new EmailChannel());
await emailService.notify('Hello via Email!');

const smsService = new NotificationService(new SmsChannel());
await smsService.notify('Hello via SMS!');
```

---

#### L - Liskov Substitution Principle（リスコフの置換原則）

**定義**: 派生クラスは基底クラスと置き換え可能である

**❌ 悪い例**:
```typescript
class Bird {
  fly() {
    console.log('Flying...');
  }
}

// ペンギンは飛べないので、flyメソッドで例外を投げる
class Penguin extends Bird {
  fly() {
    throw new Error('Penguins cannot fly!');
  }
}

// Bird型を期待する関数がPenguinで壊れる
function makeBirdFly(bird: Bird) {
  bird.fly();  // Penguinの場合エラー
}
```

**✅ 良い例**:
```typescript
// 飛べる鳥と飛べない鳥を分離
interface Bird {
  eat(): void;
}

interface FlyableBird extends Bird {
  fly(): void;
}

class Sparrow implements FlyableBird {
  eat() {
    console.log('Eating...');
  }

  fly() {
    console.log('Flying...');
  }
}

class Penguin implements Bird {
  eat() {
    console.log('Eating...');
  }

  swim() {
    console.log('Swimming...');
  }
}

// 飛べる鳥のみを受け取る
function makeBirdFly(bird: FlyableBird) {
  bird.fly();  // 常に安全
}
```

---

#### I - Interface Segregation Principle（インターフェース分離の原則）

**定義**: クライアントは使わないメソッドに依存すべきでない

**❌ 悪い例**:
```typescript
// 巨大なインターフェース
interface Worker {
  work(): void;
  eat(): void;
  sleep(): void;
}

// ロボットはeatとsleepを使わない
class Robot implements Worker {
  work() {
    console.log('Working...');
  }

  eat() {
    throw new Error('Robots do not eat!');
  }

  sleep() {
    throw new Error('Robots do not sleep!');
  }
}
```

**✅ 良い例**:
```typescript
// インターフェースを分離
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

interface Sleepable {
  sleep(): void;
}

// 必要なインターフェースのみ実装
class Human implements Workable, Eatable, Sleepable {
  work() {
    console.log('Working...');
  }

  eat() {
    console.log('Eating...');
  }

  sleep() {
    console.log('Sleeping...');
  }
}

class Robot implements Workable {
  work() {
    console.log('Working...');
  }
}
```

---

#### D - Dependency Inversion Principle（依存性逆転の原則）

**定義**: 高レベルモジュールは低レベルモジュールに依存すべきでない。両方とも抽象に依存すべき。

**❌ 悪い例**:
```typescript
// UserServiceが具体的なMySQLRepositoryに依存
class MySQLUserRepository {
  save(user: User) {
    // MySQL固有の処理
  }
}

class UserService {
  private repository = new MySQLUserRepository();  // 具象クラスに直接依存

  createUser(user: User) {
    this.repository.save(user);
  }
}
```

**✅ 良い例**:
```typescript
// 抽象インターフェース
interface UserRepository {
  save(user: User): Promise<User>;
  findById(id: number): Promise<User | null>;
}

// 具体実装
class MySQLUserRepository implements UserRepository {
  async save(user: User): Promise<User> {
    // MySQL固有の処理
    return user;
  }

  async findById(id: number): Promise<User | null> {
    return null;
  }
}

class PostgresUserRepository implements UserRepository {
  async save(user: User): Promise<User> {
    // PostgreSQL固有の処理
    return user;
  }

  async findById(id: number): Promise<User | null> {
    return null;
  }
}

// UserServiceは抽象に依存（依存性注入）
class UserService {
  constructor(private repository: UserRepository) {}  // インターフェースに依存

  async createUser(user: User): Promise<User> {
    return await this.repository.save(user);
  }
}

// 使用例（DI）
const mysqlRepo = new MySQLUserRepository();
const userService = new UserService(mysqlRepo);

// リポジトリを簡単に切り替え可能
const postgresRepo = new PostgresUserRepository();
const userService2 = new UserService(postgresRepo);
```

---

### 2.2 クラス図（タスク管理SaaSの例）

#### ドメインモデルのクラス図

```
┌─────────────────────────┐
│       User              │
├─────────────────────────┤
│ - id: number            │
│ - email: string         │
│ - name: string          │
│ - passwordHash: string  │
├─────────────────────────┤
│ + updateProfile()       │
│ + verifyPassword()      │
└───────────┬─────────────┘
            │
            │ 1:N
            │
            ▼
┌─────────────────────────┐
│     TeamMember          │
├─────────────────────────┤
│ - id: number            │
│ - teamId: number        │
│ - userId: number        │
│ - role: TeamRole        │
├─────────────────────────┤
│ + hasPermission()       │
└───────────┬─────────────┘
            │
            │ N:1
            │
            ▼
┌─────────────────────────┐          ┌─────────────────────────┐
│       Team              │   1:N    │      Project            │
├─────────────────────────┤◄─────────├─────────────────────────┤
│ - id: number            │          │ - id: number            │
│ - name: string          │          │ - teamId: number        │
│ - description: string   │          │ - name: string          │
├─────────────────────────┤          ├─────────────────────────┤
│ + addMember()           │          │ + addTask()             │
│ + removeMember()        │          │ + getTaskCount()        │
└─────────────────────────┘          └───────────┬─────────────┘
                                                  │
                                                  │ 1:N
                                                  │
                                                  ▼
                                     ┌─────────────────────────┐
                                     │        Task             │
                                     ├─────────────────────────┤
                                     │ - id: number            │
                                     │ - projectId: number     │
                                     │ - title: string         │
                                     │ - status: TaskStatus    │
                                     │ - priority: Priority    │
                                     ├─────────────────────────┤
                                     │ + complete()            │
                                     │ + assign()              │
                                     │ + isOverdue()           │
                                     └─────────────────────────┘
```

#### サービス層のクラス図

```
┌─────────────────────────┐
│   <<interface>>         │
│   UserRepository        │
├─────────────────────────┤
│ + save()                │
│ + findById()            │
│ + findByEmail()         │
└───────────┬─────────────┘
            │
            │ implements
            │
            ▼
┌─────────────────────────┐
│ PrismaUserRepository    │
├─────────────────────────┤
│ - prisma: PrismaClient  │
├─────────────────────────┤
│ + save()                │
│ + findById()            │
│ + findByEmail()         │
└─────────────────────────┘

┌─────────────────────────┐
│     UserService         │
├─────────────────────────┤
│ - repository            │
│ - mailService           │
│ - hashService           │
├─────────────────────────┤
│ + createUser()          │
│ + updateUser()          │
│ + deleteUser()          │
│ + authenticate()        │
└─────────────────────────┘
            │
            │ uses
            │
            ▼
┌─────────────────────────┐
│     MailService         │
├─────────────────────────┤
│ - emailClient           │
├─────────────────────────┤
│ + sendWelcomeEmail()    │
│ + sendPasswordReset()   │
└─────────────────────────┘
```

### 2.3 クラス実装例

#### ドメインモデル（User）

```typescript
// domain/user.entity.ts
import bcrypt from 'bcrypt';

export class User {
  constructor(
    public readonly id: number,
    public email: string,
    public name: string,
    private passwordHash: string,
    public readonly createdAt: Date,
    public updatedAt: Date
  ) {}

  // パスワード検証
  async verifyPassword(password: string): Promise<boolean> {
    return await bcrypt.compare(password, this.passwordHash);
  }

  // プロフィール更新
  updateProfile(name: string): void {
    this.name = name;
    this.updatedAt = new Date();
  }

  // パスワード更新
  async updatePassword(newPassword: string): Promise<void> {
    const saltRounds = 10;
    this.passwordHash = await bcrypt.hash(newPassword, saltRounds);
    this.updatedAt = new Date();
  }

  // ファクトリーメソッド
  static async create(
    email: string,
    name: string,
    password: string
  ): Promise<Omit<User, 'id' | 'createdAt' | 'updatedAt'>> {
    const saltRounds = 10;
    const passwordHash = await bcrypt.hash(password, saltRounds);

    return {
      email,
      name,
      passwordHash,
    } as any;
  }

  // DTO変換
  toDTO(): UserDTO {
    return {
      id: this.id,
      email: this.email,
      name: this.name,
      createdAt: this.createdAt,
      updatedAt: this.updatedAt,
    };
  }
}

export interface UserDTO {
  id: number;
  email: string;
  name: string;
  createdAt: Date;
  updatedAt: Date;
}
```

#### リポジトリパターン

```typescript
// repositories/user.repository.ts
import { PrismaClient } from '@prisma/client';
import { User } from '../domain/user.entity';

export interface UserRepository {
  save(user: Partial<User>): Promise<User>;
  findById(id: number): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
  update(id: number, data: Partial<User>): Promise<User>;
  delete(id: number): Promise<void>;
}

export class PrismaUserRepository implements UserRepository {
  constructor(private prisma: PrismaClient) {}

  async save(userData: Partial<User>): Promise<User> {
    const created = await this.prisma.user.create({
      data: {
        email: userData.email!,
        name: userData.name!,
        passwordHash: (userData as any).passwordHash,
      },
    });

    return new User(
      created.id,
      created.email,
      created.name,
      created.passwordHash,
      created.createdAt,
      created.updatedAt
    );
  }

  async findById(id: number): Promise<User | null> {
    const user = await this.prisma.user.findUnique({
      where: { id },
    });

    if (!user) return null;

    return new User(
      user.id,
      user.email,
      user.name,
      user.passwordHash,
      user.createdAt,
      user.updatedAt
    );
  }

  async findByEmail(email: string): Promise<User | null> {
    const user = await this.prisma.user.findUnique({
      where: { email },
    });

    if (!user) return null;

    return new User(
      user.id,
      user.email,
      user.name,
      user.passwordHash,
      user.createdAt,
      user.updatedAt
    );
  }

  async update(id: number, data: Partial<User>): Promise<User> {
    const updated = await this.prisma.user.update({
      where: { id },
      data: {
        name: data.name,
        email: data.email,
      },
    });

    return new User(
      updated.id,
      updated.email,
      updated.name,
      updated.passwordHash,
      updated.createdAt,
      updated.updatedAt
    );
  }

  async delete(id: number): Promise<void> {
    await this.prisma.user.delete({
      where: { id },
    });
  }
}
```

#### サービス層

```typescript
// services/user.service.ts
import { UserRepository } from '../repositories/user.repository';
import { MailService } from './mail.service';
import { User, UserDTO } from '../domain/user.entity';

export class UserService {
  constructor(
    private userRepository: UserRepository,
    private mailService: MailService
  ) {}

  async createUser(
    email: string,
    name: string,
    password: string
  ): Promise<UserDTO> {
    // 1. メールアドレスの重複チェック
    const existingUser = await this.userRepository.findByEmail(email);
    if (existingUser) {
      throw new Error('Email already exists');
    }

    // 2. ユーザー作成
    const userData = await User.create(email, name, password);
    const user = await this.userRepository.save(userData);

    // 3. ウェルカムメール送信
    await this.mailService.sendWelcomeEmail(user);

    // 4. DTOに変換して返却
    return user.toDTO();
  }

  async authenticate(email: string, password: string): Promise<UserDTO> {
    // 1. ユーザー取得
    const user = await this.userRepository.findByEmail(email);
    if (!user) {
      throw new Error('Invalid credentials');
    }

    // 2. パスワード検証
    const isValid = await user.verifyPassword(password);
    if (!isValid) {
      throw new Error('Invalid credentials');
    }

    // 3. DTOに変換して返却
    return user.toDTO();
  }

  async updateProfile(id: number, name: string): Promise<UserDTO> {
    // 1. ユーザー取得
    const user = await this.userRepository.findById(id);
    if (!user) {
      throw new Error('User not found');
    }

    // 2. プロフィール更新
    user.updateProfile(name);

    // 3. 保存
    const updated = await this.userRepository.update(id, user);

    return updated.toDTO();
  }

  async deleteUser(id: number): Promise<void> {
    const user = await this.userRepository.findById(id);
    if (!user) {
      throw new Error('User not found');
    }

    await this.userRepository.delete(id);
  }
}
```

---

## 3. シーケンス図

### 3.1 シーケンス図とは

**定義**: オブジェクト間のメッセージのやり取りを時系列で表現

**用途**:
- API呼び出しのフロー
- 複数システム間の連携
- エラーハンドリングの流れ

### 3.2 ユーザー登録のシーケンス図

```
クライアント    Controller    UserService    UserRepository    MailService    DB
    │               │              │                │                 │         │
    ├──POST /register──►            │                │                 │         │
    │               │              │                │                 │         │
    │               ├──createUser()──►               │                 │         │
    │               │              │                │                 │         │
    │               │              ├──findByEmail()──►                 │         │
    │               │              │                │                 │         │
    │               │              │                ├──SELECT───────────►       │
    │               │              │                │                 │         │
    │               │              │                ◄────null─────────┤         │
    │               │              │                │                 │         │
    │               │              ├──save()────────►                 │         │
    │               │              │                │                 │         │
    │               │              │                ├──INSERT─────────►         │
    │               │              │                │                 │         │
    │               │              │                ◄────user─────────┤         │
    │               │              │                │                 │         │
    │               │              ├──sendWelcomeEmail()───────────────►         │
    │               │              │                │                 │         │
    │               │              │                │         ┌───────┴─────┐   │
    │               │              │                │         │Send Email   │   │
    │               │              │                │         └───────┬─────┘   │
    │               │              │                │                 │         │
    │               │              ◄────────────────────────────OK────┤         │
    │               │              │                │                 │         │
    │               ◄──userDTO─────┤                │                 │         │
    │               │              │                │                 │         │
    ◄───201 Created────┤           │                │                 │         │
    │               │              │                │                 │         │
```

### 3.3 タスク作成のシーケンス図（エラーケース含む）

```
Client      Controller    TaskService    ProjectRepo    TaskRepo    NotificationService
  │             │              │              │            │                │
  ├──POST /tasks──►            │              │            │                │
  │             │              │              │            │                │
  │             ├──createTask()──►            │            │                │
  │             │              │              │            │                │
  │             │              ├──findById()──►            │                │
  │             │              │              │            │                │
  │             │              │              ◄───null────┤                │
  │             │              │              │            │                │
  │             │              ├──X Project not found      │                │
  │             │              │              │            │                │
  │             ◄──404 Error───┤              │            │                │
  │             │              │              │            │                │
  ◄───404─────────┤            │              │            │                │
  │             │              │              │            │                │
  │             │              │              │            │                │
  │  [正常ケース]              │              │            │                │
  │             │              │              │            │                │
  ├──POST /tasks──►            │              │            │                │
  │             │              │              │            │                │
  │             ├──createTask()──►            │            │                │
  │             │              │              │            │                │
  │             │              ├──findById()──►            │                │
  │             │              │              │            │                │
  │             │              │              ◄──project──┤                │
  │             │              │              │            │                │
  │             │              ├──save()──────────────────►                │
  │             │              │              │            │                │
  │             │              │              │            ◄───task────────┤
  │             │              │              │            │                │
  │             │              ├──notifyAssignee()─────────────────────────►
  │             │              │              │            │                │
  │             ◄──taskDTO─────┤              │            │                │
  │             │              │              │            │                │
  ◄───201─────────┤            │              │            │                │
  │             │              │              │            │                │
```

---

## 4. 状態遷移図

### 4.1 タスクの状態遷移図

```
                    [作成]
                      │
                      ▼
              ┌──────────────┐
              │     TODO     │◄─────────────┐
              └──────┬───────┘              │
                     │                      │
                  [開始]                 [差戻し]
                     │                      │
                     ▼                      │
              ┌──────────────┐              │
              │ IN_PROGRESS  │──────────────┘
              └──────┬───────┘
                     │
                  [完了]
                     │
                     ▼
              ┌──────────────┐
         ┌────│     DONE     │
         │    └──────┬───────┘
         │           │
    [再開]        [アーカイブ]
         │           │
         │           ▼
         │    ┌──────────────┐
         └───►│   ARCHIVED   │
              └──────────────┘
```

### 4.2 状態遷移の実装

```typescript
// domain/task-status.ts
export enum TaskStatus {
  TODO = 'todo',
  IN_PROGRESS = 'in_progress',
  DONE = 'done',
  ARCHIVED = 'archived',
}

// 許可された状態遷移
const ALLOWED_TRANSITIONS: Record<TaskStatus, TaskStatus[]> = {
  [TaskStatus.TODO]: [TaskStatus.IN_PROGRESS],
  [TaskStatus.IN_PROGRESS]: [TaskStatus.TODO, TaskStatus.DONE],
  [TaskStatus.DONE]: [TaskStatus.IN_PROGRESS, TaskStatus.ARCHIVED],
  [TaskStatus.ARCHIVED]: [TaskStatus.TODO],
};

export class Task {
  constructor(
    public readonly id: number,
    public title: string,
    public status: TaskStatus,
    public assignedTo: number | null,
    public dueDate: Date | null
  ) {}

  // 状態遷移
  transitionTo(newStatus: TaskStatus): void {
    const allowedStates = ALLOWED_TRANSITIONS[this.status];

    if (!allowedStates.includes(newStatus)) {
      throw new Error(
        `Cannot transition from ${this.status} to ${newStatus}`
      );
    }

    this.status = newStatus;
  }

  // 完了
  complete(): void {
    this.transitionTo(TaskStatus.DONE);
  }

  // 開始
  start(): void {
    if (this.status !== TaskStatus.TODO) {
      throw new Error('Can only start tasks in TODO status');
    }
    this.transitionTo(TaskStatus.IN_PROGRESS);
  }

  // 再開
  reopen(): void {
    if (this.status !== TaskStatus.DONE) {
      throw new Error('Can only reopen completed tasks');
    }
    this.transitionTo(TaskStatus.IN_PROGRESS);
  }

  // アーカイブ
  archive(): void {
    if (this.status !== TaskStatus.DONE) {
      throw new Error('Can only archive completed tasks');
    }
    this.transitionTo(TaskStatus.ARCHIVED);
  }

  // 期限切れチェック
  isOverdue(): boolean {
    if (!this.dueDate) return false;
    if (this.status === TaskStatus.DONE || this.status === TaskStatus.ARCHIVED) {
      return false;
    }
    return new Date() > this.dueDate;
  }
}
```

---

## 5. 処理フロー設計

### 5.1 フローチャート（ユーザー登録処理）

```
             [開始]
                │
                ▼
        ┌──────────────┐
        │入力値検証    │
        └──────┬───────┘
               │
          ┌────▼────┐
          │ 有効？  │
          └────┬────┘
               │
         No ◄──┼──► Yes
         │     │
         ▼     │
    ┌─────────┐│
    │400 Error││
    └─────────┘│
               ▼
        ┌──────────────┐
        │メール重複    │
        │チェック      │
        └──────┬───────┘
               │
          ┌────▼────┐
          │ 存在？  │
          └────┬────┘
               │
         Yes ◄─┼──► No
         │     │
         ▼     │
    ┌─────────┐│
    │409 Error││
    └─────────┘│
               ▼
        ┌──────────────┐
        │パスワード    │
        │ハッシュ化    │
        └──────┬───────┘
               │
               ▼
        ┌──────────────┐
        │DB保存        │
        └──────┬───────┘
               │
          ┌────▼────┐
          │ 成功？  │
          └────┬────┘
               │
         No ◄──┼──► Yes
         │     │
         ▼     │
    ┌─────────┐│
    │500 Error││
    └─────────┘│
               ▼
        ┌──────────────┐
        │ウェルカム    │
        │メール送信    │
        └──────┬───────┘
               │
               ▼
        ┌──────────────┐
        │201 Created   │
        │レスポンス    │
        └──────┬───────┘
               │
               ▼
             [終了]
```

### 5.2 疑似コード

```typescript
// controllers/auth.controller.ts

async register(req: Request, res: Response) {
  try {
    // Step 1: 入力値のバリデーション
    const { email, name, password } = req.body;

    const validation = validateRegistrationInput({ email, name, password });
    if (!validation.isValid) {
      return res.status(400).json({
        error: 'Validation error',
        details: validation.errors,
      });
    }

    // Step 2: メールアドレスの重複チェック
    const existingUser = await this.userRepository.findByEmail(email);
    if (existingUser) {
      return res.status(409).json({
        error: 'Email already exists',
      });
    }

    // Step 3: パスワードのハッシュ化
    const hashedPassword = await bcrypt.hash(password, 10);

    // Step 4: ユーザーをDBに保存
    const user = await this.userRepository.save({
      email,
      name,
      passwordHash: hashedPassword,
    });

    // Step 5: ウェルカムメールを送信（非同期）
    this.mailService.sendWelcomeEmail(user).catch((err) => {
      console.error('Failed to send welcome email:', err);
      // メール送信失敗はログに記録するが、ユーザー登録は成功とする
    });

    // Step 6: JWTトークン生成
    const token = jwt.sign(
      { userId: user.id, email: user.email },
      process.env.JWT_SECRET!,
      { expiresIn: '24h' }
    );

    // Step 7: 成功レスポンス
    return res.status(201).json({
      user: user.toDTO(),
      token,
    });

  } catch (error) {
    console.error('Registration error:', error);
    return res.status(500).json({
      error: 'Internal server error',
    });
  }
}
```

### 5.3 複雑なビジネスロジックの設計

#### タスクの自動割り当てロジック

```typescript
// services/task-assignment.service.ts

export class TaskAssignmentService {
  constructor(
    private userRepository: UserRepository,
    private taskRepository: TaskRepository
  ) {}

  /**
   * チームメンバーに自動でタスクを割り当てる
   *
   * アルゴリズム:
   * 1. 担当タスク数が最も少ないメンバーを選択
   * 2. 同数の場合は、優先度の高いタスクを持っていない人を優先
   * 3. それでも同じ場合は、直近のタスク完了日が古い人を優先
   */
  async autoAssignTask(taskId: number, teamId: number): Promise<void> {
    // Step 1: チームメンバーを取得
    const members = await this.userRepository.findTeamMembers(teamId);

    if (members.length === 0) {
      throw new Error('No team members available');
    }

    // Step 2: 各メンバーの担当タスク数を取得
    const memberWorkloads = await Promise.all(
      members.map(async (member) => {
        const tasks = await this.taskRepository.findByAssignee(member.id);
        const activeTasks = tasks.filter(
          (t) => t.status !== TaskStatus.DONE && t.status !== TaskStatus.ARCHIVED
        );
        const urgentTaskCount = activeTasks.filter(
          (t) => t.priority === TaskPriority.URGENT
        ).length;
        const lastCompletedTask = await this.taskRepository.findLastCompletedTask(
          member.id
        );

        return {
          member,
          taskCount: activeTasks.length,
          urgentTaskCount,
          lastCompletedAt: lastCompletedTask?.updatedAt || new Date(0),
        };
      })
    );

    // Step 3: ソート（タスク数 → 緊急タスク数 → 最終完了日）
    memberWorkloads.sort((a, b) => {
      // タスク数で比較
      if (a.taskCount !== b.taskCount) {
        return a.taskCount - b.taskCount;
      }

      // 緊急タスク数で比較
      if (a.urgentTaskCount !== b.urgentTaskCount) {
        return a.urgentTaskCount - b.urgentTaskCount;
      }

      // 最終完了日で比較（古い方を優先）
      return a.lastCompletedAt.getTime() - b.lastCompletedAt.getTime();
    });

    // Step 4: 最適なメンバーにタスクを割り当て
    const selectedMember = memberWorkloads[0].member;
    await this.taskRepository.assignTask(taskId, selectedMember.id);

    console.log(`Task ${taskId} assigned to ${selectedMember.name}`);
  }
}
```

---

## 6. エラーハンドリング設計

### 6.1 エラークラスの階層構造

```typescript
// errors/base.error.ts

export abstract class AppError extends Error {
  constructor(
    public readonly message: string,
    public readonly statusCode: number,
    public readonly isOperational: boolean = true
  ) {
    super(message);
    Error.captureStackTrace(this, this.constructor);
  }
}

// errors/client.errors.ts

export class BadRequestError extends AppError {
  constructor(message: string = 'Bad request') {
    super(message, 400);
  }
}

export class UnauthorizedError extends AppError {
  constructor(message: string = 'Unauthorized') {
    super(message, 401);
  }
}

export class ForbiddenError extends AppError {
  constructor(message: string = 'Forbidden') {
    super(message, 403);
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string) {
    super(`${resource} not found`, 404);
  }
}

export class ConflictError extends AppError {
  constructor(message: string = 'Conflict') {
    super(message, 409);
  }
}

export class ValidationError extends AppError {
  constructor(
    message: string,
    public readonly errors: Record<string, string>
  ) {
    super(message, 422);
  }
}

// errors/server.errors.ts

export class InternalServerError extends AppError {
  constructor(message: string = 'Internal server error') {
    super(message, 500, false);  // 運用エラーではない
  }
}

export class DatabaseError extends AppError {
  constructor(message: string) {
    super(message, 500, false);
  }
}
```

### 6.2 グローバルエラーハンドラー

```typescript
// middleware/error-handler.ts

import { Request, Response, NextFunction } from 'express';
import { AppError } from '../errors/base.error';

export function errorHandler(
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  // AppErrorの場合
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      error: err.message,
      ...(err instanceof ValidationError && { details: err.errors }),
    });
  }

  // 予期しないエラー
  console.error('Unexpected error:', err);

  // Sentry等にエラーを送信
  // Sentry.captureException(err);

  return res.status(500).json({
    error: 'Internal server error',
  });
}

// 非同期エラーをキャッチするラッパー
export function asyncHandler(
  fn: (req: Request, res: Response, next: NextFunction) => Promise<any>
) {
  return (req: Request, res: Response, next: NextFunction) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
}
```

### 6.3 エラーハンドリングの実装例

```typescript
// controllers/task.controller.ts

import { asyncHandler } from '../middleware/error-handler';
import { NotFoundError, ForbiddenError, ValidationError } from '../errors';

export class TaskController {
  constructor(
    private taskService: TaskService,
    private projectRepository: ProjectRepository
  ) {}

  // タスク作成
  createTask = asyncHandler(async (req: Request, res: Response) => {
    const { projectId, title, description, assignedTo, priority, dueDate } = req.body;
    const userId = req.user!.id;

    // バリデーション
    const errors: Record<string, string> = {};

    if (!title || title.trim().length === 0) {
      errors.title = 'Title is required';
    }

    if (title && title.length > 200) {
      errors.title = 'Title must be less than 200 characters';
    }

    if (Object.keys(errors).length > 0) {
      throw new ValidationError('Validation failed', errors);
    }

    // プロジェクトの存在確認
    const project = await this.projectRepository.findById(projectId);
    if (!project) {
      throw new NotFoundError('Project');
    }

    // 権限チェック
    const hasPermission = await this.projectRepository.hasAccess(userId, projectId);
    if (!hasPermission) {
      throw new ForbiddenError('You do not have access to this project');
    }

    // タスク作成
    const task = await this.taskService.createTask({
      projectId,
      title,
      description,
      assignedTo,
      priority,
      dueDate,
      createdBy: userId,
    });

    return res.status(201).json({ task });
  });

  // タスク削除
  deleteTask = asyncHandler(async (req: Request, res: Response) => {
    const taskId = parseInt(req.params.id);
    const userId = req.user!.id;

    // タスクの存在確認
    const task = await this.taskService.findById(taskId);
    if (!task) {
      throw new NotFoundError('Task');
    }

    // 権限チェック（作成者のみ削除可能）
    if (task.createdBy !== userId) {
      throw new ForbiddenError('Only the task creator can delete this task');
    }

    await this.taskService.deleteTask(taskId);

    return res.status(204).send();
  });
}
```

### 6.4 リトライロジック

```typescript
// utils/retry.ts

export async function retry<T>(
  fn: () => Promise<T>,
  options: {
    maxAttempts?: number;
    delayMs?: number;
    backoff?: boolean;
    onRetry?: (attempt: number, error: Error) => void;
  } = {}
): Promise<T> {
  const {
    maxAttempts = 3,
    delayMs = 1000,
    backoff = true,
    onRetry,
  } = options;

  let lastError: Error;

  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error as Error;

      if (attempt === maxAttempts) {
        throw lastError;
      }

      // リトライコールバック
      if (onRetry) {
        onRetry(attempt, lastError);
      }

      // 待機（指数バックオフ）
      const delay = backoff ? delayMs * Math.pow(2, attempt - 1) : delayMs;
      await new Promise((resolve) => setTimeout(resolve, delay));
    }
  }

  throw lastError!;
}

// 使用例
async function sendEmail(to: string, subject: string, body: string) {
  return await retry(
    async () => {
      return await emailClient.send({ to, subject, body });
    },
    {
      maxAttempts: 3,
      delayMs: 1000,
      backoff: true,
      onRetry: (attempt, error) => {
        console.log(`Retry attempt ${attempt}: ${error.message}`);
      },
    }
  );
}
```

---

## 7. 実践演習

### 演習1: クラス設計

**課題**: 「オンライン書籍管理システム」のクラス図を作成せよ。

**要件**:
- Book（書籍）、Author（著者）、Publisher（出版社）、Review（レビュー）
- SOLID原則を適用
- Repository パターンを使用

---

### 演習2: シーケンス図作成

**課題**: 「商品購入フロー」のシーケンス図を作成せよ。

**アクター**:
- ユーザー
- 在庫管理システム
- 決済システム
- 通知システム

**フロー**:
1. 在庫確認
2. 決済処理
3. 在庫減算
4. 購入完了通知

---

### 演習3: 状態遷移図とコード実装

**課題**: 「注文ステータス」の状態遷移図を作成し、TypeScriptで実装せよ。

**状態**:
- PENDING（保留中）
- CONFIRMED（確認済み）
- SHIPPED（発送済み）
- DELIVERED（配達完了）
- CANCELLED（キャンセル）

**遷移ルール**:
- PENDING → CONFIRMED, CANCELLED
- CONFIRMED → SHIPPED, CANCELLED
- SHIPPED → DELIVERED
- DELIVERED → (終了状態)
- CANCELLED → (終了状態)

---

## まとめ

このドキュメントでは、詳細設計の実践的な手法を学習しました。

### 重要ポイント

1. **SOLID原則**
   - 単一責任、オープン・クローズド、リスコフ置換、インターフェース分離、依存性逆転

2. **クラス設計**
   - ドメインモデル、Repository パターン、Service 層
   - 依存性注入（DI）

3. **UML図**
   - クラス図: 構造の可視化
   - シーケンス図: フローの可視化
   - 状態遷移図: 状態管理の可視化

4. **エラーハンドリング**
   - エラークラスの階層化
   - グローバルエラーハンドラー
   - リトライロジック

### 次のステップ

- [実装サンプル](../../02_implementation/examples/README.md) - コード実装
- [セキュリティ設計](../f_security_and_compliance/README.md) - セキュリティ対策
- [テスト設計](../../tests/README.md) - ユニットテスト、統合テスト

---

**学習時間**: 約10時間
**最終更新**: 2025-10-13
