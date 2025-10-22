# 🔰 演習1: 個人プロフィールページ
**難易度**: ⭐ 初級  
**推定時間**: 4-6時間  
**AI Education Assistant #8 担当**

---

## 🎯 学習目標

この演習を通じて以下のスキルを習得します：

- HTML5のセマンティックタグの理解と活用
- CSSの基本レイアウト（Flexbox, Grid）
- JavaScriptのDOM操作
- レスポンシブデザインの実装
- フォームの操作とバリデーション
- ローカルストレージの活用

---

## 📋 要件定義

### 機能要件

#### 必須機能
1. **プロフィール表示**
   - 名前、職業、自己紹介文
   - プロフィール画像
   - スキル一覧（タグ形式）
   - SNSリンク（GitHub, Twitter, LinkedIn）

2. **プロフィール編集**
   - インライン編集機能
   - リアルタイムプレビュー
   - 保存機能（ローカルストレージ）

3. **スキル管理**
   - スキル追加・削除
   - スキルレベル設定（1-5段階）
   - カテゴリ別分類

#### 推奨機能
4. **テーマ切り替え**
   - ライト/ダークモード
   - カスタムカラー設定

5. **アニメーション**
   - スムーズなトランジション
   - ホバーエフェクト
   - スクロールアニメーション

### 非機能要件

- **レスポンシブ**: モバイル、タブレット、デスクトップ対応
- **アクセシビリティ**: WCAG 2.1 Level AA準拠
- **パフォーマンス**: Lighthouse スコア90+
- **ブラウザ対応**: Chrome, Firefox, Safari, Edge最新版

---

## 🎨 デザインガイドライン

### レイアウト構成

```
┌─────────────────────────────────┐
│           Header                │ 
├─────────────────────────────────┤
│  Profile │                     │
│  Image   │   Basic Info        │
│          │   (Name, Title)     │
├─────────────────────────────────┤
│          Bio Section            │
├─────────────────────────────────┤
│          Skills Section         │
├─────────────────────────────────┤
│          Contact Section        │
└─────────────────────────────────┘
```

### カラーパレット

**ライトテーマ:**
- Primary: #3B82F6 (Blue 500)
- Secondary: #10B981 (Emerald 500)
- Background: #FFFFFF
- Surface: #F9FAFB (Gray 50)
- Text: #111827 (Gray 900)

**ダークテーマ:**
- Primary: #60A5FA (Blue 400)
- Secondary: #34D399 (Emerald 400)
- Background: #0F172A (Slate 900)
- Surface: #1E293B (Slate 800)
- Text: #F1F5F9 (Slate 100)

### フォント
- Heading: 'Inter', sans-serif
- Body: 'Inter', sans-serif
- Code: 'JetBrains Mono', monospace

---

## 🗂️ ファイル構成

```
01_profile_page/
├── starter/
│   ├── index.html
│   ├── css/
│   │   ├── style.css
│   │   └── responsive.css
│   ├── js/
│   │   ├── main.js
│   │   ├── storage.js
│   │   └── theme.js
│   ├── assets/
│   │   ├── images/
│   │   └── icons/
│   └── data/
│       └── profile.json
├── solution/
│   └── [完成版ファイル群]
├── tests/
│   ├── visual-tests/
│   └── unit-tests/
└── docs/
    ├── wireframes/
    └── api-spec.md
```

---

## 🚀 実装手順

### Phase 1: 基本構造の作成（1-2時間）

1. **HTML構造の作成**
   ```html
   <!DOCTYPE html>
   <html lang="ja">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>プロフィール | あなたの名前</title>
   </head>
   <body>
       <header class="header">
           <!-- テーマ切り替えボタン -->
       </header>
       
       <main class="main">
           <section class="profile-section">
               <!-- プロフィール画像と基本情報 -->
           </section>
           
           <section class="bio-section">
               <!-- 自己紹介文 -->
           </section>
           
           <section class="skills-section">
               <!-- スキル一覧 -->
           </section>
           
           <section class="contact-section">
               <!-- 連絡先・SNS -->
           </section>
       </main>
   </body>
   </html>
   ```

2. **CSS基本スタイル**
   - CSS Reset/Normalize
   - カスタムプロパティ（CSS変数）
   - 基本レイアウト（Flexbox/Grid）

### Phase 2: スタイリングとレスポンシブ（2-3時間）

3. **詳細スタイリング**
   ```css
   :root {
       /* ライトテーマ */
       --primary-color: #3B82F6;
       --background-color: #FFFFFF;
       --text-color: #111827;
   }
   
   [data-theme="dark"] {
       /* ダークテーマ */
       --primary-color: #60A5FA;
       --background-color: #0F172A;
       --text-color: #F1F5F9;
   }
   
   .profile-card {
       background: var(--background-color);
       color: var(--text-color);
       border-radius: 12px;
       box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
   }
   ```

4. **レスポンシブデザイン**
   ```css
   /* モバイル */
   @media (max-width: 768px) {
       .profile-section {
           flex-direction: column;
           text-align: center;
       }
   }
   
   /* タブレット */
   @media (min-width: 769px) and (max-width: 1024px) {
       .main {
           max-width: 720px;
       }
   }
   
   /* デスクトップ */
   @media (min-width: 1025px) {
       .main {
           max-width: 1200px;
       }
   }
   ```

### Phase 3: JavaScript機能実装（1-2時間）

5. **データ管理**
   ```javascript
   // storage.js
   class ProfileStorage {
       static save(data) {
           localStorage.setItem('userProfile', JSON.stringify(data));
       }
       
       static load() {
           const data = localStorage.getItem('userProfile');
           return data ? JSON.parse(data) : this.getDefaultProfile();
       }
       
       static getDefaultProfile() {
           return {
               name: 'あなたの名前',
               title: 'あなたの職業',
               bio: 'あなたの自己紹介文',
               skills: [],
               contacts: {}
           };
       }
   }
   ```

6. **編集機能**
   ```javascript
   // main.js
   class ProfileEditor {
       constructor() {
           this.profile = ProfileStorage.load();
           this.init();
       }
       
       init() {
           this.render();
           this.bindEvents();
       }
       
       makeEditable(element) {
           element.contentEditable = true;
           element.focus();
           element.addEventListener('blur', () => this.saveChanges());
       }
       
       saveChanges() {
           // 変更を保存
           ProfileStorage.save(this.profile);
       }
   }
   ```

7. **テーマ切り替え**
   ```javascript
   // theme.js
   class ThemeManager {
       constructor() {
           this.theme = localStorage.getItem('theme') || 'light';
           this.apply();
       }
       
       toggle() {
           this.theme = this.theme === 'light' ? 'dark' : 'light';
           this.apply();
           localStorage.setItem('theme', this.theme);
       }
       
       apply() {
           document.documentElement.setAttribute('data-theme', this.theme);
       }
   }
   ```

---

## ✅ チェックリスト

### 機能チェック
- [ ] プロフィール情報が表示される
- [ ] インライン編集が可能
- [ ] 変更が自動保存される
- [ ] テーマ切り替えが動作する
- [ ] スキル追加・削除ができる
- [ ] SNSリンクが正しく動作する

### 品質チェック
- [ ] レスポンシブデザインが適切
- [ ] アクセシビリティに配慮
- [ ] パフォーマンスが良好
- [ ] コードが整理されている
- [ ] エラーハンドリングが適切

### テスト項目
- [ ] 異なるデバイスサイズでの表示確認
- [ ] ブラウザ互換性確認
- [ ] キーボード操作確認
- [ ] スクリーンリーダー対応確認

---

## 🎓 学習リソース

### 参考サイト
- [MDN Web Docs - HTML](https://developer.mozilla.org/ja/docs/Web/HTML)
- [MDN Web Docs - CSS](https://developer.mozilla.org/ja/docs/Web/CSS)
- [JavaScript.info](https://ja.javascript.info/)

### デザインインスピレーション
- [Dribbble - Profile Cards](https://dribbble.com/search/profile-card)
- [Behance - Portfolio Design](https://www.behance.net/search/projects?search=portfolio%20design)

### ツール
- [Lighthouse](https://developers.google.com/web/tools/lighthouse) - パフォーマンス測定
- [WAVE](https://wave.webaim.org/) - アクセシビリティチェック
- [Can I Use](https://caniuse.com/) - ブラウザ対応確認

---

## 📝 提出方法

1. 完成したコードをGitHubリポジトリにプッシュ
2. README.mdに実装のポイントと学んだことを記載
3. デモサイトをGitHub Pagesなどで公開
4. 学習ジャーナルに進捗を記録

### 評価ポイント
- **創造性** (25%): オリジナリティとアイデア
- **技術力** (35%): 実装の正確性と効率性
- **デザイン** (25%): UI/UXの品質
- **文書化** (15%): コード・設計の説明

---

## 🎉 完了後のNext Step

1. **[TODOアプリ](../02_todo_app/)** でReact基礎を学習
2. **[天気予報アプリ](../03_weather_app/)** でAPI連携を習得
3. 作成したプロフィールページにポートフォリオ機能を追加

---

**🎓 AI Education Assistant による作成**: 2025-10-22  
**📈 難易度**: ⭐ 初級 | **⏱️ 推定時間**: 4-6時間