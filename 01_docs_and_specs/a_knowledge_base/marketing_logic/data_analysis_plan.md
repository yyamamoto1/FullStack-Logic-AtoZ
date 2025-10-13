# データ分析計画

データドリブンな意思決定のための分析手法とツールを学びます。

---

## 📑 目次

1. [データ分析の4つのレベル](#データ分析の4つのレベル)
2. [ファネル分析](#ファネル分析)
3. [コホート分析](#コホート分析)
4. [RFM分析](#rfm分析)
5. [A/Bテスト](#abテスト)
6. [分析ツール](#分析ツール)
7. [実装例](#実装例)

---

## データ分析の4つのレベル

### 1. Descriptive Analytics（記述的分析）

**目的:** 何が起こったかを理解する

**使用する指標:**
- 売上高
- ユーザー数
- ページビュー
- 平均セッション時間

**ツール:**
- ダッシュボード
- レポート
- グラフ

**実例:**

```python
# 月次売上レポート
import pandas as pd
import matplotlib.pyplot as plt

# データ
monthly_revenue = {
    '2024-01': 100000,
    '2024-02': 120000,
    '2024-03': 150000,
    '2024-04': 180000,
    '2024-05': 200000,
}

df = pd.DataFrame(list(monthly_revenue.items()), columns=['月', '売上'])

# 可視化
plt.figure(figsize=(10, 6))
plt.plot(df['月'], df['売上'], marker='o')
plt.title('月次売上推移')
plt.xlabel('月')
plt.ylabel('売上（円）')
plt.xticks(rotation=45)
plt.grid(True)
plt.show()

# 基本統計
print(f"平均売上: {df['売上'].mean():,.0f}円")
print(f"最大売上: {df['売上'].max():,.0f}円")
print(f"成長率: {(df['売上'].iloc[-1] / df['売上'].iloc[0] - 1) * 100:.1f}%")
```

**出力:**
```
平均売上: 150,000円
最大売上: 200,000円
成長率: 100.0%
```

---

### 2. Diagnostic Analytics（診断的分析）

**目的:** なぜ起こったかを理解する

**手法:**
- ドリルダウン分析
- 相関分析
- セグメント分析

**実例: 売上減少の原因分析**

```python
# 売上が減少した月の詳細分析

# 仮説1: 新規ユーザーが減った？
new_users = {
    '2024-01': 500,
    '2024-02': 450,  # 減少
    '2024-03': 480,
    '2024-04': 510,
    '2024-05': 530,
}

# 仮説2: コンバージョン率が下がった？
conversion_rate = {
    '2024-01': 2.5,
    '2024-02': 2.0,  # 減少
    '2024-03': 2.3,
    '2024-04': 2.6,
    '2024-05': 2.8,
}

# 仮説3: 客単価が下がった？
avg_order_value = {
    '2024-01': 8000,
    '2024-02': 8500,
    '2024-03': 9000,
    '2024-04': 9200,
    '2024-05': 9500,
}

# 分析
print("2月の売上減少の原因:")
print("- 新規ユーザー: 10%減")
print("- コンバージョン率: 20%減")  # ←主要因
print("- 客単価: 6%増")
print("\n結論: コンバージョン率の低下が主原因")
```

---

### 3. Predictive Analytics（予測的分析）

**目的:** 何が起こりそうかを予測する

**手法:**
- 回帰分析
- 時系列分析
- 機械学習モデル

**実例: 売上予測**

```python
from sklearn.linear_model import LinearRegression
import numpy as np

# 過去データ
months = np.array([1, 2, 3, 4, 5]).reshape(-1, 1)
revenue = np.array([100000, 120000, 150000, 180000, 200000])

# モデル作成
model = LinearRegression()
model.fit(months, revenue)

# 今後3ヶ月の予測
future_months = np.array([6, 7, 8]).reshape(-1, 1)
predictions = model.predict(future_months)

print("売上予測:")
print(f"6月: {predictions[0]:,.0f}円")
print(f"7月: {predictions[1]:,.0f}円")
print(f"8月: {predictions[2]:,.0f}円")

# 決定係数（精度）
from sklearn.metrics import r2_score
print(f"\nモデル精度（R²）: {r2_score(revenue, model.predict(months)):.2f}")
```

**出力:**
```
売上予測:
6月: 220,000円
7月: 245,000円
8月: 270,000円

モデル精度（R²）: 0.98
```

---

### 4. Prescriptive Analytics（処方的分析）

**目的:** どうすべきかを提案する

**手法:**
- 最適化アルゴリズム
- シミュレーション
- What-If 分析

**実例: 広告予算最適化**

```python
# 各チャネルのROI
channels = {
    'Google Ads': {'budget': 100000, 'roi': 3.0},
    'Facebook Ads': {'budget': 80000, 'roi': 2.5},
    'Twitter Ads': {'budget': 50000, 'roi': 1.8},
    'LinkedIn Ads': {'budget': 70000, 'roi': 2.2},
}

# 現状
total_budget = sum(ch['budget'] for ch in channels.values())
total_revenue = sum(ch['budget'] * ch['roi'] for ch in channels.values())
print(f"現状の総予算: {total_budget:,.0f}円")
print(f"現状の総売上: {total_revenue:,.0f}円")
print(f"現状のROI: {total_revenue / total_budget:.2f}x\n")

# 最適化提案: ROIが高いチャネルに予算を集中
print("【最適化提案】")
print("Google Ads に予算を集中（ROI 3.0が最高）")
optimized_budget = {
    'Google Ads': 200000,  # 倍増
    'Facebook Ads': 80000,
    'Twitter Ads': 0,      # 削減
    'LinkedIn Ads': 20000, # 削減
}

optimized_revenue = (
    optimized_budget['Google Ads'] * 3.0 +
    optimized_budget['Facebook Ads'] * 2.5 +
    optimized_budget['LinkedIn Ads'] * 2.2
)
print(f"最適化後の総予算: {sum(optimized_budget.values()):,.0f}円")
print(f"最適化後の総売上: {optimized_revenue:,.0f}円")
print(f"最適化後のROI: {optimized_revenue / sum(optimized_budget.values()):.2f}x")
print(f"\n改善: +{(optimized_revenue - total_revenue) / total_revenue * 100:.1f}%")
```

---

## ファネル分析

顧客の行動フローを可視化し、離脱ポイントを特定。

### ファネルの構造

```
訪問者      10,000人 (100%)
  ↓
登録        2,000人  (20%)  ← 離脱: 8,000人
  ↓
有料化      400人    (4%)   ← 離脱: 1,600人
  ↓
継続利用    300人    (3%)   ← 離脱: 100人
```

### コンバージョン率の計算

```python
# ファネルデータ
funnel = {
    '訪問者': 10000,
    '登録': 2000,
    '有料化': 400,
    '継続利用': 300,
}

# 各ステップのコンバージョン率
stages = list(funnel.keys())
values = list(funnel.values())

print("ファネル分析\n" + "="*50)
for i in range(len(stages)):
    rate = values[i] / values[0] * 100
    print(f"{stages[i]}: {values[i]:,}人 ({rate:.1f}%)")

    if i > 0:
        step_rate = values[i] / values[i-1] * 100
        drop_off = values[i-1] - values[i]
        print(f"  ← 前ステップからの転換率: {step_rate:.1f}%")
        print(f"  ← 離脱: {drop_off:,}人\n")
```

**出力:**
```
ファネル分析
==================================================
訪問者: 10,000人 (100.0%)
登録: 2,000人 (20.0%)
  ← 前ステップからの転換率: 20.0%
  ← 離脱: 8,000人

有料化: 400人 (4.0%)
  ← 前ステップからの転換率: 20.0%
  ← 離脱: 1,600人

継続利用: 300人 (3.0%)
  ← 前ステップからの転換率: 75.0%
  ← 離脱: 100人
```

### 改善施策の優先順位

```python
# 各ステップの改善インパクト試算

# シナリオ1: 登録率を20% → 25%に改善
scenario1_revenue = 10000 * 0.25 * 0.20 * 0.75 * 5000  # 5000円/月
print(f"シナリオ1（登録率改善）: +{scenario1_revenue - (300 * 5000):,}円")

# シナリオ2: 有料化率を20% → 25%に改善
scenario2_revenue = 10000 * 0.20 * 0.25 * 0.75 * 5000
print(f"シナリオ2（有料化率改善）: +{scenario2_revenue - (300 * 5000):,}円")

# シナリオ3: 継続率を75% → 85%に改善
scenario3_revenue = 10000 * 0.20 * 0.20 * 0.85 * 5000
print(f"シナリオ3（継続率改善）: +{scenario3_revenue - (300 * 5000):,}円")
```

**結論:** 登録率の改善が最もインパクトが大きい

---

## コホート分析

ユーザーをグループ分けして、時間経過による行動変化を分析。

### コホートテーブル

```
登録月    | 1ヶ月後 | 2ヶ月後 | 3ヶ月後 | 4ヶ月後
------------------------------------------------
2024-01   | 100%    | 60%     | 45%     | 40%
2024-02   | 100%    | 65%     | 50%     | 45%
2024-03   | 100%    | 70%     | 55%     | -
2024-04   | 100%    | 75%     | -       | -
2024-05   | 100%    | -       | -       | -
```

### Pythonでの実装

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# コホートデータ
data = {
    '登録月': ['2024-01', '2024-02', '2024-03', '2024-04', '2024-05'],
    '1ヶ月後': [100, 100, 100, 100, 100],
    '2ヶ月後': [60, 65, 70, 75, None],
    '3ヶ月後': [45, 50, 55, None, None],
    '4ヶ月後': [40, 45, None, None, None],
}

df = pd.DataFrame(data)
df = df.set_index('登録月')

# ヒートマップで可視化
plt.figure(figsize=(10, 6))
sns.heatmap(df, annot=True, fmt='.0f', cmap='YlOrRd', cbar_kws={'label': 'リテンション率(%)'})
plt.title('コホート分析: ユーザーリテンション')
plt.xlabel('経過月数')
plt.ylabel('登録月')
plt.show()

# 分析
print("【分析結果】")
print(f"3ヶ月後の平均リテンション: {df['3ヶ月後'].mean():.0f}%")
print("改善トレンド: 新しいコホートほどリテンションが高い")
print("施策: オンボーディングの改善が効果を出している")
```

---

## RFM分析

顧客を Recency（最終購入日）、Frequency（購入頻度）、Monetary（購入金額）で分類。

### RFMスコアリング

```python
import pandas as pd

# 顧客データ
customers = pd.DataFrame({
    'customer_id': [1, 2, 3, 4, 5],
    'recency': [5, 30, 60, 90, 120],      # 最終購入からの日数
    'frequency': [10, 5, 3, 2, 1],        # 購入回数
    'monetary': [100000, 50000, 30000, 20000, 10000],  # 累計購入額
})

# RFMスコア算出（1-5点）
customers['R_score'] = pd.qcut(customers['recency'], 5, labels=[5, 4, 3, 2, 1], duplicates='drop')
customers['F_score'] = pd.qcut(customers['frequency'], 5, labels=[1, 2, 3, 4, 5], duplicates='drop')
customers['M_score'] = pd.qcut(customers['monetary'], 5, labels=[1, 2, 3, 4, 5], duplicates='drop')

# 総合スコア
customers['RFM_score'] = (
    customers['R_score'].astype(int) +
    customers['F_score'].astype(int) +
    customers['M_score'].astype(int)
)

# セグメント分類
def classify_customer(row):
    if row['RFM_score'] >= 13:
        return '優良顧客'
    elif row['RFM_score'] >= 10:
        return '一般顧客'
    elif row['R_score'] <= 2:
        return '離脱リスク'
    else:
        return '新規顧客'

customers['segment'] = customers.apply(classify_customer, axis=1)

print(customers[['customer_id', 'RFM_score', 'segment']])
```

**出力:**
```
   customer_id  RFM_score   segment
0            1         15   優良顧客
1            2         12   一般顧客
2            3          9   新規顧客
3            4          7   離脱リスク
4            5          5   離脱リスク
```

### セグメント別施策

```python
strategies = {
    '優良顧客': {
        '施策': 'VIPプログラム、先行アクセス',
        '目的': 'ロイヤルティ維持',
        '予算': '高',
    },
    '一般顧客': {
        '施策': 'アップセル、クロスセル',
        '目的': '購入額増加',
        '予算': '中',
    },
    '新規顧客': {
        '施策': 'オンボーディング、チュートリアル',
        '目的': 'アクティベーション',
        '予算': '中',
    },
    '離脱リスク': {
        '施策': '再エンゲージメントキャンペーン',
        '目的': '離脱防止',
        '予算': '低',
    },
}

for segment, strategy in strategies.items():
    print(f"\n【{segment}】")
    print(f"施策: {strategy['施策']}")
    print(f"目的: {strategy['目的']}")
    print(f"予算: {strategy['予算']}")
```

---

## A/Bテスト

2つのバージョンを比較して、どちらが優れているかを統計的に検証。

### A/Bテストの流れ

```
1. 仮説設定
   ↓
2. 実験設計
   ↓
3. データ収集
   ↓
4. 統計検定
   ↓
5. 意思決定
```

### 実装例

```python
import numpy as np
from scipy import stats

# A/Bテストデータ
# バージョンA: 既存のCTAボタン（青）
# バージョンB: 新しいCTAボタン（赤）

visitors_A = 10000
conversions_A = 500
cvr_A = conversions_A / visitors_A

visitors_B = 10000
conversions_B = 550
cvr_B = conversions_B / visitors_B

print("A/Bテスト結果")
print("="*50)
print(f"バージョンA: {conversions_A}/{visitors_A} = {cvr_A*100:.2f}%")
print(f"バージョンB: {conversions_B}/{visitors_B} = {cvr_B*100:.2f}%")
print(f"改善率: {(cvr_B/cvr_A - 1)*100:.1f}%\n")

# 統計的有意性の検定（カイ二乗検定）
contingency_table = np.array([
    [conversions_A, visitors_A - conversions_A],
    [conversions_B, visitors_B - conversions_B]
])

chi2, p_value, dof, expected = stats.chi2_contingency(contingency_table)

print(f"p値: {p_value:.4f}")
if p_value < 0.05:
    print("結論: 統計的に有意な差がある（p < 0.05）")
    print("判定: バージョンBを採用 ✅")
else:
    print("結論: 統計的に有意な差がない（p >= 0.05）")
    print("判定: バージョンAを継続")
```

### サンプルサイズ計算

```python
from statsmodels.stats.power import zt_ind_solve_power

# 必要なサンプルサイズを計算
base_cvr = 0.05  # 既存のCVR: 5%
expected_cvr = 0.055  # 期待するCVR: 5.5%
effect_size = (expected_cvr - base_cvr) / np.sqrt(base_cvr * (1 - base_cvr))

sample_size = zt_ind_solve_power(
    effect_size=effect_size,
    alpha=0.05,      # 有意水準
    power=0.8,       # 検出力
    alternative='two-sided'
)

print(f"\n【サンプルサイズ計算】")
print(f"必要な訪問者数（各バージョン）: {sample_size:.0f}人")
print(f"合計: {sample_size * 2:.0f}人")
```

---

## 分析ツール

### Google Analytics 4

**設定方法:**

```html
<!-- Google Analytics 4 タグ -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

**イベントトラッキング:**

```javascript
// ボタンクリックをトラッキング
gtag('event', 'button_click', {
  'event_category': 'CTA',
  'event_label': 'Sign Up Button',
  'value': 1
});

// コンバージョン
gtag('event', 'purchase', {
  'transaction_id': 'T12345',
  'value': 9800,
  'currency': 'JPY',
  'items': [
    {
      'item_id': 'SKU_12345',
      'item_name': 'Pro Plan',
      'price': 9800,
      'quantity': 1
    }
  ]
});
```

---

### Mixpanel

```javascript
// Mixpanel初期化
mixpanel.init('YOUR_PROJECT_TOKEN');

// ユーザー識別
mixpanel.identify('user_12345');

// プロパティ設定
mixpanel.people.set({
  '$email': 'user@example.com',
  '$name': '田中太郎',
  'plan': 'Pro',
  'signup_date': '2024-10-13'
});

// イベント送信
mixpanel.track('Task Created', {
  'task_type': 'development',
  'priority': 'high',
  'estimated_time': 120
});

// ファネル分析用
mixpanel.track('Signup Step 1 - Visit');
mixpanel.track('Signup Step 2 - Email');
mixpanel.track('Signup Step 3 - Complete');
```

---

### Amplitude

```javascript
// Amplitude初期化
amplitude.getInstance().init('YOUR_API_KEY');

// ユーザー識別
amplitude.getInstance().setUserId('user_12345');

// ユーザープロパティ
amplitude.getInstance().setUserProperties({
  'plan': 'Pro',
  'team_size': 5,
  'industry': 'Technology'
});

// イベント送信
amplitude.getInstance().logEvent('Feature Used', {
  'feature_name': 'AI Priority',
  'usage_count': 1,
  'user_role': 'admin'
});
```

---

## 実装例

### ダッシュボード構築（React + Chart.js）

```jsx
// Dashboard.jsx
import React, { useState, useEffect } from 'react';
import { Line, Bar, Doughnut } from 'react-chartjs-2';

const Dashboard = () => {
  const [metrics, setMetrics] = useState({
    revenue: [],
    users: [],
    conversionRate: [],
  });

  useEffect(() => {
    // APIからデータ取得
    fetch('/api/analytics/metrics')
      .then(res => res.json())
      .then(data => setMetrics(data));
  }, []);

  // 売上推移グラフ
  const revenueData = {
    labels: ['1月', '2月', '3月', '4月', '5月'],
    datasets: [{
      label: '売上',
      data: metrics.revenue,
      borderColor: 'rgb(75, 192, 192)',
      tension: 0.1
    }]
  };

  // ユーザー数グラフ
  const usersData = {
    labels: ['1月', '2月', '3月', '4月', '5月'],
    datasets: [{
      label: 'ユーザー数',
      data: metrics.users,
      backgroundColor: 'rgba(54, 162, 235, 0.5)',
    }]
  };

  // コンバージョン率
  const conversionData = {
    labels: ['訪問', '登録', '有料化'],
    datasets: [{
      data: [10000, 2000, 400],
      backgroundColor: [
        'rgba(255, 99, 132, 0.5)',
        'rgba(54, 162, 235, 0.5)',
        'rgba(255, 206, 86, 0.5)',
      ],
    }]
  };

  return (
    <div className="dashboard">
      <h1>分析ダッシュボード</h1>

      <div className="metrics-grid">
        <div className="metric-card">
          <h2>売上推移</h2>
          <Line data={revenueData} />
        </div>

        <div className="metric-card">
          <h2>ユーザー数</h2>
          <Bar data={usersData} />
        </div>

        <div className="metric-card">
          <h2>コンバージョンファネル</h2>
          <Doughnut data={conversionData} />
        </div>
      </div>
    </div>
  );
};

export default Dashboard;
```

---

**次のステップ:** [KPI定義とロジックツリー](./kpi_definition.md)を学ぶ

**最終更新**: 2025-10-13
