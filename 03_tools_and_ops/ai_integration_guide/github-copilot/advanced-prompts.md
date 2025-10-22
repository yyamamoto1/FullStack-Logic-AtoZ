# 🚀 GitHub Copilot 高度なプロンプトテクニック

## 📋 概要

GitHub Copilotを最大限活用するための高度なプロンプトエンジニアリング技法を解説します。

---

## 🎯 コンテキスト最適化戦略

### 1. プロジェクト情報の効果的な伝達

```typescript
/**
 * GITHUB COPILOT CONTEXT ENHANCEMENT
 * 
 * Project: Next.js E-commerce Platform
 * Version: Next.js 14 + React 18 + TypeScript 5
 * State Management: Zustand + React Query
 * Styling: Tailwind CSS + Headless UI
 * Database: PostgreSQL + Prisma ORM
 * Authentication: NextAuth.js + JWT
 * Payment: Stripe + Webhook handlers
 * Testing: Jest + React Testing Library + Playwright
 * 
 * Architecture Patterns:
 * - Server Components + Client Components (App Router)
 * - API Routes with Route Handlers
 * - Middleware for auth/logging
 * - Edge functions for geo-specific content
 * 
 * Code Conventions:
 * - Functional components with TypeScript
 * - Custom hooks for business logic
 * - Error boundaries for graceful failures
 * - Consistent file naming (kebab-case)
 * - Barrel exports for clean imports
 */
```

### 2. ファイル固有のコンテキスト設定

```typescript
/**
 * FILE CONTEXT: Product Management Component
 * 
 * Purpose: Admin interface for managing e-commerce products
 * Features: CRUD operations, bulk actions, image upload
 * Dependencies: React Hook Form, Zod validation, React Query
 * State: Local form state + global product cache
 * 
 * Related Files:
 * - types/product.ts (Product interfaces)
 * - api/products.ts (API client functions)
 * - hooks/useProducts.ts (Custom hooks)
 * - components/ui/ (Reusable UI components)
 */

import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { productSchema, type Product } from '@/types/product';

// Admin component for managing product catalog with bulk operations
interface ProductManagerProps {
  initialProducts?: Product[];
  onProductUpdate?: (product: Product) => void;
}

export const ProductManager: React.FC<ProductManagerProps> = ({
  initialProducts,
  onProductUpdate
}) => {
  // Copilot will generate comprehensive product management implementation
  const form = useForm<Product>({
    resolver: zodResolver(productSchema),
    defaultValues: {
      // Copilot suggests appropriate default values
    }
  });

  // Bulk operations for managing multiple products
  const handleBulkAction = async (action: 'delete' | 'activate' | 'deactivate', productIds: string[]) => {
    // Copilot generates optimized bulk operation logic
  };

  return (
    // Copilot creates comprehensive UI with proper accessibility
  );
};
```

---

## 🔧 パターン別プロンプト技法

### React Hooks パターン

```typescript
// PATTERN: Custom Hook for API Data Management
// Creates a reusable hook with loading states, error handling, and caching
export const useEntityData = <T>(
  entityType: string,
  id: string,
  options?: QueryOptions
) => {
  // Copilot generates:
  // - React Query integration
  // - Loading/error states
  // - Optimistic updates
  // - Cache invalidation
  // - TypeScript generic support
};

// PATTERN: Form Hook with Validation
// Generates form management with validation, submission, and error handling
export const useValidatedForm = <TSchema extends ZodSchema>(
  schema: TSchema,
  onSubmit: (data: z.infer<TSchema>) => Promise<void>
) => {
  // Copilot creates:
  // - React Hook Form setup
  // - Zod validation integration
  // - Submission handling
  // - Field error management
  // - Form reset functionality
};
```

### API Route パターン

```typescript
// PATTERN: Next.js App Router API Handler
// Creates type-safe API route with validation, auth, and error handling
import { NextRequest, NextResponse } from 'next/server';
import { z } from 'zod';

// POST /api/products - Create new product with image upload
const createProductSchema = z.object({
  name: z.string().min(1).max(100),
  description: z.string().max(1000),
  price: z.number().positive(),
  categoryId: z.string().uuid(),
  images: z.array(z.string().url()).max(5)
});

export async function POST(request: NextRequest) {
  // Copilot generates:
  // - Request validation
  // - Authentication check
  // - Database transaction
  // - Error handling
  // - Response formatting
  // - Rate limiting
}

// PATTERN: Database Operation with Prisma
// Generates optimized database queries with proper relations
const getProductsWithDetails = async (filters: ProductFilters) => {
  // Copilot creates:
  // - Prisma query optimization
  // - Relation includes
  // - Filtering logic
  // - Pagination
  // - Error handling
};
```

### Component Architecture パターン

```typescript
// PATTERN: Compound Component with Context
// Creates flexible component API with shared state
interface TabsContextValue {
  activeTab: string;
  setActiveTab: (tab: string) => void;
  orientation: 'horizontal' | 'vertical';
}

// Compound tabs component with accessibility
export const Tabs = {
  Root: ({ children, defaultTab, orientation = 'horizontal' }: TabsRootProps) => {
    // Copilot generates context provider with keyboard navigation
  },
  
  List: ({ children, className }: TabsListProps) => {
    // Copilot creates tab list with ARIA attributes
  },
  
  Tab: ({ value, children, disabled }: TabProps) => {
    // Copilot implements tab button with focus management
  },
  
  Panel: ({ value, children }: TabPanelProps) => {
    // Copilot creates panel with proper ARIA associations
  }
};

// Usage example that guides Copilot
const ExampleUsage = () => (
  <Tabs.Root defaultTab="overview">
    <Tabs.List>
      <Tabs.Tab value="overview">Overview</Tabs.Tab>
      <Tabs.Tab value="details">Details</Tabs.Tab>
    </Tabs.List>
    <Tabs.Panel value="overview">
      {/* Copilot suggests appropriate content */}
    </Tabs.Panel>
  </Tabs.Root>
);
```

---

## 🧪 テスト駆動開発プロンプト

### Unit Test パターン

```typescript
// PATTERN: React Component Testing
// Generates comprehensive test suite with user interactions
import { render, screen, userEvent } from '@testing-library/react';
import { vi } from 'vitest';

// Test for ProductCard component with user interactions
describe('ProductCard', () => {
  const mockProduct = {
    id: '1',
    name: 'Test Product',
    price: 99.99,
    image: '/test-image.jpg',
    inStock: true
  };

  // Test rendering and user interactions
  it('should handle add to cart with loading state', async () => {
    const mockAddToCart = vi.fn().mockResolvedValue(undefined);
    
    // Copilot generates:
    // - Component rendering
    // - User interaction simulation
    // - Loading state verification
    // - Success state validation
    // - Error handling test
  });

  // Test accessibility features
  it('should be accessible with keyboard navigation', async () => {
    // Copilot creates:
    // - Keyboard event simulation
    // - ARIA attribute checks
    // - Focus management tests
    // - Screen reader compatibility
  });
});
```

### Integration Test パターン

```typescript
// PATTERN: API Integration Testing
// Creates end-to-end API test with database setup
import { test, expect } from '@playwright/test';

// E2E test for product management workflow
test.describe('Product Management', () => {
  test.beforeEach(async ({ page }) => {
    // Setup test data and authentication
    await setupTestDatabase();
    await authenticateAsAdmin(page);
  });

  test('should create, edit, and delete product successfully', async ({ page }) => {
    // Copilot generates:
    // - Navigation to product page
    // - Form filling automation
    // - Image upload testing
    // - Success message verification
    // - Database state validation
  });

  test('should handle bulk operations on multiple products', async ({ page }) => {
    // Copilot creates:
    // - Multiple product selection
    // - Bulk action execution
    // - Loading state handling
    // - Result verification
    // - Error scenario testing
  });
});
```

---

## 🔒 セキュリティパターン

### Authentication & Authorization

```typescript
// PATTERN: Secure API Route with Role-Based Access
// Generates API handler with authentication and authorization checks
import { auth } from '@/lib/auth';
import { hasPermission } from '@/lib/permissions';

// DELETE /api/admin/products/[id] - Admin-only product deletion
export async function DELETE(
  request: NextRequest,
  { params }: { params: { id: string } }
) {
  // Copilot generates:
  // - Session validation
  // - Role-based authorization
  // - Input sanitization
  // - Audit logging
  // - Secure error responses
  try {
    const session = await auth(request);
    if (!session || !hasPermission(session.user, 'products:delete')) {
      // Copilot suggests appropriate error response
    }

    // Copilot implements secure deletion logic
  } catch (error) {
    // Copilot handles errors without exposing sensitive info
  }
}

// PATTERN: Input Validation and Sanitization
// Creates comprehensive input validation with security measures
const validateAndSanitizeInput = (input: unknown) => {
  // Copilot generates:
  // - XSS prevention
  // - SQL injection prevention
  // - Data type validation
  // - Length limits
  // - Character filtering
};
```

### Data Protection

```typescript
// PATTERN: Sensitive Data Handling
// Implements secure handling of personal information
interface UserProfileUpdate {
  email?: string;
  phone?: string;
  address?: Address;
}

const updateUserProfile = async (userId: string, updates: UserProfileUpdate) => {
  // Copilot generates:
  // - PII encryption
  // - Data minimization
  // - Audit trail creation
  // - GDPR compliance checks
  // - Secure database updates
};

// PATTERN: API Key Management
// Secure handling of third-party API integrations
const callExternalAPI = async (endpoint: string, data: any) => {
  // Copilot creates:
  // - Environment variable usage
  // - API key rotation handling
  // - Request signing
  // - Rate limiting
  // - Error masking
};
```

---

## 🚀 Performance Optimization プロンプト

### React Performance

```typescript
// PATTERN: Optimized React Component
// Generates performance-optimized component with memoization
import { memo, useMemo, useCallback } from 'react';

interface ProductListProps {
  products: Product[];
  filters: ProductFilters;
  onProductSelect: (product: Product) => void;
}

// Memoized product list with virtualization for large datasets
export const ProductList = memo<ProductListProps>(({
  products,
  filters,
  onProductSelect
}) => {
  // Copilot generates:
  // - useMemo for expensive computations
  // - useCallback for event handlers
  // - Virtual scrolling implementation
  // - Lazy loading for images
  // - Optimistic UI updates
  
  const filteredProducts = useMemo(() => {
    // Copilot creates optimized filtering logic
  }, [products, filters]);

  const handleProductClick = useCallback((product: Product) => {
    // Copilot implements optimized event handling
  }, [onProductSelect]);

  return (
    // Copilot suggests virtual scrolling or pagination
  );
});
```

### Database Optimization

```typescript
// PATTERN: Optimized Database Queries
// Creates efficient Prisma queries with proper indexing
const getProductsWithOptimization = async (params: ProductQueryParams) => {
  // Copilot generates:
  // - Selective field inclusion
  // - Relation optimization
  // - Query batching
  // - Index usage hints
  // - Pagination with cursors
  
  return prisma.product.findMany({
    // Copilot suggests optimal query structure
    select: {
      // Only necessary fields
    },
    where: {
      // Indexed fields for filtering
    },
    include: {
      // Minimal necessary relations
    },
    take: params.limit,
    skip: params.offset,
    orderBy: {
      // Indexed sorting fields
    }
  });
};

// PATTERN: Caching Strategy
// Implements multi-layer caching for better performance
const getCachedProducts = async (cacheKey: string) => {
  // Copilot creates:
  // - Redis cache integration
  // - Cache invalidation logic
  // - Fallback to database
  // - Cache warming strategies
  // - TTL management
};
```

---

## 📊 モニタリングと分析

### Error Tracking

```typescript
// PATTERN: Comprehensive Error Handling
// Generates error tracking with detailed context
import { captureException, withScope } from '@sentry/nextjs';

const handleAPIError = (error: Error, context: ErrorContext) => {
  // Copilot generates:
  // - Error categorization
  // - Context enrichment
  // - User impact assessment
  // - Recovery strategies
  // - Alerting logic
  
  withScope((scope) => {
    // Copilot adds relevant context
    scope.setTag('operation', context.operation);
    scope.setLevel('error');
    scope.setContext('request', context.request);
    captureException(error);
  });
};

// PATTERN: Performance Monitoring
// Implements performance tracking and alerting
const trackPerformance = (operationName: string, duration: number) => {
  // Copilot creates:
  // - Metric collection
  // - Threshold monitoring
  // - Trend analysis
  // - Alert generation
  // - Dashboard integration
};
```

### Analytics Integration

```typescript
// PATTERN: User Behavior Tracking
// Generates privacy-compliant analytics implementation
const trackUserAction = (action: UserAction, properties: AnalyticsProperties) => {
  // Copilot generates:
  // - Consent verification
  // - Data anonymization
  // - Event queuing
  // - Batch sending
  // - Error resilience
};

// PATTERN: A/B Testing Framework
// Creates flexible experimentation system
const getExperimentVariant = (experimentId: string, userId: string) => {
  // Copilot implements:
  // - Consistent user bucketing
  // - Feature flag integration
  // - Conversion tracking
  // - Statistical significance
  // - Gradual rollout
};
```

---

## 🎨 UI/UX パターン

### Accessibility-First Components

```typescript
// PATTERN: Accessible Form Component
// Generates fully accessible form with ARIA support
interface AccessibleFormProps {
  fields: FormField[];
  onSubmit: (data: FormData) => void;
  validation: ValidationSchema;
}

export const AccessibleForm: React.FC<AccessibleFormProps> = ({
  fields,
  onSubmit,
  validation
}) => {
  // Copilot generates:
  // - ARIA labels and descriptions
  // - Keyboard navigation
  // - Screen reader announcements
  // - Error state management
  // - Focus management
  // - High contrast mode support
};

// PATTERN: Loading States and Skeletons
// Creates smooth loading experiences
const ProductCardSkeleton = () => {
  // Copilot generates:
  // - Skeleton screen layout
  // - Shimmer animations
  // - Proper dimensions
  // - Accessibility considerations
  // - Performance optimization
};
```

### Responsive Design

```typescript
// PATTERN: Mobile-First Responsive Component
// Generates responsive layout with touch optimization
const ResponsiveGrid: React.FC<GridProps> = ({ children, columns }) => {
  // Copilot creates:
  // - CSS Grid implementation
  // - Breakpoint management
  // - Touch gesture support
  // - Viewport optimization
  // - Performance considerations
  
  const gridStyles = useMemo(() => ({
    // Copilot suggests responsive grid styles
  }), [columns]);

  return (
    <div
      className="grid gap-4 transition-all duration-300"
      style={gridStyles}
    >
      {children}
    </div>
  );
};
```

---

## 💡 高度なテクニック

### Context-Aware Suggestions

```typescript
// TECHNIQUE: Chain of Thought Prompting
// Guides Copilot through complex problem-solving steps

/*
STEP 1: Analyze the requirements
- Need to implement real-time chat with typing indicators
- Must support multiple rooms and file uploads
- Requires message encryption and user presence

STEP 2: Design the architecture
- WebSocket connection management
- Message queue for reliability
- Encryption layer for security
- State synchronization across clients

STEP 3: Implementation strategy
- Start with connection management
- Add message handling
- Implement typing indicators
- Add file upload support
- Integrate encryption
*/

class RealtimeChatManager {
  // Copilot will follow the outlined steps to generate implementation
}
```

### Template-Based Generation

```typescript
// TECHNIQUE: Template Patterns
// Provides Copilot with reusable code templates

// TEMPLATE: CRUD Service
// Generate complete CRUD service for any entity
class EntityService<T extends BaseEntity> {
  constructor(
    private repository: Repository<T>,
    private validator: Validator<T>,
    private logger: Logger
  ) {}

  async create(data: CreateEntityData<T>): Promise<T> {
    // Copilot generates:
    // - Input validation
    // - Business logic
    // - Database operation
    // - Error handling
    // - Audit logging
  }

  async findById(id: string): Promise<T | null> {
    // Copilot implements optimized retrieval
  }

  async update(id: string, data: UpdateEntityData<T>): Promise<T> {
    // Copilot creates update logic with validation
  }

  async delete(id: string): Promise<void> {
    // Copilot implements safe deletion
  }

  async findMany(filters: EntityFilters<T>): Promise<PaginatedResult<T>> {
    // Copilot generates paginated queries
  }
}

// Usage example guides Copilot for specific implementations
const productService = new EntityService(productRepository, productValidator, logger);
const userService = new EntityService(userRepository, userValidator, logger);
```

---

## 📈 効果測定

### 生産性指標

```typescript
// GitHub Copilot効果測定システム
interface CopilotMetrics {
  suggestionsAccepted: number;
  suggestionsShown: number;
  acceptanceRate: number;
  timesSaved: number; // 分単位
  linesGenerated: number;
  bugsPreventedByAI: number;
}

const trackCopilotUsage = () => {
  // 使用状況の自動追跡
  // - 受け入れ率の測定
  // - 生成コードの品質評価
  // - 時間節約の計算
  // - エラー削減効果の測定
};
```

---

**最終更新**: 2025-10-22  
**担当**: AI Engineer #9  
**次回更新**: 新技術・パターン追加時