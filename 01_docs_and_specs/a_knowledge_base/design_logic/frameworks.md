# デザインフレームワーク

デザイン思考、デザインシステム、主要なUI/UXフレームワークを学び、体系的な設計アプローチを習得します。

---

## 📑 目次

1. [デザイン思考（Design Thinking）](#デザイン思考design-thinking)
2. [Atomic Design](#atomic-design)
3. [主要なデザインシステム](#主要なデザインシステム)
4. [デザイントークン](#デザイントークン)
5. [ワイヤーフレームとプロトタイピング](#ワイヤーフレームとプロトタイピング)
6. [実践プロジェクト](#実践プロジェクト)

---

## デザイン思考（Design Thinking）

ユーザー中心のイノベーションプロセス。

### 5つのステップ

```
共感 → 問題定義 → アイデア創出 → プロトタイプ → テスト
  ↓        ↓          ↓            ↓          ↓
Empathize Define    Ideate      Prototype    Test
```

---

### 1. 共感（Empathize）

**目的:** ユーザーを深く理解する

**手法:**
- **ユーザーインタビュー** - 1対1の対話
- **観察調査** - 実際の使用場面を観察
- **カスタマージャーニーマップ** - 体験の可視化
- **ペルソナ作成** - 典型的ユーザー像

**実践例:**

```markdown
## ユーザーインタビュー

### 質問リスト
1. 現在どのように〇〇を管理していますか？
2. その方法で困っていることは何ですか？
3. 理想的にはどうなってほしいですか？
4. 一日のうちいつ、どこで使いますか？
5. 他の人と共有する必要がありますか？

### インサイト（気づき）
- ユーザーは外出先でも使いたい → モバイル対応必須
- 複数人での共同作業が多い → リアルタイム同期必要
- 専門用語が分からない → 分かりやすい言葉で説明
```

**ツール:**
- Miro（オンラインホワイトボード）
- FigJam（コラボレーションツール）
- Notion（ドキュメント管理）

---

### 2. 問題定義（Define）

**目的:** 解決すべき本質的な問題を明確化

**POV（Point of View）ステートメント:**

```
[ユーザー] は [ニーズ] を必要としている。
なぜなら [インサイト] だから。

例:
「忙しい会社員」は「簡単に健康管理できるアプリ」を必要としている。
なぜなら「時間がなく、複雑な入力は続かない」から。
```

**HMW（How Might We）質問:**

```
どうすれば〇〇できるだろうか？

例:
- どうすれば入力時間を30秒以内にできるだろうか？
- どうすれば楽しく継続できるだろうか？
- どうすれば自動でデータを取得できるだろうか？
```

---

### 3. アイデア創出（Ideate）

**目的:** できるだけ多くのアイデアを出す

**ブレインストーミングのルール:**
1. 批判禁止 - どんなアイデアもOK
2. 質より量 - とにかくたくさん出す
3. 自由奔放 - 突飛なアイデア歓迎
4. 便乗OK - 他人のアイデアを発展させる

**手法:**
- **ブレインライティング** - 書いて回す
- **マインドマップ** - 連想でアイデアを広げる
- **SCAMPER法** - 既存のものを改良
  - **S**ubstitute（代用）
  - **C**ombine（結合）
  - **A**dapt（応用）
  - **M**odify（修正）
  - **P**ut to other uses（転用）
  - **E**liminate（削除）
  - **R**everse（逆転）

**実践例:**

```markdown
## アイデア発散（30分で50個）

1. 音声入力で健康データを記録
2. スマートウォッチと自動連携
3. SNS風にフレンドと共有
4. ゲーミフィケーション（ポイント制）
5. AIが自動でアドバイス
...
50. 家族全員で健康管理

## アイデア収束（投票で上位5つ）
1. スマートウォッチ連携（15票）
2. 音声入力（12票）
3. AIアドバイス（10票）
4. ゲーミフィケーション（8票）
5. 家族共有（7票）
```

---

### 4. プロトタイプ（Prototype）

**目的:** アイデアを素早く形にする

**プロトタイプの種類:**

**Low-Fidelity（低忠実度）:**
- **紙プロトタイプ** - 手書きの画面遷移
- **ワイヤーフレーム** - 白黒の骨組み
- **スケッチ** - ラフな絵

**Mid-Fidelity（中忠実度）:**
- **クリッカブルプロトタイプ** - クリックで画面遷移
- **グレースケールモック** - 色なし、レイアウト確認

**High-Fidelity（高忠実度）:**
- **ビジュアルデザイン** - 実際のデザイン
- **インタラクティブプロトタイプ** - アニメーション付き
- **コードプロトタイプ** - 実際に動くもの

**ツール:**
- **Figma** - 最も人気（無料プランあり）
- **Adobe XD** - Adobeユーザー向け
- **Sketch** - Mac専用
- **InVision** - プロトタイピング専用
- **Framer** - コード連携が強い

---

### 5. テスト（Test）

**目的:** ユーザーからフィードバックを得る

**ユーザビリティテスト:**

```markdown
## テスト計画

### 参加者
- 5-8人（85%の問題を発見可能）
- ターゲットユーザーに近い人

### タスク（例）
1. アプリを開いて、今日の体重を記録してください
2. 先週のデータを確認してください
3. 友達を招待してください

### 観察ポイント
- 迷っている箇所はどこか？
- エラーが発生しているか？
- タスク完了にかかった時間は？
- ユーザーの感想は？

### 結果（例）
- 課題1: 体重入力ボタンが分かりにくい（3/5人が迷った）
- 課題2: データ表示が小さすぎる（2/5人が見づらいと指摘）
- 改善: ボタンを大きく、グラフを拡大
```

**テスト手法:**
- **モデレート型** - 司会者が質問しながら
- **アンモデレート型** - ユーザーが自分で操作
- **A/Bテスト** - 2つのバージョンを比較
- **5秒テスト** - 5秒見せて印象を聞く

---

## Atomic Design

**by Brad Frost**

コンポーネントを5つの階層に分けて設計。

### 階層構造

```
Atoms（原子）
  ↓
Molecules（分子）
  ↓
Organisms（有機体）
  ↓
Templates（テンプレート）
  ↓
Pages（ページ）
```

---

### 1. Atoms（原子）- 最小単位

**例:**
- ボタン
- 入力フィールド
- ラベル
- アイコン
- 色
- タイポグラフィ

**実装例:**

```jsx
// atoms/Button.jsx
export const Button = ({ children, variant = 'primary', ...props }) => {
  return (
    <button className={`btn btn-${variant}`} {...props}>
      {children}
    </button>
  );
};

// atoms/Input.jsx
export const Input = ({ label, error, ...props }) => {
  return (
    <div className="input-wrapper">
      {label && <label>{label}</label>}
      <input {...props} />
      {error && <span className="error">{error}</span>}
    </div>
  );
};
```

---

### 2. Molecules（分子）- Atomsの組み合わせ

**例:**
- 検索フォーム（入力フィールド + ボタン）
- ラベル付き入力（ラベル + 入力フィールド）
- ソーシャルシェアボタン群

**実装例:**

```jsx
// molecules/SearchForm.jsx
import { Input } from '../atoms/Input';
import { Button } from '../atoms/Button';

export const SearchForm = ({ onSearch }) => {
  const [query, setQuery] = useState('');

  return (
    <form onSubmit={(e) => { e.preventDefault(); onSearch(query); }}>
      <Input
        value={query}
        onChange={(e) => setQuery(e.target.value)}
        placeholder="検索..."
      />
      <Button type="submit">検索</Button>
    </form>
  );
};
```

---

### 3. Organisms（有機体）- 独立した機能

**例:**
- ヘッダー（ロゴ + ナビゲーション + 検索 + ユーザーメニュー）
- カード（画像 + タイトル + 説明 + ボタン）
- フッター

**実装例:**

```jsx
// organisms/Header.jsx
import { Logo } from '../atoms/Logo';
import { Navigation } from '../molecules/Navigation';
import { SearchForm } from '../molecules/SearchForm';
import { UserMenu } from '../molecules/UserMenu';

export const Header = () => {
  return (
    <header className="header">
      <Logo />
      <Navigation />
      <SearchForm onSearch={handleSearch} />
      <UserMenu />
    </header>
  );
};
```

---

### 4. Templates（テンプレート）- ページ構造

**例:**
- ブログレイアウト（ヘッダー + サイドバー + メインコンテンツ + フッター）
- ダッシュボードレイアウト

**実装例:**

```jsx
// templates/BlogTemplate.jsx
import { Header } from '../organisms/Header';
import { Sidebar } from '../organisms/Sidebar';
import { Footer } from '../organisms/Footer';

export const BlogTemplate = ({ children }) => {
  return (
    <div className="blog-layout">
      <Header />
      <div className="content-wrapper">
        <Sidebar />
        <main>{children}</main>
      </div>
      <Footer />
    </div>
  );
};
```

---

### 5. Pages（ページ）- 実際のコンテンツ

**例:**
- ホームページ
- 商品詳細ページ
- ユーザープロフィールページ

**実装例:**

```jsx
// pages/HomePage.jsx
import { BlogTemplate } from '../templates/BlogTemplate';
import { Hero } from '../organisms/Hero';
import { ArticleList } from '../organisms/ArticleList';

export const HomePage = () => {
  return (
    <BlogTemplate>
      <Hero title="Welcome to My Blog" />
      <ArticleList articles={latestArticles} />
    </BlogTemplate>
  );
};
```

---

### Atomic Designのメリット

✅ **再利用性** - コンポーネントを使い回せる
✅ **一貫性** - デザインが統一される
✅ **保守性** - 変更が楽
✅ **スケーラビリティ** - 大規模プロジェクトに対応
✅ **チーム開発** - 分業しやすい

---

## 主要なデザインシステム

### 1. Material Design（Google）

**特徴:**
- フラットデザイン + シャドウ（奥行き）
- アニメーション重視
- グリッドシステム

**コンセプト:**
- 物理的な紙とインクのメタファー
- 大胆なグラフィックとタイポグラフィ
- 意図的なモーション

**カラー:**
- プライマリカラー
- セカンダリカラー
- サーフェスカラー

**コンポーネント:**
- App Bar
- Bottom Navigation
- Card
- Floating Action Button (FAB)
- Snackbar

**実装:**
- [MUI (Material-UI)](https://mui.com/) - React
- [Vuetify](https://vuetifyjs.com/) - Vue
- [Angular Material](https://material.angular.io/) - Angular

---

### 2. Human Interface Guidelines（Apple）

**特徴:**
- ミニマリスト
- タイポグラフィ重視
- ネイティブな操作感

**原則:**
- **Clarity（明快さ）** - 意図が明確
- **Deference（従属性）** - コンテンツが主役
- **Depth（奥行き）** - 階層を表現

**デザイン要素:**
- **SF Pro（システムフォント）**
- **スペーシング** - 8ptグリッド
- **カラー** - システムカラー使用推奨

**実装:**
- SwiftUI（iOS/macOS）
- UIKit（iOS）

**Webでの再現:**
```css
/* iOS風デザイン */
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  font-size: 17px;
  line-height: 1.47;
}

.button-ios {
  background: #007AFF;
  color: white;
  border-radius: 10px;
  padding: 12px 24px;
  font-weight: 600;
}
```

---

### 3. Fluent Design System（Microsoft）

**特徴:**
- 透明度（Acrylic Material）
- 光と影
- 滑らかなアニメーション

**5つの原則:**
- **Light（光）** - 注意を引く
- **Depth（奥行き）** - レイヤー構造
- **Motion（動き）** - 関係性を示す
- **Material（素材）** - 物理的質感
- **Scale（スケール）** - デバイス適応

**実装:**
- [Fluent UI](https://developer.microsoft.com/en-us/fluentui) - React

---

## デザイントークン

デザインの変数を定義。

### トークンの種類

```css
/* カラートークン */
:root {
  --color-primary: #007bff;
  --color-secondary: #6c757d;
  --color-success: #28a745;
  --color-danger: #dc3545;
  --color-warning: #ffc107;
  --color-info: #17a2b8;

  --color-gray-50: #f8f9fa;
  --color-gray-100: #e9ecef;
  --color-gray-200: #dee2e6;
  /* ... */
  --color-gray-900: #212529;

  /* スペーシングトークン */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;
  --spacing-2xl: 48px;

  /* タイポグラフィトークン */
  --font-family-base: 'Inter', sans-serif;
  --font-family-heading: 'Poppins', sans-serif;
  --font-family-mono: 'Fira Code', monospace;

  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-md: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 20px;
  --font-size-2xl: 24px;
  --font-size-3xl: 30px;
  --font-size-4xl: 36px;

  --font-weight-light: 300;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;

  --line-height-tight: 1.25;
  --line-height-normal: 1.5;
  --line-height-relaxed: 1.75;

  /* ボーダーラディウストークン */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
  --radius-full: 9999px;

  /* シャドウトークン */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.15);

  /* アニメーショントークン */
  --transition-fast: 150ms ease;
  --transition-normal: 300ms ease;
  --transition-slow: 500ms ease;

  /* Z-indexトークン */
  --z-dropdown: 1000;
  --z-sticky: 1020;
  --z-fixed: 1030;
  --z-modal-backdrop: 1040;
  --z-modal: 1050;
  --z-popover: 1060;
  --z-tooltip: 1070;
}
```

### 使用例

```css
.button {
  background: var(--color-primary);
  color: white;
  padding: var(--spacing-md) var(--spacing-lg);
  font-size: var(--font-size-md);
  font-weight: var(--font-weight-semibold);
  border-radius: var(--radius-md);
  box-shadow: var(--shadow-md);
  transition: all var(--transition-normal);
}

.button:hover {
  box-shadow: var(--shadow-lg);
  transform: translateY(-2px);
}
```

---

## ワイヤーフレームとプロトタイピング

### Figmaの使い方

**基本操作:**
```
F - フレーム作成
R - 長方形
T - テキスト
Cmd/Ctrl + D - 複製
Cmd/Ctrl + G - グループ化
Shift + A - オートレイアウト
```

**プロトタイピング:**
1. フレーム間を接続
2. インタラクション設定（クリック、ホバー等）
3. アニメーション設定（Instant, Dissolve, Slide等）
4. プレゼンテーションモードで確認

---

## 実践プロジェクト

### プロジェクト: ToDoアプリのデザイン

**ステップ1: デザイン思考**
```markdown
## 共感
- ユーザー: 忙しいビジネスパーソン
- 課題: タスクが多すぎて管理できない

## 問題定義
「忙しいビジネスパーソン」は「シンプルで素早く使えるToDoアプリ」を必要としている。
なぜなら「複雑なアプリは使わなくなる」から。

## アイデア
- 音声入力
- 優先度の自動判定
- カレンダー連携
```

**ステップ2: Atomic Design**
```jsx
// Atoms
<Input />
<Button />
<Checkbox />

// Molecules
<TaskItem checkbox={<Checkbox />} text="タスク" button={<Button />} />

// Organisms
<TaskList tasks={[...]} />

// Template
<AppLayout>
  <Header />
  <TaskList />
  <Footer />
</AppLayout>
```

---

**次のステップ:** [コンポーネントライブラリ設計](./component_library.md)を学ぶ

**最終更新**: 2025-10-13
