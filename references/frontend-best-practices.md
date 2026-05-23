<!-- TL;DR: 前端最佳实践：错误处理、Error Boundary、Toast、用户体验 -->
---
name: frontend-best-practices
description: 前端最佳实践 - 错误处理、用户体验、响应式设计。包含 Error Boundary、Toast 通知、统一错误处理等。用于前端开发时提升质量和用户体验。
---

# Frontend Best Practices - 前端最佳实践

> **TL;DR**: 前端开发必须遵循的最佳实践，提升用户体验和代码质量。

## 触发条件

- 开发前端页面
- 处理用户交互
- 涉及错误处理
- Code Review 自审时

---

## 1. 错误处理

### ❌ 禁止：使用 alert()

```javascript
// 错误：使用 alert 展示错误
function handleError(error) {
    alert("Error: " + error.message);
}
```

### ✅ 正确：使用 Toast

```javascript
// 正确：使用 Toast 通知
import { toast } from 'react-hot-toast';

function handleError(error) {
    toast.error(error.message || '操作失败，请重试');
}

function handleSuccess(message) {
    toast.success(message || '操作成功');
}
```

### React Toast 组件示例

```jsx
// ToastProvider.jsx
import { Toaster } from 'react-hot-toast';

export function ToastProvider({ children }) {
    return (
        <>
            <Toaster
                position="top-right"
                toastOptions={{
                    duration: 3000,
                    style: {
                        background: '#363636',
                        color: '#fff',
                    },
                    success: {
                        duration: 3000,
                        theme: {
                            primary: 'green',
                            secondary: 'black',
                        },
                    },
                    error: {
                        duration: 4000,
                        theme: {
                            primary: 'red',
                            secondary: 'black',
                        },
                    },
                }}
            />
            {children}
        </>
    );
}
```

---

## 2. Error Boundary

### ❌ 禁止：无 Error Boundary

```jsx
// 错误：没有错误边界，整个应用会崩溃
function App() {
    return <Dashboard />;
}
```

### ✅ 正确：使用 Error Boundary

```jsx
// ErrorBoundary.jsx
import { Component } from 'react';

class ErrorBoundary extends Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false, error: null };
    }

    static getDerivedStateFromError(error) {
        return { hasError: true, error };
    }

    componentDidCatch(error, errorInfo) {
        console.error('Error:', error, errorInfo);
        // 上报到监控服务
    }

    render() {
        if (this.state.hasError) {
            return this.props.fallback || (
                <div className="error-container">
                    <h2>出错了</h2>
                    <p>抱歉，页面发生了一些问题。</p>
                    <button onClick={() => window.location.reload()}>
                        刷新页面
                    </button>
                </div>
            );
        }
        return this.props.children;
    }
}

// 使用
function App() {
    return (
        <ErrorBoundary>
            <Dashboard />
        </ErrorBoundary>
    );
}
```

### Error Boundary 使用场景

| 场景 | 说明 |
|------|------|
| 页面级别 | 包裹整个页面组件 |
| 组件级别 | 包裹可能出错的独立组件 |
| 异步加载 | 包裹 Suspense 边界 |

---

## 3. 统一错误处理

### API 错误处理

```javascript
// api.js
export async function fetchData(url) {
    try {
        const response = await fetch(url);

        if (!response.ok) {
            const error = await response.json().catch(() => ({}));
            throw new Error(error.message || `HTTP ${response.status}`);
        }

        return await response.json();
    } catch (error) {
        if (error.name === 'TypeError') {
            throw new Error('网络连接失败，请检查网络');
        }
        throw error;
    }
}

// 组件中使用
function MyComponent() {
    const [data, setData] = useState(null);

    useEffect(() => {
        fetchData('/api/data')
            .then(setData)
            .catch(error => {
                toast.error(error.message);
            });
    }, []);

    return <div>{data}</div>;
}
```

### 表单验证错误

```jsx
// 表单错误处理
function LoginForm() {
    const [errors, setErrors] = useState({});

    function validate(values) {
        const newErrors = {};

        if (!values.email) {
            newErrors.email = '请输入邮箱';
        } else if (!/\S+@\S+\.\S+/.test(values.email)) {
            newErrors.email = '邮箱格式不正确';
        }

        if (!values.password) {
            newErrors.password = '请输入密码';
        } else if (values.password.length < 6) {
            newErrors.password = '密码至少6位';
        }

        setErrors(newErrors);
        return Object.keys(newErrors).length === 0;
    }

    function handleSubmit(e) {
        e.preventDefault();
        if (validate(values)) {
            // 提交
        }
    }

    return (
        <form>
            <input name="email" />
            {errors.email && <span className="error">{errors.email}</span>}

            <input name="password" type="password" />
            {errors.password && <span className="error">{errors.password}</span>}

            <button type="submit">登录</button>
        </form>
    );
}
```

---

## 4. 加载状态

### ✅ 加载状态设计

```jsx
// LoadingSpinner.jsx
export function LoadingSpinner({ text = '加载中...' }) {
    return (
        <div className="loading-container">
            <div className="spinner"></div>
            <p>{text}</p>
        </div>
    );
}

// Skeleton Loading
export function UserCardSkeleton() {
    return (
        <div className="skeleton-card">
            <div className="skeleton-avatar"></div>
            <div className="skeleton-text"></div>
            <div className="skeleton-text short"></div>
        </div>
    );
}
```

### 禁用状态

```jsx
// 按钮禁用状态
<button
    onClick={handleSubmit}
    disabled={isSubmitting}
>
    {isSubmitting ? '提交中...' : '提交'}
</button>
```

---

## 5. 响应式设计

### 断点设置

```css
/* CSS 断点 */
:root {
    --breakpoint-sm: 576px;
    --breakpoint-md: 768px;
    --breakpoint-lg: 992px;
    --breakpoint-xl: 1200px;
}

/* 移动优先 */
.container {
    padding: 0 16px;
}

@media (min-width: 768px) {
    .container {
        padding: 0 24px;
    }
}

@media (min-width: 1200px) {
    .container {
        max-width: 1200px;
        margin: 0 auto;
    }
}
```

### 移动端适配

```jsx
// 使用响应式组件
function App() {
    const [isMobile, setIsMobile] = useState(window.innerWidth < 768);

    useEffect(() => {
        const handleResize = () => setIsMobile(window.innerWidth < 768);
        window.addEventListener('resize', handleResize);
        return () => window.removeEventListener('resize', handleResize);
    }, []);

    return isMobile ? <MobileLayout /> : <DesktopLayout />;
}
```

---

## 6. 前端自审清单

| 检查项 | 说明 | 优先级 |
|--------|------|--------|
| 无 alert() | 使用 Toast 替代 | P0 |
| Error Boundary | 关键组件有错误边界 | P0 |
| 统一错误处理 | API 错误统一处理 | P1 |
| 加载状态 | 异步操作有 loading 状态 | P1 |
| 禁用状态 | 提交等操作有禁用状态 | P1 |
| 响应式 | 支持手机/平板/桌面 | P1 |

---

## 常见错误对照表

| 错误 | 正确做法 |
|------|---------|
| `alert("error")` | `toast.error("error")` |
| 无 Error Boundary | 用 `ErrorBoundary` 包裹组件 |
| 静默失败 | 所有错误必须反馈给用户 |
| `any` 类型 | 使用具体类型定义 |
