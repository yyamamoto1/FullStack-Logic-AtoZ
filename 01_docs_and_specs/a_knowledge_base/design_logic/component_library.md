# コンポーネントライブラリ設計

再利用可能で拡張性の高いUIコンポーネントライブラリの設計思想と実装パターンを学びます。

---

## 📑 目次

1. [コンポーネント設計の4原則](#コンポーネント設計の4原則)
2. [基本コンポーネント](#基本コンポーネント)
3. [コンポーネントパターン](#コンポーネントパターン)
4. [コンポーネントAPI設計](#コンポーネントapi設計)
5. [スタイリング戦略](#スタイリング戦略)
6. [ドキュメントとStorybook](#ドキュメントとstorybook)
7. [実践: コンポーネントライブラリを作る](#実践コンポーネントライブラリを作る)

---

## コンポーネント設計の4原則

### 1. Reusability（再利用性）

**定義:** 様々な場所で繰り返し使える

✅ **良い例:**
```jsx
// 汎用的なButton
<Button variant="primary">保存</Button>
<Button variant="secondary">キャンセル</Button>
<Button variant="danger">削除</Button>
<Button size="small">編集</Button>
<Button size="large" fullWidth>送信</Button>
```

❌ **悪い例:**
```jsx
// 用途が限定的
<SaveButton />
<CancelButton />
<DeleteButton />
// → 新しいボタンが必要になるたびにコンポーネントを作る必要がある
```

---

### 2. Composability（合成可能性）

**定義:** 小さなコンポーネントを組み合わせて大きなコンポーネントを作れる

✅ **良い例:**
```jsx
// 柔軟に組み合わせ可能
<Card>
  <CardHeader>
    <CardTitle>タイトル</CardTitle>
    <CardActions>
      <Button>編集</Button>
      <Button>削除</Button>
    </CardActions>
  </CardHeader>
  <CardContent>
    <p>コンテンツ</p>
  </CardContent>
  <CardFooter>
    <Button>もっと見る</Button>
  </CardFooter>
</Card>
```

❌ **悪い例:**
```jsx
// すべてをpropsで制御（柔軟性がない）
<Card
  title="タイトル"
  content="コンテンツ"
  showEditButton
  showDeleteButton
  showMoreButton
/>
```

---

### 3. Encapsulation（カプセル化）

**定義:** 内部実装を隠蔽し、外部からは単純なAPIで操作できる

✅ **良い例:**
```jsx
// 内部の複雑性を隠蔽
<DatePicker
  value={date}
  onChange={setDate}
  minDate={new Date()}
  format="YYYY-MM-DD"
/>
// ユーザーは内部のカレンダーロジックを気にしなくてよい
```

---

### 4. Extensibility（拡張性）

**定義:** 新しい機能を追加しやすい

✅ **良い例:**
```jsx
// カスタムスタイルを追加可能
<Button
  className="custom-style"
  onClick={handleClick}
  leftIcon={<SaveIcon />}
  rightIcon={<ArrowIcon />}
>
  保存
</Button>
```

---

## 基本コンポーネント

### 1. Button

**バリエーション:**
- Variant: primary, secondary, tertiary, danger, ghost
- Size: small, medium, large
- State: default, hover, active, disabled, loading

**実装:**

```tsx
// Button.tsx
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'tertiary' | 'danger' | 'ghost';
  size?: 'small' | 'medium' | 'large';
  fullWidth?: boolean;
  loading?: boolean;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  children: React.ReactNode;
}

export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'medium',
  fullWidth = false,
  loading = false,
  leftIcon,
  rightIcon,
  children,
  disabled,
  className,
  ...props
}) => {
  return (
    <button
      className={cn(
        'button',
        `button--${variant}`,
        `button--${size}`,
        fullWidth && 'button--full-width',
        loading && 'button--loading',
        className
      )}
      disabled={disabled || loading}
      {...props}
    >
      {loading && <Spinner className="button__spinner" />}
      {!loading && leftIcon && (
        <span className="button__icon button__icon--left">{leftIcon}</span>
      )}
      <span className="button__text">{children}</span>
      {!loading && rightIcon && (
        <span className="button__icon button__icon--right">{rightIcon}</span>
      )}
    </button>
  );
};
```

**スタイル:**
```css
/* Button.css */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  border: none;
  border-radius: var(--radius-md);
  font-weight: var(--font-weight-semibold);
  cursor: pointer;
  transition: all var(--transition-normal);
  position: relative;
}

/* Variants */
.button--primary {
  background: var(--color-primary);
  color: white;
}

.button--primary:hover:not(:disabled) {
  background: var(--color-primary-dark);
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

.button--secondary {
  background: var(--color-gray-200);
  color: var(--color-gray-900);
}

.button--danger {
  background: var(--color-danger);
  color: white;
}

/* Sizes */
.button--small {
  padding: 6px 12px;
  font-size: var(--font-size-sm);
}

.button--medium {
  padding: 10px 20px;
  font-size: var(--font-size-md);
}

.button--large {
  padding: 14px 28px;
  font-size: var(--font-size-lg);
}

/* States */
.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.button--loading {
  pointer-events: none;
}

.button--full-width {
  width: 100%;
}
```

---

### 2. Input

**バリエーション:**
- Type: text, email, password, number, search
- Size: small, medium, large
- State: default, focus, error, disabled

**実装:**

```tsx
// Input.tsx
interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  error?: string;
  helperText?: string;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  fullWidth?: boolean;
}

export const Input: React.FC<InputProps> = ({
  label,
  error,
  helperText,
  leftIcon,
  rightIcon,
  fullWidth = false,
  className,
  id,
  ...props
}) => {
  const inputId = id || `input-${Math.random()}`;

  return (
    <div className={cn('input-wrapper', fullWidth && 'input-wrapper--full-width')}>
      {label && (
        <label htmlFor={inputId} className="input__label">
          {label}
          {props.required && <span className="input__required">*</span>}
        </label>
      )}

      <div className={cn('input-container', error && 'input-container--error')}>
        {leftIcon && <span className="input__icon input__icon--left">{leftIcon}</span>}
        <input
          id={inputId}
          className={cn('input', className)}
          aria-invalid={error ? 'true' : 'false'}
          aria-describedby={error ? `${inputId}-error` : helperText ? `${inputId}-helper` : undefined}
          {...props}
        />
        {rightIcon && <span className="input__icon input__icon--right">{rightIcon}</span>}
      </div>

      {error && (
        <span id={`${inputId}-error`} className="input__error" role="alert">
          {error}
        </span>
      )}

      {!error && helperText && (
        <span id={`${inputId}-helper`} className="input__helper-text">
          {helperText}
        </span>
      )}
    </div>
  );
};
```

---

### 3. Modal

**実装:**

```tsx
// Modal.tsx
interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title?: string;
  children: React.ReactNode;
  footer?: React.ReactNode;
  size?: 'small' | 'medium' | 'large' | 'full';
  closeOnOverlayClick?: boolean;
}

export const Modal: React.FC<ModalProps> = ({
  isOpen,
  onClose,
  title,
  children,
  footer,
  size = 'medium',
  closeOnOverlayClick = true,
}) => {
  useEffect(() => {
    if (isOpen) {
      document.body.style.overflow = 'hidden';
    } else {
      document.body.style.overflow = 'unset';
    }
    return () => {
      document.body.style.overflow = 'unset';
    };
  }, [isOpen]);

  if (!isOpen) return null;

  return createPortal(
    <>
      <div
        className="modal-overlay"
        onClick={closeOnOverlayClick ? onClose : undefined}
        role="presentation"
      />
      <div className={cn('modal', `modal--${size}`)} role="dialog" aria-modal="true">
        <div className="modal__header">
          {title && <h2 className="modal__title">{title}</h2>}
          <button
            className="modal__close"
            onClick={onClose}
            aria-label="閉じる"
          >
            <CloseIcon />
          </button>
        </div>

        <div className="modal__content">{children}</div>

        {footer && <div className="modal__footer">{footer}</div>}
      </div>
    </>,
    document.body
  );
};
```

---

## コンポーネントパターン

### 1. Compound Components

**目的:** 関連するコンポーネントを密結合させる

```tsx
// Tabs.tsx
interface TabsContextValue {
  activeTab: string;
  setActiveTab: (tab: string) => void;
}

const TabsContext = React.createContext<TabsContextValue | undefined>(undefined);

export const Tabs: React.FC<{ defaultTab: string; children: React.ReactNode }> = ({
  defaultTab,
  children,
}) => {
  const [activeTab, setActiveTab] = useState(defaultTab);

  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className="tabs">{children}</div>
    </TabsContext.Provider>
  );
};

export const TabList: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  return <div className="tabs__list" role="tablist">{children}</div>;
};

export const Tab: React.FC<{ value: string; children: React.ReactNode }> = ({
  value,
  children,
}) => {
  const context = useContext(TabsContext);
  if (!context) throw new Error('Tab must be used within Tabs');

  const { activeTab, setActiveTab } = context;
  const isActive = activeTab === value;

  return (
    <button
      className={cn('tabs__tab', isActive && 'tabs__tab--active')}
      onClick={() => setActiveTab(value)}
      role="tab"
      aria-selected={isActive}
    >
      {children}
    </button>
  );
};

export const TabPanel: React.FC<{ value: string; children: React.ReactNode }> = ({
  value,
  children,
}) => {
  const context = useContext(TabsContext);
  if (!context) throw new Error('TabPanel must be used within Tabs');

  const { activeTab } = context;
  if (activeTab !== value) return null;

  return (
    <div className="tabs__panel" role="tabpanel">
      {children}
    </div>
  );
};

// 使用例
<Tabs defaultTab="tab1">
  <TabList>
    <Tab value="tab1">タブ1</Tab>
    <Tab value="tab2">タブ2</Tab>
    <Tab value="tab3">タブ3</Tab>
  </TabList>

  <TabPanel value="tab1">
    <p>タブ1のコンテンツ</p>
  </TabPanel>
  <TabPanel value="tab2">
    <p>タブ2のコンテンツ</p>
  </TabPanel>
  <TabPanel value="tab3">
    <p>タブ3のコンテンツ</p>
  </TabPanel>
</Tabs>
```

---

### 2. Render Props Pattern

```tsx
// DataFetcher.tsx
interface DataFetcherProps<T> {
  url: string;
  children: (data: {
    data: T | null;
    loading: boolean;
    error: Error | null;
    refetch: () => void;
  }) => React.ReactNode;
}

export function DataFetcher<T>({ url, children }: DataFetcherProps<T>) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    try {
      const response = await fetch(url);
      const json = await response.json();
      setData(json);
      setError(null);
    } catch (err) {
      setError(err as Error);
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return <>{children({ data, loading, error, refetch: fetchData })}</>;
}

// 使用例
<DataFetcher<User[]> url="/api/users">
  {({ data, loading, error, refetch }) => {
    if (loading) return <Spinner />;
    if (error) return <ErrorMessage error={error} />;
    return (
      <>
        <UserList users={data} />
        <Button onClick={refetch}>再読み込み</Button>
      </>
    );
  }}
</DataFetcher>
```

---

### 3. Hooks Pattern

```tsx
// useDisclosure.ts
export const useDisclosure = (initialState = false) => {
  const [isOpen, setIsOpen] = useState(initialState);

  const open = useCallback(() => setIsOpen(true), []);
  const close = useCallback(() => setIsOpen(false), []);
  const toggle = useCallback(() => setIsOpen((prev) => !prev), []);

  return { isOpen, open, close, toggle };
};

// 使用例
function MyComponent() {
  const modal = useDisclosure();

  return (
    <>
      <Button onClick={modal.open}>モーダルを開く</Button>
      <Modal isOpen={modal.isOpen} onClose={modal.close}>
        <p>モーダルのコンテンツ</p>
      </Modal>
    </>
  );
}
```

---

## コンポーネントAPI設計

### 良いAPIの原則

1. **直感的** - 使い方が明白
2. **一貫性** - 似たコンポーネントは似たAPI
3. **柔軟性** - 様々なユースケースに対応
4. **型安全** - TypeScriptで型チェック

### 例: Dropdown

```tsx
// ❌ 悪い例: APIが直感的でない
<Dropdown
  config={{
    items: [{ id: 1, text: 'Item 1' }],
    onSelect: (item) => {},
    trigger: 'click',
  }}
/>

// ✅ 良い例: APIが直感的
<Dropdown trigger={<Button>メニュー</Button>}>
  <DropdownItem onClick={() => {}}>アイテム1</DropdownItem>
  <DropdownItem onClick={() => {}}>アイテム2</DropdownItem>
  <DropdownDivider />
  <DropdownItem onClick={() => {}}>アイテム3</DropdownItem>
</Dropdown>
```

---

## スタイリング戦略

### 1. CSS Modules

```tsx
// Button.module.css
.button {
  /* スタイル */
}

.button--primary {
  /* スタイル */
}

// Button.tsx
import styles from './Button.module.css';

export const Button = ({ variant }) => (
  <button className={cn(styles.button, styles[`button--${variant}`])}>
    ...
  </button>
);
```

### 2. Styled Components

```tsx
import styled from 'styled-components';

const StyledButton = styled.button<{ variant: string }>`
  padding: 10px 20px;
  background: ${(props) =>
    props.variant === 'primary' ? 'blue' : 'gray'};
  color: white;
  border: none;
  border-radius: 4px;

  &:hover {
    opacity: 0.8;
  }
`;

export const Button = ({ variant = 'primary', children }) => (
  <StyledButton variant={variant}>{children}</StyledButton>
);
```

### 3. Tailwind CSS

```tsx
export const Button = ({ variant = 'primary', children }) => {
  const baseClasses = 'px-5 py-2 rounded font-semibold transition';
  const variantClasses = {
    primary: 'bg-blue-500 text-white hover:bg-blue-600',
    secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
  };

  return (
    <button className={cn(baseClasses, variantClasses[variant])}>
      {children}
    </button>
  );
};
```

---

## ドキュメントとStorybook

### Storybook セットアップ

```bash
npx storybook@latest init
```

### Story作成

```tsx
// Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './Button';

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'danger'],
    },
    size: {
      control: 'select',
      options: ['small', 'medium', 'large'],
    },
  },
};

export default meta;
type Story = StoryObj<typeof Button>;

export const Primary: Story = {
  args: {
    variant: 'primary',
    children: 'Button',
  },
};

export const Secondary: Story = {
  args: {
    variant: 'secondary',
    children: 'Button',
  },
};

export const WithIcon: Story = {
  args: {
    variant: 'primary',
    leftIcon: <SaveIcon />,
    children: '保存',
  },
};

export const Loading: Story = {
  args: {
    variant: 'primary',
    loading: true,
    children: '読み込み中',
  },
};
```

---

## 実践: コンポーネントライブラリを作る

### プロジェクト構成

```
/my-component-library
├── src/
│   ├── components/
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.module.css
│   │   │   ├── Button.stories.tsx
│   │   │   ├── Button.test.tsx
│   │   │   └── index.ts
│   │   ├── Input/
│   │   └── Modal/
│   ├── utils/
│   │   └── cn.ts  # クラス名結合ユーティリティ
│   ├── tokens/
│   │   └── design-tokens.css
│   └── index.ts  # エクスポート
├── package.json
└── tsconfig.json
```

### package.json

```json
{
  "name": "@your-org/components",
  "version": "1.0.0",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc",
    "storybook": "storybook dev -p 6006",
    "build-storybook": "storybook build"
  },
  "peerDependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  }
}
```

---

**次のステップ:** [マーケティングロジック](../marketing_logic/principles.md)を学ぶ

**最終更新**: 2025-10-13
