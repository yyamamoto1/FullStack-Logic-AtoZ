# デザイン原則

認知心理学、ゲシュタルト原理、ユニバーサルデザインに基づいたUI/UXデザインの基本原則を学びます。

---

## 📑 目次

1. [認知心理学ベースの法則](#認知心理学ベースの法則)
2. [ゲシュタルト原理](#ゲシュタルト原理)
3. [ユニバーサルデザイン](#ユニバーサルデザイン)
4. [デザインの基本原則](#デザインの基本原則)
5. [実践例](#実践例)
6. [チェックリスト](#チェックリスト)

---

## 認知心理学ベースの法則

### 1. ヒックの法則（Hick's Law）

**定義:**
> 選択肢の数が増えるほど、意思決定にかかる時間が増加する

**数式:**
```
T = b × log₂(n + 1)

T = 意思決定時間
b = 定数
n = 選択肢の数
```

**実例:**

❌ **悪い例: レストランメニュー**
```
メニュー項目: 150種類
→ ユーザーは圧倒され、決められない
```

✅ **良い例: レストランメニュー**
```
カテゴリ分け:
- 前菜 (10種類)
- メイン (15種類)
- デザート (8種類)
→ まずカテゴリを選び、その中から選ぶ（階層化）
```

**UI/UXでの応用:**

✅ **良い実装:**
- ナビゲーションメニューは5-7項目まで
- 複雑な選択はステップに分ける
- プログレッシブディスクロージャー（段階的開示）

❌ **避けるべき:**
- 一度に20個以上のボタンを表示
- フラットな構造で100項目のリスト

**実装例（React）:**
```jsx
// ❌ 悪い例: すべてを一度に表示
<nav>
  <button>Home</button>
  <button>About</button>
  <button>Services</button>
  <button>Product A</button>
  <button>Product B</button>
  <button>Product C</button>
  // ... 30個のボタン
</nav>

// ✅ 良い例: 階層化
<nav>
  <button>Home</button>
  <button>About</button>
  <Dropdown label="Services">
    <button>Service A</button>
    <button>Service B</button>
    <button>Service C</button>
  </Dropdown>
  <Dropdown label="Products">
    <button>Product A</button>
    <button>Product B</button>
    <button>Product C</button>
  </Dropdown>
</nav>
```

---

### 2. フィッツの法則（Fitts's Law）

**定義:**
> ターゲットへの到達時間は、距離と大きさによって決まる

**数式:**
```
T = a + b × log₂(D/W + 1)

T = 到達時間
D = ターゲットまでの距離
W = ターゲットの幅
a, b = 定数
```

**実例:**

❌ **悪い例:**
```
[×]  ← 小さな閉じるボタン（10px × 10px）
      画面の隅に配置
```

✅ **良い例:**
```
[  ×  ]  ← 大きな閉じるボタン（44px × 44px）
          タップしやすい位置に配置
```

**UI/UXでの応用:**

✅ **良い実装:**
- **重要なボタンは大きく** - CTA（Call To Action）は最低44px × 44px
- **よく使うボタンは近くに** - 関連機能を近接配置
- **画面の端・角を活用** - 無限の幅を持つ（マウスが端で止まる）

❌ **避けるべき:**
- 小さすぎるボタン（タップミス多発）
- 離れすぎた関連ボタン

**実装例（CSS）:**
```css
/* ❌ 悪い例: 小さすぎる */
.button {
  width: 20px;
  height: 20px;
  padding: 2px;
}

/* ✅ 良い例: タップしやすいサイズ */
.button {
  min-width: 44px;
  min-height: 44px;
  padding: 12px 24px;
  /* タッチターゲットの最小サイズは44px × 44px */
}

/* ✅ さらに良い例: プライマリボタンはさらに大きく */
.button-primary {
  min-width: 120px;
  min-height: 48px;
  padding: 14px 32px;
  font-size: 16px;
  font-weight: 600;
}
```

**モバイルでの考慮事項:**
- 最小タップターゲット: 44px × 44px（Apple HIG）
- 推奨タップターゲット: 48dp × 48dp（Material Design）
- ボタン間のスペース: 最低8px

---

### 3. ミラーの法則（Miller's Law）

**定義:**
> 人間の短期記憶は平均7±2個の項目しか保持できない

**実例:**

❌ **悪い例: フォーム**
```
一度に15個の入力フィールド
→ ユーザーは圧倒される
```

✅ **良い例: フォーム**
```
ステップ1: 基本情報（3項目）
ステップ2: 連絡先（3項目）
ステップ3: 詳細（3項目）
→ 各ステップで認知負荷を軽減
```

**UI/UXでの応用:**

✅ **良い実装:**
- **チャンキング** - 情報を3-7個のグループに分ける
- **ステップ分割** - 長いフォームを複数ステップに
- **プログレスバー** - 全体の中でどこにいるか示す

❌ **避けるべき:**
- 一画面に10個以上の異なる要素
- 長すぎる指示文（箇条書きにする）

**実装例:**
```jsx
// ❌ 悪い例: 一度に全て
<form>
  <input name="name" />
  <input name="email" />
  <input name="phone" />
  <input name="address" />
  <input name="city" />
  <input name="state" />
  <input name="zip" />
  <input name="country" />
  <input name="company" />
  <input name="title" />
  // ... さらに続く
</form>

// ✅ 良い例: ステップに分割
<MultiStepForm>
  <Step1>
    <input name="name" />
    <input name="email" />
    <input name="phone" />
  </Step1>
  <Step2>
    <input name="address" />
    <input name="city" />
    <input name="state" />
  </Step2>
  <Step3>
    <input name="company" />
    <input name="title" />
  </Step3>
</MultiStepForm>
```

---

### 4. ヤコブの法則（Jakob's Law）

**定義:**
> ユーザーは、他のサイトで過ごした時間が長いため、あなたのサイトも同じように動作することを期待する

**実例:**

✅ **従うべき慣習:**
- ロゴは左上（クリックでホームに戻る）
- ハンバーガーメニュー（☰）はモバイルナビゲーション
- ショッピングカートアイコンは右上
- 検索は右上、または目立つ位置
- フッターに会社情報、利用規約

❌ **避けるべき:**
- 独自すぎる操作方法（学習コストが高い）
- 一般的な記号の意味を変える

**UI/UXでの応用:**

```jsx
// ✅ 良い例: 標準的なレイアウト
<Header>
  <Logo position="left" onClick={navigateHome} />
  <Navigation position="center" />
  <Search position="right" />
  <CartIcon position="right" />
  <UserIcon position="right" />
</Header>

// ❌ 悪い例: 非標準なレイアウト
<Header>
  <CartIcon position="left" />  {/* カートが左？ */}
  <Logo position="right" />     {/* ロゴが右？ */}
  <Search position="bottom" />  {/* 検索が下？ */}
</Header>
```

---

### 5. テスラーの複雑性保存の法則

**定義:**
> システムには固有の複雑性があり、削減することはできるが、なくすことはできない。複雑性をシステム側で引き受けるか、ユーザー側で引き受けるかの選択になる。

**実例:**

✅ **システムが複雑性を引き受ける:**
```
Googleの検索:
- ユーザー: 単純なキーワード入力
- システム: 複雑なランキングアルゴリズム
```

❌ **ユーザーに複雑性を押し付ける:**
```
検索に正規表現を要求する
→ ほとんどのユーザーは使えない
```

**UI/UXでの応用:**

✅ **良い実装:**
- **スマートデフォルト** - 最も一般的な設定を初期値に
- **オートコンプリート** - 入力候補を自動表示
- **バリデーション** - エラーを事前に防ぐ

```jsx
// ✅ 良い例: 日付選択
<DatePicker
  defaultValue={new Date()}        // スマートデフォルト
  minDate={new Date()}              // 過去は選択できない（バリデーション）
  format="YYYY-MM-DD"               // フォーマットはシステムが管理
  locale="ja"                       // ローカライズ自動
/>

// ❌ 悪い例: ユーザーに複雑性を押し付ける
<input
  type="text"
  placeholder="YYYY-MM-DD形式で入力してください。過去の日付は入力できません。"
  // ユーザーが手入力、フォーマットチェックもユーザー任せ
/>
```

---

## ゲシュタルト原理

人間の視覚認知のパターンを理解する。

### 1. 近接の原理（Proximity）

**定義:**
> 近くにあるものは、関連していると認識される

**実例:**

✅ **良い例:**
```
[ラベル]
[入力フィールド]
    ← 近接している（関連が明確）

[ラベル]


[入力フィールド]
    ← 離れている（関連が不明瞭）
```

**実装:**
```css
/* ✅ 良い例 */
.form-group {
  margin-bottom: 24px;  /* グループ間は広く */
}

.form-group label {
  margin-bottom: 4px;   /* ラベルと入力は近く */
}

.form-group input {
  margin-bottom: 0;
}
```

---

### 2. 類同の原理（Similarity）

**定義:**
> 似ているものは、同じグループとして認識される

**実例:**

✅ **良い例:**
```css
/* プライマリボタンは全て青 */
.button-primary {
  background: blue;
  color: white;
}

/* セカンダリボタンは全てグレー */
.button-secondary {
  background: gray;
  color: black;
}
```

---

### 3. 閉合の原理（Closure）

**定義:**
> 不完全な図形でも、脳が自動的に補完して認識する

**実例:**

```
○ ○ ○
○   ○
○ ○ ○

→ 四角形として認識される
```

**UI/UXでの応用:**
- ローディングスピナー（円の一部でも円と認識）
- ブレッドクラム（> で区切られた経路）

---

### 4. 連続の原理（Continuity）

**定義:**
> 要素は、滑らかな経路に沿って配置されていると認識される

**実例:**

✅ **良い例:**
```
[ステップ1] → [ステップ2] → [ステップ3]
視線が自然に左から右へ流れる
```

---

### 5. 図と地の原理（Figure-Ground）

**定義:**
> 視覚要素を「図」（前景）と「地」（背景）に分離して認識する

**実例:**

✅ **良い例:**
```css
/* モーダル */
.modal-overlay {
  background: rgba(0, 0, 0, 0.5);  /* 地 */
}

.modal-content {
  background: white;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
  /* 図（明確に前に出る） */
}
```

---

## ユニバーサルデザイン

すべてのユーザーが使える設計。

### WCAG 2.1（Web Content Accessibility Guidelines）

#### レベル

- **Level A** - 最低限のアクセシビリティ
- **Level AA** - 推奨レベル（多くの法規制の基準）
- **Level AAA** - 最高レベル

#### 4つの原則（POUR）

**1. Perceivable（知覚可能）**

✅ **実装:**
```jsx
// 画像に代替テキスト
<img src="chart.png" alt="2024年の売上グラフ。Q1が20万円、Q2が30万円..." />

// 色だけに頼らない
<button className="error">
  <WarningIcon /> {/* アイコンも表示 */}
  エラーが発生しました
</button>
```

**2. Operable（操作可能）**

✅ **実装:**
```jsx
// キーボードで操作可能
<button
  onClick={handleClick}
  onKeyPress={(e) => e.key === 'Enter' && handleClick()}
  tabIndex={0}
>
  送信
</button>

// フォーカス表示
<style>
  button:focus {
    outline: 2px solid blue;
    outline-offset: 2px;
  }
</style>
```

**3. Understandable（理解可能）**

✅ **実装:**
```jsx
// 明確なエラーメッセージ
<form>
  <input type="email" aria-label="メールアドレス" />
  {error && (
    <div role="alert" className="error">
      有効なメールアドレスを入力してください。例: user@example.com
    </div>
  )}
</form>
```

**4. Robust（堅牢）**

✅ **実装:**
```html
<!-- セマンティックHTML -->
<nav>
  <ul>
    <li><a href="/">ホーム</a></li>
    <li><a href="/about">会社概要</a></li>
  </ul>
</nav>

<!-- ARIAラベル -->
<button aria-label="メニューを閉じる" aria-expanded="true">
  <CloseIcon />
</button>
```

---

### カラーコントラスト

**WCAG AAの基準:**
- **通常テキスト**: 4.5:1以上
- **大きなテキスト**: 3:1以上

```css
/* ✅ 良い例: コントラスト比 7:1 */
.text {
  color: #000000;  /* 黒 */
  background: #FFFFFF;  /* 白 */
}

/* ❌ 悪い例: コントラスト比 2:1 */
.text {
  color: #CCCCCC;  /* 薄いグレー */
  background: #FFFFFF;  /* 白 */
}
```

**チェックツール:**
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- Chrome DevTools の Lighthouse

---

## デザインの基本原則

### 1. 一貫性（Consistency）

✅ **実装:**
```css
/* デザイントークン */
:root {
  --color-primary: #007bff;
  --color-secondary: #6c757d;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --font-size-sm: 14px;
  --font-size-md: 16px;
  --font-size-lg: 20px;
}

/* すべてのボタンで統一 */
.button {
  padding: var(--spacing-md);
  font-size: var(--font-size-md);
  border-radius: 4px;
}
```

---

### 2. フィードバック（Feedback）

✅ **実装:**
```jsx
// ボタンクリック時の視覚的フィードバック
<button
  onClick={async () => {
    setLoading(true);  // ローディング表示
    await submitForm();
    setSuccess(true);  // 成功メッセージ
    setTimeout(() => setSuccess(false), 3000);
  }}
  disabled={loading}
>
  {loading ? <Spinner /> : '送信'}
</button>

{success && (
  <Alert type="success">
    送信が完了しました！
  </Alert>
)}
```

---

### 3. エラー防止（Error Prevention）

✅ **実装:**
```jsx
// 確認ダイアログ
<button
  onClick={() => {
    if (confirm('本当に削除しますか？この操作は取り消せません。')) {
      deleteItem();
    }
  }}
>
  削除
</button>

// リアルタイムバリデーション
<input
  type="email"
  value={email}
  onChange={(e) => {
    setEmail(e.target.value);
    if (!isValidEmail(e.target.value)) {
      setError('有効なメールアドレスを入力してください');
    } else {
      setError('');
    }
  }}
/>
```

---

## 実践例

### ログインフォームの改善

**Before（悪い例）:**
```jsx
<form>
  <input type="text" />
  <input type="password" />
  <button>Login</button>
</form>
```

**After（良い例）:**
```jsx
<form>
  {/* ラベル追加（知覚可能） */}
  <label htmlFor="email">メールアドレス</label>
  <input
    id="email"
    type="email"
    autoComplete="email"
    aria-required="true"
    aria-describedby="email-error"
  />
  {emailError && (
    <span id="email-error" role="alert" className="error">
      {emailError}
    </span>
  )}

  {/* パスワード表示/非表示トグル */}
  <label htmlFor="password">パスワード</label>
  <div className="password-input">
    <input
      id="password"
      type={showPassword ? 'text' : 'password'}
      autoComplete="current-password"
      aria-required="true"
    />
    <button
      type="button"
      onClick={() => setShowPassword(!showPassword)}
      aria-label={showPassword ? 'パスワードを隠す' : 'パスワードを表示'}
    >
      {showPassword ? <EyeOffIcon /> : <EyeIcon />}
    </button>
  </div>

  {/* 明確なCTA */}
  <button
    type="submit"
    disabled={loading}
    className="button-primary"
  >
    {loading ? <Spinner /> : 'ログイン'}
  </button>

  {/* パスワードリセットリンク */}
  <a href="/forgot-password" className="link-secondary">
    パスワードをお忘れですか？
  </a>
</form>
```

---

## チェックリスト

### デザイン原則の適用

- [ ] ヒックの法則: 選択肢を7個以下に
- [ ] フィッツの法則: ボタンは44px×44px以上
- [ ] ミラーの法則: 情報を3-7個のグループに
- [ ] ヤコブの法則: 一般的な慣習に従う
- [ ] テスラーの法則: 複雑性をシステムが引き受ける

### ゲシュタルト原理

- [ ] 関連要素は近接配置
- [ ] 同じ種類の要素は統一デザイン
- [ ] 視線の流れを意識した配置

### アクセシビリティ

- [ ] すべての画像にalt属性
- [ ] カラーコントラスト比 4.5:1以上
- [ ] キーボードで操作可能
- [ ] スクリーンリーダー対応
- [ ] フォーカス表示が明確

---

**次のステップ:** [デザインフレームワーク](./frameworks.md)を学ぶ

**最終更新**: 2025-10-13
