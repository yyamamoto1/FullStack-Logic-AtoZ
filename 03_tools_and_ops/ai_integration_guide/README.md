# 🤖 AI統合活用ガイド
**GitHub Copilot & ChatGPT最大活用マニュアル**

---

## 📋 概要

このガイドでは、GitHub CopilotとChatGPTを開発ワークフローに統合し、生産性を最大化する方法を体系的に解説します。

---

## 🎯 学習目標

- GitHub Copilotの高度な活用テクニック習得
- ChatGPT APIを使った開発支援ツール構築
- AI駆動開発ワークフローの設計と実装
- プロンプトエンジニアリングの実践
- チーム開発でのAI統合ベストプラクティス

---

## 📚 コンテンツ構成

### 1. GitHub Copilot活用編
- [基本設定と最適化](./github-copilot/setup.md)
- [高度なプロンプトテクニック](./github-copilot/advanced-prompts.md)
- [プロジェクト固有の設定](./github-copilot/project-config.md)
- [チーム統合と設定共有](./github-copilot/team-integration.md)

### 2. ChatGPT活用編
- [開発支援ツール構築](./chatgpt/dev-tools.md)
- [コードレビュー自動化](./chatgpt/code-review.md)
- [ドキュメント自動生成](./chatgpt/documentation.md)
- [テスト生成とTDD支援](./chatgpt/testing.md)

### 3. 統合ワークフロー編
- [CI/CD統合](./workflows/cicd-integration.md)
- [VSCode拡張機能開発](./workflows/vscode-extension.md)
- [自動化スクリプト集](./workflows/automation-scripts.md)
- [品質管理システム](./workflows/quality-management.md)

### 4. プロンプトライブラリ
- [コード生成プロンプト](./prompts/code-generation.md)
- [レビュー・リファクタリング](./prompts/review-refactor.md)
- [アーキテクチャ設計](./prompts/architecture.md)
- [トラブルシューティング](./prompts/troubleshooting.md)

---

## 🚀 クイックスタート

### 1. 環境セットアップ

**必要なツール:**
```bash
# VSCode拡張機能
code --install-extension GitHub.copilot
code --install-extension GitHub.copilot-chat

# Node.js依存関係
npm install -g @ai-tools/cli
npm install openai
```

**設定ファイル作成:**
```bash
# プロジェクトルートに設定ファイルを作成
cp .ai-assistant-config.example.json .ai-assistant-config.json
```

### 2. 基本的な使用開始

**GitHub Copilot:**
```typescript
// ファイルの先頭にコンテキストコメントを追加
// Project: E-commerce Platform
// Framework: React + TypeScript
// Database: PostgreSQL
// Style: Functional Components with Hooks

// Function to fetch user profile data
const fetchUserProfile = async (userId: string) => {
    // Copilotが自動補完でコードを提案
};
```

**ChatGPT統合:**
```bash
# AI開発アシスタントCLIの使用
ai-dev generate component UserProfile --framework react --typescript
ai-dev review src/components/UserProfile.tsx
ai-dev test generate src/components/UserProfile.tsx
```

---

## 💡 主要機能

### GitHub Copilot最適化

#### 1. インテリジェントコンテキスト設定
```typescript
/**
 * PROJECT CONTEXT FOR GITHUB COPILOT
 * 
 * Project: FullStack E-commerce Platform
 * Tech Stack: React 18 + TypeScript + Next.js 14
 * Backend: Node.js + Express + PostgreSQL
 * State Management: Zustand
 * Styling: Tailwind CSS
 * Testing: Jest + React Testing Library
 * 
 * Code Style:
 * - Functional components with hooks
 * - Strict TypeScript
 * - Error boundaries
 * - Accessibility first
 * - Performance optimized
 */

// Component for displaying user order history
interface UserOrder {
    id: string;
    date: Date;
    total: number;
    status: 'pending' | 'shipped' | 'delivered';
    items: OrderItem[];
}

const UserOrderHistory: React.FC<{ userId: string }> = ({ userId }) => {
    // Copilotが型安全で最適化されたコードを提案
};
```

#### 2. プロンプトパターンライブラリ
```typescript
// PATTERN: API Route Handler with Validation
// Generate Express route with input validation, error handling, and OpenAPI docs
export async function POST(request: NextRequest) {
    // Copilotが完全なAPI実装を提案
}

// PATTERN: React Hook for Data Fetching
// Create custom hook with loading, error states, and cache management
export const useUserData = (userId: string) => {
    // CopilotがuseQuery patternで実装を提案
};

// PATTERN: Database Model with Relations
// Generate Prisma schema with proper relations and constraints
model User {
    // Copilotがフィールドとリレーションを提案
}
```

### ChatGPT開発支援ツール

#### 1. 自動コードレビューシステム
```typescript
import { OpenAI } from 'openai';

class AICodeReviewer {
    private openai: OpenAI;
    
    constructor() {
        this.openai = new OpenAI({
            apiKey: process.env.OPENAI_API_KEY
        });
    }

    async reviewCode(code: string, context: ReviewContext): Promise<ReviewResult> {
        const prompt = this.buildReviewPrompt(code, context);
        
        const completion = await this.openai.chat.completions.create({
            model: 'gpt-4-turbo-preview',
            messages: [
                {
                    role: 'system',
                    content: `あなたは経験豊富なシニアエンジニアです。
                    以下の観点でコードをレビューしてください：
                    - コード品質と可読性
                    - セキュリティ脆弱性
                    - パフォーマンス最適化
                    - ベストプラクティス遵守
                    - テスタビリティ`
                },
                {
                    role: 'user',
                    content: prompt
                }
            ],
            temperature: 0.3
        });

        return this.parseReviewResult(completion.choices[0].message.content!);
    }

    private buildReviewPrompt(code: string, context: ReviewContext): string {
        return `
プロジェクト: ${context.projectName}
言語/フレームワーク: ${context.framework}
ファイル: ${context.fileName}

レビュー対象コード:
\`\`\`${context.language}
${code}
\`\`\`

チーム規約:
${context.teamStandards.map(rule => `- ${rule}`).join('\n')}

詳細なレビューを提供してください。問題があれば具体的な修正案も含めてください。
        `;
    }
}
```

#### 2. インテリジェントテスト生成
```typescript
class AITestGenerator {
    async generateTests(sourceCode: string, testType: TestType): Promise<string> {
        const prompt = this.buildTestPrompt(sourceCode, testType);
        
        const completion = await this.openai.chat.completions.create({
            model: 'gpt-4-turbo-preview',
            messages: [
                {
                    role: 'system',
                    content: `テスト駆動開発の専門家として、
                    包括的で保守性の高いテストコードを生成してください。
                    
                    重視する点:
                    - エッジケースのカバレッジ
                    - モックとスタブの適切な使用
                    - テストの独立性
                    - 分かりやすいテスト名
                    - Given-When-Then パターン`
                },
                {
                    role: 'user',
                    content: prompt
                }
            ],
            temperature: 0.4
        });

        return completion.choices[0].message.content!;
    }

    private buildTestPrompt(sourceCode: string, testType: TestType): string {
        return `
以下のソースコードに対する${testType}テストを生成してください：

\`\`\`typescript
${sourceCode}
\`\`\`

生成するテスト:
1. 正常系のテスト（ハッピーパス）
2. 異常系のテスト（エラーケース）
3. 境界値のテスト
4. エッジケースのテスト

テストフレームワーク: Jest + React Testing Library
モッキング: MSW (Mock Service Worker)

テストコードには以下を含めてください:
- テストデータのセットアップ
- 適切なモック設定
- アサーション
- クリーンアップ処理
        `;
    }
}
```

### 高度な統合ワークフロー

#### 1. AI駆動CI/CDパイプライン
```yaml
# .github/workflows/ai-enhanced-ci.yml
name: AI Enhanced CI/CD

on:
  pull_request:
    types: [opened, synchronize]
  push:
    branches: [main, develop]

jobs:
  ai-code-analysis:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
      with:
        fetch-depth: 0

    - name: AI Code Quality Check
      env:
        OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
      run: |
        node scripts/ai-quality-check.js
        
    - name: AI Security Scan
      run: |
        node scripts/ai-security-scan.js
        
    - name: AI Performance Analysis
      run: |
        node scripts/ai-performance-check.js

    - name: Generate AI Test Recommendations
      run: |
        node scripts/ai-test-recommendations.js

    - name: Comment PR with AI Analysis
      uses: actions/github-script@v7
      with:
        script: |
          const fs = require('fs');
          const analysis = JSON.parse(fs.readFileSync('ai-analysis-results.json'));
          
          const comment = `## 🤖 AI Code Analysis
          
          **Quality Score:** ${analysis.qualityScore}/100
          **Security Score:** ${analysis.securityScore}/100
          **Performance Score:** ${analysis.performanceScore}/100
          
          ### 🔍 Key Findings
          ${analysis.findings.map(f => `- ${f}`).join('\n')}
          
          ### 📝 Recommendations
          ${analysis.recommendations.map(r => `- ${r}`).join('\n')}
          
          ### 🧪 Suggested Tests
          ${analysis.testSuggestions.map(t => `- ${t}`).join('\n')}
          `;
          
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: comment
          });
```

#### 2. リアルタイムAI開発支援
```typescript
// VSCode Extension: Real-time AI Assistant
class RealtimeAIAssistant {
    private statusBar: vscode.StatusBarItem;
    private aiService: AIService;

    constructor(context: vscode.ExtensionContext) {
        this.aiService = new AIService();
        this.setupRealtimeFeatures(context);
    }

    private setupRealtimeFeatures(context: vscode.ExtensionContext) {
        // リアルタイムコード分析
        vscode.workspace.onDidChangeTextDocument(async (event) => {
            if (this.shouldAnalyze(event)) {
                const analysis = await this.aiService.analyzeCode(
                    event.document.getText(),
                    event.document.languageId
                );
                this.showInlineHints(analysis);
            }
        });

        // インテリジェントオートコンプリート
        vscode.languages.registerCompletionItemProvider(
            { scheme: 'file', language: 'typescript' },
            {
                provideCompletionItems: async (document, position) => {
                    const context = this.buildContext(document, position);
                    const suggestions = await this.aiService.getSmartCompletions(context);
                    return suggestions.map(this.createCompletionItem);
                }
            }
        );

        // コード品質リアルタイム評価
        vscode.languages.registerCodeLensProvider(
            { scheme: 'file' },
            {
                provideCodeLenses: async (document) => {
                    const metrics = await this.aiService.getCodeMetrics(document.getText());
                    return this.createQualityLenses(metrics);
                }
            }
        );
    }

    private async showInlineHints(analysis: CodeAnalysis) {
        // インライン改善提案の表示
        for (const suggestion of analysis.suggestions) {
            const decoration = vscode.window.createTextEditorDecorationType({
                after: {
                    contentText: ` 💡 AI: ${suggestion.message}`,
                    color: '#888',
                    fontStyle: 'italic'
                }
            });

            vscode.window.activeTextEditor?.setDecorations(
                decoration,
                [new vscode.Range(suggestion.line, 0, suggestion.line, 0)]
            );
        }
    }
}
```

---

## 📖 プロンプトライブラリ

### コード生成プロンプト

#### React Component生成
```
役割: React TypeScript開発者
タスク: 指定された要件に基づいてReactコンポーネントを生成

要件:
- TypeScript strict mode対応
- Tailwind CSSでスタイリング
- アクセシビリティ対応（ARIA属性）
- エラーバウンダリ実装
- パフォーマンス最適化（memo, useMemo, useCallback）
- Storybookストーリー含む

コンポーネント名: {componentName}
機能要件: {requirements}
Props仕様: {propsSpec}

以下の形式で出力してください：
1. TypeScript インターフェース定義
2. メインコンポーネント実装
3. Storybookストーリー
4. 単体テスト
5. 使用例
```

#### API Route生成
```
役割: バックエンドAPI開発者
タスク: Express.js APIルートの実装

要件:
- TypeScript + Express
- 入力バリデーション（Zod）
- 認証・認可チェック
- エラーハンドリング
- OpenAPI仕様書
- セキュリティ対策（レート制限、CORS）
- ログ出力

エンドポイント: {method} {path}
機能: {functionality}
入力スキーマ: {inputSchema}
出力スキーマ: {outputSchema}

実装してください：
1. ルートハンドラー
2. バリデーションスキーマ
3. エラーハンドリング
4. OpenAPI仕様
5. 単体テスト
```

### レビュー・リファクタリングプロンプト

#### 包括的コードレビュー
```
役割: シニアソフトウェアエンジニア
タスク: 提供されたコードの包括的レビュー

評価観点:
1. **機能性**: 仕様通りに動作するか
2. **可読性**: コードが理解しやすいか
3. **保守性**: 将来の変更に対応しやすいか
4. **パフォーマンス**: 効率的な実装か
5. **セキュリティ**: 脆弱性はないか
6. **テスタビリティ**: テストしやすい設計か

コード:
```{language}
{code}
```

チーム規約:
{teamStandards}

以下の形式でレビュー結果を提供してください：
- 総合スコア: X/100
- 重大な問題: [問題点と修正提案]
- 改善提案: [具体的な改善案]
- 良い点: [評価できる実装]
- セキュリティチェック: [セキュリティ関連の懸念]
- パフォーマンス最適化: [最適化の提案]
```

#### リファクタリング提案
```
役割: リファクタリング専門家
タスク: コードの改善提案と実装

現在のコード:
```{language}
{originalCode}
```

リファクタリング目標:
- {goals}

制約:
- 既存機能を破壊しない
- APIの互換性を維持
- パフォーマンスを改善
- 可読性を向上

提供してください:
1. リファクタリング計画
2. 改善されたコード
3. 変更点の説明
4. マイグレーション手順
5. テスト戦略
```

---

## 🔧 設定とカスタマイズ

### プロジェクト設定ファイル

**.ai-assistant-config.json:**
```json
{
  "project": {
    "name": "FullStack E-commerce Platform",
    "type": "web-application",
    "framework": {
      "frontend": "React + TypeScript + Next.js",
      "backend": "Node.js + Express + PostgreSQL",
      "testing": "Jest + React Testing Library",
      "styling": "Tailwind CSS"
    }
  },
  "ai": {
    "providers": {
      "openai": {
        "model": "gpt-4-turbo-preview",
        "maxTokens": 2000,
        "temperature": 0.3
      },
      "copilot": {
        "enabled": true,
        "customPrompts": true
      }
    },
    "features": {
      "codeGeneration": true,
      "codeReview": true,
      "testGeneration": true,
      "documentation": true,
      "refactoring": true
    }
  },
  "team": {
    "codingStandards": {
      "language": "TypeScript",
      "indentation": "spaces",
      "indentSize": 2,
      "maxLineLength": 100,
      "requireJSDoc": true
    },
    "reviewGuidelines": [
      "セキュリティファースト",
      "パフォーマンス重視",
      "アクセシビリティ対応",
      "テストカバレッジ80%以上"
    ]
  },
  "automation": {
    "preCommitReview": true,
    "prAnalysis": true,
    "autoDocGeneration": true,
    "qualityGates": {
      "codeQuality": 85,
      "security": 90,
      "performance": 80
    }
  }
}
```

### VSCode設定

**settings.json:**
```json
{
  "github.copilot.enable": {
    "*": true,
    "yaml": false,
    "plaintext": false
  },
  "github.copilot.advanced": {
    "secret_key": "github_pat_xxx",
    "length": 500,
    "temperature": 0.3,
    "top_p": 1,
    "stop": ["\n\n", "\n\r\n", "\r\n\r\n"]
  },
  "aiDevAssistant.enabled": true,
  "aiDevAssistant.openaiApiKey": "${OPENAI_API_KEY}",
  "aiDevAssistant.autoReview": true,
  "aiDevAssistant.inlineHints": true,
  "aiDevAssistant.teamConfig": "./.ai-assistant-config.json"
}
```

---

## 📊 効果測定とKPI

### 開発生産性指標

```typescript
interface ProductivityMetrics {
    codeGenerationTime: {
        before: number;  // AI導入前の平均時間（分）
        after: number;   // AI導入後の平均時間（分）
        improvement: number; // 改善率（%）
    };
    
    bugDetectionRate: {
        aiDetected: number;     // AI検出バグ数
        totalBugs: number;      // 総バグ数
        detectionRate: number;  // 検出率（%）
    };
    
    codeQuality: {
        maintainabilityIndex: number;
        cyclomaticComplexity: number;
        testCoverage: number;
    };
    
    teamEfficiency: {
        reviewTime: number;        // レビュー時間（時間）
        reworkRate: number;        // やり直し率（%）
        knowledgeSharing: number;  // ナレッジ共有指数
    };
}

// メトリクス計測システム
class ProductivityTracker {
    async measureCodeGenerationTime(task: DevelopmentTask): Promise<number> {
        const startTime = Date.now();
        // AI支援による開発タスク実行
        await this.executeWithAI(task);
        return Date.now() - startTime;
    }

    async analyzeBugDetection(): Promise<BugDetectionMetrics> {
        const aiDetectedBugs = await this.getAIDetectedBugs();
        const totalBugs = await this.getTotalBugs();
        
        return {
            aiDetected: aiDetectedBugs.length,
            totalBugs: totalBugs.length,
            detectionRate: (aiDetectedBugs.length / totalBugs.length) * 100,
            severity: this.categorizeBugsBySeverity(aiDetectedBugs)
        };
    }
}
```

---

## 🎯 ベストプラクティス

### 1. プロンプト設計の原則
- **具体性**: 曖昧な指示ではなく、具体的な要件を明示
- **コンテキスト**: プロジェクトの技術スタックと制約を含める
- **段階的詳細化**: 複雑なタスクは小さなステップに分割
- **検証可能性**: 結果を評価できる明確な基準を設定

### 2. AI統合のガイドライン
- **人間中心設計**: AIは支援ツール、最終判断は人間が行う
- **品質保証**: AI生成コードも通常のレビュープロセスを経る
- **継続学習**: チームの知見をAIシステムにフィードバック
- **セキュリティ重視**: 機密情報をAIサービスに送信しない

### 3. チーム開発での活用
- **標準化**: チーム共通のAI活用ルールを策定
- **知識共有**: 有効なプロンプトパターンを共有
- **品質基準**: AI支援下でも一定の品質基準を維持
- **継続改善**: 定期的にAI活用効果を評価・改善

---

## 🔗 関連リソース

### 公式ドキュメント
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [OpenAI API Reference](https://platform.openai.com/docs)
- [VSCode Extension API](https://code.visualstudio.com/api)

### 学習リソース
- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [AI-Powered Development](https://www.oreilly.com/library/view/ai-powered-development/)
- [GitHub Copilot Best Practices](https://github.blog/2023-06-20-how-to-write-better-prompts-for-github-copilot/)

---

**最終更新**: 2025-10-22  
**担当**: AI Engineer #9  
**次回更新**: 新機能・ベストプラクティス追加時