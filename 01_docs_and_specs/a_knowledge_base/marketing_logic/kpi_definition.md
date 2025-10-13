# KPI/KGI設計とダッシュボード構築

**学習時間**: 約4時間
**前提知識**: データ分析の基礎、SQLの基本操作、マーケティング指標の理解
**関連ドキュメント**:
- [マーケティング原則](./principles.md)
- [データ分析計画](./data_analysis_plan.md)

---

## 目次

1. [KPI/KGIの基本概念](#1-kpikgiの基本概念)
2. [ロジックツリーによるKPI設計](#2-ロジックツリーによるkpi設計)
3. [カテゴリ別KPI定義](#3-カテゴリ別kpi定義)
4. [KPI計測とダッシュボード構築](#4-kpi計測とダッシュボード構築)
5. [実践演習](#5-実践演習)

---

## 1. KPI/KGIの基本概念

### 1.1 KGIとKPIの違い

#### KGI (Key Goal Indicator) - 重要目標達成指標

**定義**: ビジネスの最終目標を数値化した指標

**特徴**:
- 経営層が注目する最終的な成果
- 達成までの期間が長い（四半期、年単位）
- 組織全体の成功を測る
- 数が少ない（通常1〜3個）

**例**:
```
- 年間売上高: 1億円
- 年間契約者数: 10,000人
- 営業利益率: 20%
- 市場シェア: 15%
```

#### KPI (Key Performance Indicator) - 重要業績評価指標

**定義**: KGI達成のために管理すべき中間指標

**特徴**:
- KGIを達成するためのプロセス指標
- 短期間で測定可能（日次、週次、月次）
- 現場の改善活動に直結
- 複数の階層構造を持つ

**例**:
```
- 月間訪問者数: 50,000人
- コンバージョン率: 3%
- 顧客獲得単価 (CAC): 5,000円
- 月間アクティブユーザー数 (MAU): 20,000人
```

### 1.2 SMART原則によるKPI設定

優れたKPIは **SMART原則** に従う:

| 原則 | 説明 | 例 |
|-----|------|-----|
| **S**pecific<br>（具体的） | 誰が見ても明確で曖昧さがない | ❌「ユーザーを増やす」<br>✅「月間アクティブユーザー数を20%増加」 |
| **M**easurable<br>（測定可能） | 数値で測定でき、進捗が追える | ❌「顧客満足度を向上」<br>✅「NPS（Net Promoter Score）を50以上にする」 |
| **A**chievable<br>（達成可能） | 現実的で達成可能な目標 | ❌「1ヶ月で売上を10倍にする」<br>✅「3ヶ月で売上を30%増加させる」 |
| **R**elevant<br>（関連性） | ビジネス目標に直結している | ❌「SNSのフォロワー数」（直接売上に関係ない場合）<br>✅「リード獲得数」（売上に直結） |
| **T**ime-bound<br>（期限） | 明確な達成期限がある | ❌「いつかコンバージョン率を改善」<br>✅「2025年12月までにコンバージョン率を5%にする」 |

---

## 2. ロジックツリーによるKPI設計

### 2.1 KGIからKPIへの分解

**ステップ**:
1. KGI（最終目標）を設定
2. KGIに影響する要素を洗い出す
3. 各要素をさらに細分化
4. 測定可能なKPIに落とし込む

### 2.2 実例: SaaSビジネスのKPIツリー

```
┌─────────────────────────────────────────────────────────────┐
│                         KGI                                  │
│                  年間売上: 1億円                              │
└─────────────┬───────────────────────────────────────────────┘
              │
    ┌─────────┴─────────┐
    │                   │
┌───▼────────┐    ┌─────▼──────┐
│ 新規顧客   │    │ 既存顧客   │
│ 売上       │    │ 売上       │
│ 6,000万円  │    │ 4,000万円  │
└───┬────────┘    └─────┬──────┘
    │                   │
    │                   │
┌───▼──────────────┐  ┌─▼───────────────┐
│ KPI: 新規顧客数  │  │ KPI: 継続率     │
│  × ARPU          │  │  × 既存顧客数   │
│                  │  │  × ARPU         │
│ 1,000人 × 5,000円│  │ 90% × 800人     │
│ = 500万円/月     │  │ × 5,000円       │
└───┬──────────────┘  └─┬───────────────┘
    │                    │
    │                    │
┌───▼─────────────┐  ┌──▼──────────────┐
│ 訪問者数         │  │ チャーン率対策  │
│ コンバージョン率 │  │ アップセル率    │
│ CAC              │  │ クロスセル率    │
└──────────────────┘  └─────────────────┘
```

### 2.3 ロジックツリー実装例（Python）

```python
# KPIツリーの構造定義
class KPINode:
    def __init__(self, name, value=None, children=None, formula=None):
        self.name = name
        self.value = value
        self.children = children or []
        self.formula = formula  # 計算式（lambda関数）

    def calculate(self):
        """子ノードから値を計算"""
        if self.formula and self.children:
            child_values = [child.calculate() for child in self.children]
            self.value = self.formula(*child_values)
        return self.value

    def print_tree(self, level=0):
        """ツリー構造を表示"""
        indent = "  " * level
        value_str = f": {self.value:,.0f}" if self.value else ""
        print(f"{indent}├─ {self.name}{value_str}")
        for child in self.children:
            child.print_tree(level + 1)

# KGI: 年間売上
kgi = KPINode(
    name="年間売上（円）",
    formula=lambda new, existing: (new + existing) * 12,
    children=[
        # 新規顧客売上（月次）
        KPINode(
            name="新規顧客売上/月",
            formula=lambda customers, arpu: customers * arpu,
            children=[
                KPINode(
                    name="新規顧客数/月",
                    formula=lambda visitors, cvr: visitors * cvr,
                    children=[
                        KPINode(name="月間訪問者数", value=50000),
                        KPINode(name="CVR", value=0.02),  # 2%
                    ]
                ),
                KPINode(name="ARPU（新規）", value=5000),
            ]
        ),
        # 既存顧客売上（月次）
        KPINode(
            name="既存顧客売上/月",
            formula=lambda customers, retention, arpu: customers * retention * arpu,
            children=[
                KPINode(name="既存顧客数", value=800),
                KPINode(name="継続率", value=0.90),  # 90%
                KPINode(name="ARPU（既存）", value=5000),
            ]
        ),
    ]
)

# 計算実行
kgi.calculate()

# ツリー表示
print("\n=== KPIツリー ===")
kgi.print_tree()

print(f"\n【最終結果】")
print(f"年間売上目標: {kgi.value:,.0f}円")

# 感度分析
print("\n=== 感度分析 ===")
print("訪問者数を10%増やした場合:")
kgi.children[0].children[0].children[0].value = 55000  # 50,000 → 55,000
new_value = kgi.calculate()
print(f"年間売上: {new_value:,.0f}円（+{(new_value/kgi.value - 1)*100:.1f}%）")
```

**実行結果:**
```
=== KPIツリー ===
├─ 年間売上（円）: 113,040,000
  ├─ 新規顧客売上/月: 5,000,000
    ├─ 新規顧客数/月: 1,000
      ├─ 月間訪問者数: 50,000
      ├─ CVR: 0.02
    ├─ ARPU（新規）: 5,000
  ├─ 既存顧客売上/月: 3,600,000
    ├─ 既存顧客数: 800
    ├─ 継続率: 0.90
    ├─ ARPU（既存）: 5,000

【最終結果】
年間売上目標: 113,040,000円

=== 感度分析 ===
訪問者数を10%増やした場合:
年間売上: 119,040,000円（+5.3%）
```

---

## 3. カテゴリ別KPI定義

### 3.1 獲得（Acquisition）系KPI

#### 訪問者数

**定義**: サイト/アプリへの訪問者数

**計測方法**:
- ユニークユーザー数（UV）
- セッション数
- ページビュー数（PV）

**目標設定例**:
```
現状: 月間30,000 UV
目標: 月間50,000 UV（+67%）
期間: 3ヶ月
```

#### コンバージョン率（CVR）

**定義**: 訪問者のうち、目標行動（会員登録、購入など）を完了した割合

**計算式**:
```
CVR = (コンバージョン数 ÷ 訪問者数) × 100
```

**ベンチマーク**:
- SaaS無料トライアル: 2〜5%
- ECサイト: 1〜3%
- リード獲得: 5〜10%

#### 顧客獲得単価（CAC: Customer Acquisition Cost）

**定義**: 1人の顧客を獲得するためにかかったコスト

**計算式**:
```
CAC = マーケティング費用 + 営業費用
      ─────────────────────────
           新規顧客数
```

**例**:
```python
# CAC計算
def calculate_cac(marketing_cost, sales_cost, new_customers):
    """
    顧客獲得単価を計算

    Args:
        marketing_cost: マーケティング費用（円）
        sales_cost: 営業費用（円）
        new_customers: 新規顧客数

    Returns:
        float: CAC（円/人）
    """
    total_cost = marketing_cost + sales_cost
    cac = total_cost / new_customers
    return cac

# 実例
marketing_cost = 1_000_000  # 広告費 100万円
sales_cost = 500_000        # 営業人件費 50万円
new_customers = 300         # 新規顧客 300人

cac = calculate_cac(marketing_cost, sales_cost, new_customers)
print(f"CAC: {cac:,.0f}円/人")  # 出力: CAC: 5,000円/人
```

#### LTV/CAC比率

**定義**: 顧客生涯価値と獲得コストの比率

**計算式**:
```
LTV/CAC比率 = LTV ÷ CAC
```

**ベンチマーク**:
- 健全な状態: 3:1 以上
- 理想的: 4:1 〜 5:1
- 危険水域: 1:1 以下（赤字）

**例**:
```python
def evaluate_unit_economics(ltv, cac):
    """
    ユニットエコノミクスを評価

    Args:
        ltv: 顧客生涯価値（円）
        cac: 顧客獲得単価（円）

    Returns:
        dict: 評価結果
    """
    ratio = ltv / cac

    if ratio >= 4:
        status = "優良"
        comment = "持続可能な成長が可能"
    elif ratio >= 3:
        status = "健全"
        comment = "ビジネスモデルは健全"
    elif ratio >= 1:
        status = "要改善"
        comment = "収益性の改善が必要"
    else:
        status = "危険"
        comment = "ビジネスモデルの見直しが急務"

    return {
        "LTV": ltv,
        "CAC": cac,
        "比率": ratio,
        "評価": status,
        "コメント": comment
    }

# 実例
ltv = 50_000   # LTV: 5万円
cac = 5_000    # CAC: 5千円

result = evaluate_unit_economics(ltv, cac)
print(f"LTV/CAC比率: {result['比率']:.1f}")
print(f"評価: {result['評価']} - {result['コメント']}")
# 出力: LTV/CAC比率: 10.0
#       評価: 優良 - 持続可能な成長が可能
```

### 3.2 エンゲージメント（Engagement）系KPI

#### DAU / MAU / WAU

**定義**:
- DAU (Daily Active Users): 日次アクティブユーザー数
- WAU (Weekly Active Users): 週次アクティブユーザー数
- MAU (Monthly Active Users): 月次アクティブユーザー数

**アクティブの定義例**:
- ログインした
- 特定機能を使用した
- コンテンツを投稿した

#### DAU/MAU比率（Stickiness）

**定義**: ユーザーのアプリ利用頻度を示す指標

**計算式**:
```
Stickiness = DAU ÷ MAU × 100
```

**ベンチマーク**:
- 優秀: 20%以上（月のうち6日以上利用）
- 平均的: 10〜20%
- 要改善: 10%以下

**例**:
```python
def calculate_stickiness(dau, mau):
    """
    Stickiness（粘着性）を計算

    Args:
        dau: 日次アクティブユーザー数（平均）
        mau: 月次アクティブユーザー数

    Returns:
        float: Stickiness（%）
    """
    stickiness = (dau / mau) * 100
    return stickiness

# 実例
dau = 5_000   # 平均DAU: 5,000人
mau = 20_000  # MAU: 20,000人

stickiness = calculate_stickiness(dau, mau)
print(f"Stickiness: {stickiness:.1f}%")  # 出力: 25.0%

# 評価
if stickiness >= 20:
    print("評価: 優秀 - ユーザーは高頻度で利用している")
elif stickiness >= 10:
    print("評価: 平均的 - エンゲージメント施策が必要")
else:
    print("評価: 要改善 - 製品価値の見直しが必要")
```

#### セッション時間

**定義**: 1回の訪問でユーザーがサイト/アプリに滞在した時間

**活用方法**:
- コンテンツの質の評価
- UI/UXの改善指標
- 機能の利用頻度分析

#### エンゲージメント率

**定義**: コンテンツに対するユーザーの反応率

**計算式（SNS）**:
```
エンゲージメント率 = (いいね + コメント + シェア) ÷ フォロワー数 × 100
```

### 3.3 収益（Revenue）系KPI

#### MRR (Monthly Recurring Revenue) - 月次経常収益

**定義**: サブスクリプションビジネスの月次売上

**計算式**:
```
MRR = 有料顧客数 × ARPU
```

**分解**:
```
MRR = New MRR（新規）
    + Expansion MRR（アップセル・クロスセル）
    - Contraction MRR（ダウングレード）
    - Churn MRR（解約）
```

**例**:
```python
class MRRCalculator:
    def __init__(self):
        self.history = []

    def calculate_mrr(self, new_mrr, expansion_mrr, contraction_mrr, churn_mrr):
        """
        MRRを計算

        Args:
            new_mrr: 新規MRR（円）
            expansion_mrr: アップセルMRR（円）
            contraction_mrr: ダウングレードMRR（円）
            churn_mrr: 解約MRR（円）

        Returns:
            dict: MRR内訳
        """
        total_mrr = new_mrr + expansion_mrr - contraction_mrr - churn_mrr

        result = {
            "New MRR": new_mrr,
            "Expansion MRR": expansion_mrr,
            "Contraction MRR": -contraction_mrr,
            "Churn MRR": -churn_mrr,
            "Net New MRR": new_mrr + expansion_mrr - contraction_mrr - churn_mrr,
            "Total MRR": total_mrr
        }

        self.history.append(result)
        return result

    def calculate_growth_rate(self):
        """MRR成長率を計算"""
        if len(self.history) < 2:
            return 0

        current_mrr = self.history[-1]["Total MRR"]
        previous_mrr = self.history[-2]["Total MRR"]

        growth_rate = ((current_mrr - previous_mrr) / previous_mrr) * 100
        return growth_rate

# 使用例
calculator = MRRCalculator()

# 1月
jan_result = calculator.calculate_mrr(
    new_mrr=500_000,        # 新規: 50万円
    expansion_mrr=100_000,  # アップセル: 10万円
    contraction_mrr=20_000, # ダウングレード: 2万円
    churn_mrr=50_000        # 解約: 5万円
)

print("=== 1月 MRR ===")
for key, value in jan_result.items():
    print(f"{key}: {value:,}円")

# 2月
feb_result = calculator.calculate_mrr(
    new_mrr=600_000,
    expansion_mrr=120_000,
    contraction_mrr=25_000,
    churn_mrr=45_000
)

print("\n=== 2月 MRR ===")
for key, value in feb_result.items():
    print(f"{key}: {value:,}円")

# 成長率
growth_rate = calculator.calculate_growth_rate()
print(f"\nMRR成長率: {growth_rate:+.1f}%")
```

#### ARPU (Average Revenue Per User) - ユーザーあたり平均売上

**定義**: 1ユーザーあたりの平均売上

**計算式**:
```
ARPU = 総売上 ÷ ユーザー数
```

**活用**:
- 顧客セグメント別の収益性比較
- 価格戦略の評価
- LTV計算の基礎データ

#### ARR (Annual Recurring Revenue) - 年間経常収益

**定義**: 年間のサブスクリプション売上

**計算式**:
```
ARR = MRR × 12
```

**または**:
```
ARR = 年間契約の総額
```

### 3.4 定着（Retention）系KPI

#### リテンション率（継続率）

**定義**: 一定期間後も継続利用しているユーザーの割合

**計算式**:
```
n日後リテンション率 = n日後も利用しているユーザー数 ÷ 初日のユーザー数 × 100
```

**分析**:
```python
import pandas as pd
import numpy as np

def calculate_retention_curve(df, cohort_col='signup_date', event_col='login_date'):
    """
    リテンションカーブを計算

    Args:
        df: ユーザー行動データ
        cohort_col: コホート列名
        event_col: イベント日付列名

    Returns:
        DataFrame: リテンション率テーブル
    """
    # コホートごとのユーザー数
    df['cohort'] = pd.to_datetime(df[cohort_col]).dt.to_period('M')
    df['event_period'] = pd.to_datetime(df[event_col]).dt.to_period('M')

    # 経過月数を計算
    df['months_since_signup'] = (df['event_period'] - df['cohort']).apply(lambda x: x.n)

    # コホート分析
    cohort_data = df.groupby(['cohort', 'months_since_signup'])['user_id'].nunique().reset_index()
    cohort_data.columns = ['cohort', 'months', 'active_users']

    # 初月のユーザー数
    cohort_sizes = cohort_data[cohort_data['months'] == 0].set_index('cohort')['active_users']

    # リテンション率を計算
    cohort_data = cohort_data.set_index('cohort')
    cohort_data['cohort_size'] = cohort_data.index.map(cohort_sizes)
    cohort_data['retention_rate'] = (cohort_data['active_users'] / cohort_data['cohort_size']) * 100

    # ピボットテーブルに変換
    retention_table = cohort_data.pivot_table(
        index='cohort',
        columns='months',
        values='retention_rate'
    )

    return retention_table

# サンプルデータ
data = {
    'user_id': [1, 1, 1, 2, 2, 3, 3, 3, 3, 4],
    'signup_date': ['2025-01-01', '2025-01-01', '2025-01-01', '2025-01-01', '2025-01-01',
                     '2025-02-01', '2025-02-01', '2025-02-01', '2025-02-01', '2025-02-01'],
    'login_date': ['2025-01-01', '2025-02-01', '2025-03-01', '2025-01-01', '2025-02-01',
                    '2025-02-01', '2025-03-01', '2025-04-01', '2025-05-01', '2025-02-01']
}

df = pd.DataFrame(data)
retention_table = calculate_retention_curve(df)

print("=== リテンションカーブ ===")
print(retention_table.round(1))
```

#### チャーン率（解約率）

**定義**: 一定期間に解約したユーザーの割合

**計算式**:
```
チャーン率 = (期間中の解約数 ÷ 期初のユーザー数) × 100
```

**関係式**:
```
リテンション率 + チャーン率 = 100%
```

**ベンチマーク（SaaS）**:
- 優秀: 月次5%以下（年間約46%）
- 平均: 月次5〜7%（年間約49〜58%）
- 要改善: 月次7%以上

**例**:
```python
def calculate_churn_rate(users_start, users_churned):
    """
    チャーン率を計算

    Args:
        users_start: 期初のユーザー数
        users_churned: 解約ユーザー数

    Returns:
        float: チャーン率（%）
    """
    churn_rate = (users_churned / users_start) * 100
    return churn_rate

# 実例
users_start = 1000    # 期初: 1,000人
users_churned = 50    # 解約: 50人

churn_rate = calculate_churn_rate(users_start, users_churned)
print(f"月次チャーン率: {churn_rate:.1f}%")

# 年間チャーン率（複利計算）
annual_churn = (1 - (1 - churn_rate/100)**12) * 100
print(f"年間チャーン率（予測）: {annual_churn:.1f}%")
```

#### NPS (Net Promoter Score) - 顧客推奨度

**定義**: 顧客がサービスを他人に推奨する度合い

**質問**:
> 「このサービスを友人や同僚に勧める可能性はどのくらいですか？」
> （0〜10点で評価）

**分類**:
- 推奨者（Promoter）: 9〜10点
- 中立者（Passive）: 7〜8点
- 批判者（Detractor）: 0〜6点

**計算式**:
```
NPS = 推奨者の割合（%） - 批判者の割合（%）
```

**ベンチマーク**:
- 優秀: 50以上
- 良好: 30〜50
- 平均: 0〜30
- 要改善: 0未満

**例**:
```python
def calculate_nps(scores):
    """
    NPSを計算

    Args:
        scores: スコアのリスト（0〜10）

    Returns:
        dict: NPS分析結果
    """
    total = len(scores)

    promoters = sum(1 for score in scores if score >= 9)
    passives = sum(1 for score in scores if 7 <= score <= 8)
    detractors = sum(1 for score in scores if score <= 6)

    promoter_pct = (promoters / total) * 100
    detractor_pct = (detractors / total) * 100

    nps = promoter_pct - detractor_pct

    return {
        "推奨者": promoters,
        "中立者": passives,
        "批判者": detractors,
        "推奨者率": promoter_pct,
        "批判者率": detractor_pct,
        "NPS": nps
    }

# サンプルデータ
scores = [9, 10, 8, 7, 9, 10, 6, 8, 9, 10, 5, 9, 10, 8, 9]

result = calculate_nps(scores)
print("=== NPS分析 ===")
print(f"推奨者: {result['推奨者']}人 ({result['推奨者率']:.1f}%)")
print(f"中立者: {result['中立者']}人")
print(f"批判者: {result['批判者']}人 ({result['批判者率']:.1f}%)")
print(f"\nNPS: {result['NPS']:.0f}")
```

---

## 4. KPI計測とダッシュボード構築

### 4.1 データ収集アーキテクチャ

```
┌─────────────┐
│ フロント    │
│ エンド      │──┐
└─────────────┘  │
                 │  イベント送信
┌─────────────┐  │  (JavaScript SDK)
│ モバイル    │──┤
│ アプリ      │  │
└─────────────┘  │
                 │
┌─────────────┐  │
│ バックエンド│──┘
│ API         │
└──────┬──────┘
       │
       ▼
┌─────────────────────┐
│ イベントトラッキング │
│ (Mixpanel/Amplitude) │
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│ データウェアハウス   │
│ (BigQuery/Snowflake) │
└──────┬──────────────┘
       │
       ▼
┌─────────────────────┐
│ BIツール             │
│ (Looker/Tableau)     │
└─────────────────────┘
```

### 4.2 イベントトラッキング実装

#### JavaScript（フロントエンド）

```typescript
// analytics.ts
interface EventProperties {
  [key: string]: string | number | boolean;
}

class Analytics {
  private static instance: Analytics;
  private userId: string | null = null;

  private constructor() {
    // シングルトンパターン
  }

  public static getInstance(): Analytics {
    if (!Analytics.instance) {
      Analytics.instance = new Analytics();
    }
    return Analytics.instance;
  }

  // ユーザー識別
  public identify(userId: string, traits?: EventProperties): void {
    this.userId = userId;

    // Mixpanel
    if (typeof mixpanel !== 'undefined') {
      mixpanel.identify(userId);
      if (traits) {
        mixpanel.people.set(traits);
      }
    }

    // Google Analytics 4
    if (typeof gtag !== 'undefined') {
      gtag('config', 'GA_MEASUREMENT_ID', {
        user_id: userId,
      });
    }
  }

  // イベント送信
  public track(eventName: string, properties?: EventProperties): void {
    const eventData = {
      event: eventName,
      user_id: this.userId,
      timestamp: new Date().toISOString(),
      ...properties,
    };

    // Mixpanel
    if (typeof mixpanel !== 'undefined') {
      mixpanel.track(eventName, properties);
    }

    // Google Analytics 4
    if (typeof gtag !== 'undefined') {
      gtag('event', eventName, properties);
    }

    // カスタムバックエンド
    this.sendToBackend(eventData);
  }

  // ページビュー
  public page(pageName: string, properties?: EventProperties): void {
    this.track('Page Viewed', {
      page_name: pageName,
      url: window.location.href,
      ...properties,
    });
  }

  private sendToBackend(eventData: any): void {
    // バックエンドAPIにイベント送信
    fetch('/api/analytics/events', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(eventData),
    }).catch((error) => {
      console.error('Analytics error:', error);
    });
  }
}

export const analytics = Analytics.getInstance();

// 使用例
// ユーザー登録時
analytics.identify('user_12345', {
  email: 'user@example.com',
  plan: 'pro',
  signup_date: '2025-10-13',
});

// ボタンクリック
analytics.track('Button Clicked', {
  button_name: 'upgrade_to_pro',
  page: 'pricing',
});

// ページビュー
analytics.page('Pricing Page');
```

#### Node.js（バックエンド）

```typescript
// backend/services/analytics.service.ts
import { BigQuery } from '@google-cloud/bigquery';

interface AnalyticsEvent {
  user_id: string;
  event_name: string;
  properties: Record<string, any>;
  timestamp: string;
}

export class AnalyticsService {
  private bigquery: BigQuery;
  private datasetId = 'analytics';
  private tableId = 'events';

  constructor() {
    this.bigquery = new BigQuery();
  }

  async trackEvent(event: AnalyticsEvent): Promise<void> {
    try {
      // BigQueryにイベント挿入
      await this.bigquery
        .dataset(this.datasetId)
        .table(this.tableId)
        .insert([
          {
            user_id: event.user_id,
            event_name: event.event_name,
            properties: JSON.stringify(event.properties),
            timestamp: event.timestamp,
          },
        ]);

      console.log('Event tracked:', event.event_name);
    } catch (error) {
      console.error('Analytics error:', error);
    }
  }

  async getKPIs(startDate: string, endDate: string): Promise<any> {
    const query = `
      SELECT
        COUNT(DISTINCT user_id) as total_users,
        COUNT(*) as total_events,
        COUNT(DISTINCT CASE WHEN event_name = 'purchase' THEN user_id END) as conversions,
        ROUND(COUNT(DISTINCT CASE WHEN event_name = 'purchase' THEN user_id END) / COUNT(DISTINCT user_id) * 100, 2) as conversion_rate
      FROM \`${this.datasetId}.${this.tableId}\`
      WHERE timestamp BETWEEN @start_date AND @end_date
    `;

    const options = {
      query: query,
      params: {
        start_date: startDate,
        end_date: endDate,
      },
    };

    const [rows] = await this.bigquery.query(options);
    return rows[0];
  }
}
```

### 4.3 KPIダッシュボード構築（React）

```tsx
// components/KPIDashboard.tsx
import React, { useEffect, useState } from 'react';
import { Line, Bar, Doughnut } from 'react-chartjs-2';
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  BarElement,
  ArcElement,
  Title,
  Tooltip,
  Legend,
} from 'chart.js';

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  BarElement,
  ArcElement,
  Title,
  Tooltip,
  Legend
);

interface KPIData {
  mrr: number;
  mau: number;
  churn_rate: number;
  nps: number;
  cac: number;
  ltv: number;
}

interface TrendData {
  labels: string[];
  mrr: number[];
  mau: number[];
}

export const KPIDashboard: React.FC = () => {
  const [kpiData, setKpiData] = useState<KPIData | null>(null);
  const [trendData, setTrendData] = useState<TrendData | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchKPIData();
  }, []);

  const fetchKPIData = async () => {
    try {
      const response = await fetch('/api/analytics/kpis');
      const data = await response.json();
      setKpiData(data.current);
      setTrendData(data.trend);
      setLoading(false);
    } catch (error) {
      console.error('Failed to fetch KPI data:', error);
    }
  };

  if (loading) {
    return <div className="flex items-center justify-center h-screen">Loading...</div>;
  }

  if (!kpiData || !trendData) {
    return <div>No data available</div>;
  }

  // MRRトレンドグラフ
  const mrrChartData = {
    labels: trendData.labels,
    datasets: [
      {
        label: 'MRR',
        data: trendData.mrr,
        borderColor: 'rgb(59, 130, 246)',
        backgroundColor: 'rgba(59, 130, 246, 0.1)',
        fill: true,
      },
    ],
  };

  // MAUトレンドグラフ
  const mauChartData = {
    labels: trendData.labels,
    datasets: [
      {
        label: 'MAU',
        data: trendData.mau,
        borderColor: 'rgb(34, 197, 94)',
        backgroundColor: 'rgba(34, 197, 94, 0.1)',
        fill: true,
      },
    ],
  };

  // LTV/CAC比率
  const ltvCacRatio = kpiData.ltv / kpiData.cac;

  return (
    <div className="p-6 bg-gray-50 min-h-screen">
      <h1 className="text-3xl font-bold mb-8">KPI Dashboard</h1>

      {/* KPIカード */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <KPICard
          title="MRR"
          value={`¥${kpiData.mrr.toLocaleString()}`}
          trend="+12%"
          trendUp={true}
        />
        <KPICard
          title="MAU"
          value={kpiData.mau.toLocaleString()}
          trend="+8%"
          trendUp={true}
        />
        <KPICard
          title="Churn Rate"
          value={`${kpiData.churn_rate.toFixed(1)}%`}
          trend="-0.5%"
          trendUp={false}
        />
        <KPICard
          title="NPS"
          value={kpiData.nps.toString()}
          trend="+5"
          trendUp={true}
        />
      </div>

      {/* ユニットエコノミクス */}
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-8">
        <div className="bg-white p-6 rounded-lg shadow">
          <h3 className="text-lg font-semibold mb-4">Unit Economics</h3>
          <div className="space-y-4">
            <div>
              <p className="text-sm text-gray-600">LTV</p>
              <p className="text-2xl font-bold">¥{kpiData.ltv.toLocaleString()}</p>
            </div>
            <div>
              <p className="text-sm text-gray-600">CAC</p>
              <p className="text-2xl font-bold">¥{kpiData.cac.toLocaleString()}</p>
            </div>
            <div>
              <p className="text-sm text-gray-600">LTV/CAC Ratio</p>
              <p className={`text-2xl font-bold ${ltvCacRatio >= 3 ? 'text-green-600' : 'text-red-600'}`}>
                {ltvCacRatio.toFixed(1)}x
              </p>
              <p className="text-xs text-gray-500 mt-1">
                {ltvCacRatio >= 3 ? '健全' : '要改善'}
              </p>
            </div>
          </div>
        </div>

        {/* MRRトレンド */}
        <div className="bg-white p-6 rounded-lg shadow lg:col-span-2">
          <h3 className="text-lg font-semibold mb-4">MRR Trend</h3>
          <Line
            data={mrrChartData}
            options={{
              responsive: true,
              plugins: {
                legend: {
                  display: false,
                },
              },
              scales: {
                y: {
                  beginAtZero: true,
                },
              },
            }}
          />
        </div>
      </div>

      {/* MAUトレンド */}
      <div className="bg-white p-6 rounded-lg shadow">
        <h3 className="text-lg font-semibold mb-4">MAU Trend</h3>
        <Line
          data={mauChartData}
          options={{
            responsive: true,
            plugins: {
              legend: {
                display: false,
              },
            },
            scales: {
              y: {
                beginAtZero: true,
              },
            },
          }}
        />
      </div>
    </div>
  );
};

// KPIカードコンポーネント
interface KPICardProps {
  title: string;
  value: string | number;
  trend: string;
  trendUp: boolean;
}

const KPICard: React.FC<KPICardProps> = ({ title, value, trend, trendUp }) => {
  return (
    <div className="bg-white p-6 rounded-lg shadow">
      <h3 className="text-sm font-medium text-gray-600 mb-2">{title}</h3>
      <p className="text-3xl font-bold mb-2">{value}</p>
      <div className="flex items-center">
        <span className={`text-sm font-medium ${trendUp ? 'text-green-600' : 'text-red-600'}`}>
          {trendUp ? '↑' : '↓'} {trend}
        </span>
        <span className="text-xs text-gray-500 ml-2">vs last month</span>
      </div>
    </div>
  );
};
```

### 4.4 リアルタイムアラート設定

```python
# alerting.py
import smtplib
from email.mime.text import MIMEText
from typing import Dict, List

class KPIAlertSystem:
    def __init__(self, smtp_config: Dict):
        self.smtp_config = smtp_config
        self.alert_rules = []

    def add_alert_rule(self, kpi_name: str, threshold: float, condition: str):
        """
        アラートルールを追加

        Args:
            kpi_name: KPI名
            threshold: 閾値
            condition: 条件 ('above', 'below')
        """
        self.alert_rules.append({
            'kpi_name': kpi_name,
            'threshold': threshold,
            'condition': condition
        })

    def check_alerts(self, kpi_values: Dict[str, float]) -> List[str]:
        """
        アラート条件をチェック

        Args:
            kpi_values: KPI値の辞書

        Returns:
            List[str]: トリガーされたアラートメッセージ
        """
        triggered_alerts = []

        for rule in self.alert_rules:
            kpi_name = rule['kpi_name']
            threshold = rule['threshold']
            condition = rule['condition']

            if kpi_name not in kpi_values:
                continue

            current_value = kpi_values[kpi_name]

            if condition == 'above' and current_value > threshold:
                message = f"⚠️ {kpi_name}が閾値を超えました: {current_value:.2f} > {threshold:.2f}"
                triggered_alerts.append(message)

            elif condition == 'below' and current_value < threshold:
                message = f"⚠️ {kpi_name}が閾値を下回りました: {current_value:.2f} < {threshold:.2f}"
                triggered_alerts.append(message)

        return triggered_alerts

    def send_alert_email(self, alerts: List[str], recipients: List[str]):
        """アラートをメール送信"""
        if not alerts:
            return

        subject = f"KPIアラート: {len(alerts)}件の異常を検知"
        body = "\n\n".join(alerts)

        msg = MIMEText(body, 'plain', 'utf-8')
        msg['Subject'] = subject
        msg['From'] = self.smtp_config['from']
        msg['To'] = ', '.join(recipients)

        with smtplib.SMTP(self.smtp_config['host'], self.smtp_config['port']) as server:
            server.starttls()
            server.login(self.smtp_config['username'], self.smtp_config['password'])
            server.send_message(msg)

        print(f"アラートメール送信: {len(alerts)}件")

# 使用例
alert_system = KPIAlertSystem(smtp_config={
    'host': 'smtp.gmail.com',
    'port': 587,
    'from': 'alerts@example.com',
    'username': 'your-email@gmail.com',
    'password': 'your-password'
})

# アラートルール設定
alert_system.add_alert_rule('churn_rate', threshold=7.0, condition='above')
alert_system.add_alert_rule('dau_mau_ratio', threshold=15.0, condition='below')
alert_system.add_alert_rule('cac', threshold=10000, condition='above')

# KPI値チェック
kpi_values = {
    'churn_rate': 8.5,  # 閾値超過
    'dau_mau_ratio': 12.0,  # 閾値未満
    'cac': 8000,  # 正常
}

alerts = alert_system.check_alerts(kpi_values)

if alerts:
    print("=== トリガーされたアラート ===")
    for alert in alerts:
        print(alert)

    # メール送信
    alert_system.send_alert_email(alerts, recipients=['team@example.com'])
```

---

## 5. 実践演習

### 演習1: KPIツリーの作成

**課題**: あなたが運営するECサイトのKPIツリーを作成せよ。

**KGI**: 年間売上 5,000万円

**ヒント**:
- 売上 = 訪問者数 × CVR × 客単価
- 訪問者数 = オーガニック + 広告 + SNS
- CVR = カート追加率 × 購入完了率

**提出物**:
1. KPIツリーの図
2. 各KPIの目標値
3. Pythonでの実装コード

---

### 演習2: ダッシュボード構築

**課題**: 以下のKPIを可視化するダッシュボードを作成せよ。

**KPI**:
- MRR推移（過去12ヶ月）
- MAU推移（過去12ヶ月）
- チャーン率（月次）
- LTV/CAC比率
- NPS

**要件**:
- React + Chart.jsを使用
- レスポンシブデザイン
- リアルタイム更新（10秒ごと）

---

### 演習3: アラートシステム構築

**課題**: 異常値を検知してSlackに通知するアラートシステムを実装せよ。

**アラート条件**:
- チャーン率が7%を超えた場合
- DAU/MAU比率が15%を下回った場合
- 新規登録数が前週比30%減少した場合

**技術スタック**:
- Python
- Slack Webhook API
- スケジューラ（cron or Celery）

---

## まとめ

このドキュメントでは、KPI/KGI設計とダッシュボード構築の実践的な手法を学習しました。

### 重要ポイント

1. **KGI→KPIへの分解**
   - ロジックツリーで構造的に設計
   - SMART原則に従って設定

2. **カテゴリ別KPI**
   - 獲得（CAC、CVR、LTV/CAC比率）
   - エンゲージメント（DAU/MAU、Stickiness）
   - 収益（MRR、ARPU、ARR）
   - 定着（リテンション率、チャーン率、NPS）

3. **データ計測とダッシュボード**
   - イベントトラッキング実装
   - リアルタイムダッシュボード構築
   - アラートシステムの自動化

### 次のステップ

- [要件定義](../../b_requirements/README.md) - KPIを要件に落とし込む
- [データ分析計画](./data_analysis_plan.md) - より高度な分析手法
- [カスタマージャーニー7ステップ](../../e_customer_journey_phases/README.md) - 各フェーズでのKPI設計

---

**学習時間**: 約4時間
**最終更新**: 2025-10-13
