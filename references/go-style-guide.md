<!-- TL;DR: Go 风格指南：gofmt、错误处理、接口规范、包结构、测试 -->
---
name: go-style-guide
description: Go 风格指南 - gofmt 格式化、错误处理、接口设计、包结构、测试规范。用于 Go 开发时遵循 Go 惯用风格。
---

# Go Style Guide - Go 风格指南

> **TL;DR**: 遵循 Go 惯用风格，使用 gofmt 格式化，错误要显式处理。

## 1. 格式化（gofmt 必须使用）

```bash
# 格式化单个文件
gofmt -w file.go

# 格式化整个项目
gofmt -w .

# 使用 goimports 管理导入
goimports -w .
```

### gofmt 规则

- 使用 Tab 缩进
- 无尾随空格
- 大括号标准位置
- 标准化间距

## 2. 错误处理

### ❌ 禁止：忽略错误

```go
// 错误：忽略错误
data, _ := json.Marshal(obj)

// 正确：显式处理
data, err := json.Marshal(obj)
if err != nil {
    return fmt.Errorf("marshaling object: %w", err)
}
```

### 错误包装

```go
func processFile(path string) error {
    data, err := os.ReadFile(path)
    if err != nil {
        return fmt.Errorf("reading file %s: %w", path, err)
    }
    return nil
}

// 检查包装的错误
if errors.Is(err, os.ErrNotExist) {
    // 处理文件不存在
}

var validationErr *ValidationError
if errors.As(err, &validationErr) {
    // 处理验证错误
}
```

### 自定义错误类型

```go
// 哨兵错误
var (
    ErrNotFound     = errors.New("resource not found")
    ErrUnauthorized = errors.New("unauthorized access")
)

// 自定义错误类型
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation error on %s: %s", e.Field, e.Message)
}
```

## 3. 接口设计

### ✅ 小而专注的接口

```go
// 好：单方法接口
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// 组合接口
type ReadWriter interface {
    Reader
    Writer
}
```

### ❌ 禁止：大而全的接口

```go
// 错误：接口过大，难以实现和测试
type Repository interface {
    Find(id string) (*User, error)
    FindAll() ([]*User, error)
    Create(user *User) error
    Update(user *User) error
    Delete(id string) error
    // 方法太多
}
```

### 接受接口，返回结构体

```go
// 好：接受接口，返回具体类型
func NewUserService(repo UserRepository) *UserService {
    return &UserService{repo: repo}
}

// 接口由调用者定义
type UserRepository interface {
    Find(ctx context.Context, id string) (*User, error)
    Save(ctx context.Context, user *User) error
}
```

## 4. 包结构

### 标准项目布局

```
myproject/
├── cmd/
│   └── myapp/
│       └── main.go         # 应用入口
├── internal/
│   ├── auth/
│   │   ├── auth.go
│   │   └── auth_test.go
│   ├── user/
│   │   ├── user.go
│   │   ├── repository.go
│   │   └── service.go
│   └── config/
│       └── config.go
├── pkg/                    # 公共包（可选）
│   └── api/
│       └── client.go
├── go.mod
└── README.md
```

### 包命名

```go
// 包名：短小、 lowercase、无下划线
package user          // ✅ 好
package userService  // ❌ 不好
package user_service // ❌ 不好

// Package 注释
// Package user 提供用户管理功能。
package user
```

### 导入分组

```go
import (
    "context"
    "fmt"

    "github.com/google/uuid"
    "github.com/lib/pq"

    "myproject/internal/config"
)
```

## 5. 测试规范

### 测试文件组织

```go
// user_test.go - 同包测试
package user

import "testing"

func TestUserValidation(t *testing.T) {
    // 测试实现细节
}
```

### Table-Driven 测试

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"正数", 2, 3, 5},
        {"负数", -1, -1, -2},
        {"混合", -1, 5, 4},
        {"零", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### 测试辅助函数

```go
// Helper 应该调用 t.Helper()
func newTestUser(t *testing.T) *User {
    t.Helper()
    return &User{
        ID:    uuid.New().String(),
        Name:  "Test User",
        Email: "test@example.com",
    }
}

func assertNoError(t *testing.T, err error) {
    t.Helper()
    if err != nil {
        t.Fatalf("unexpected error: %v", err)
    }
}
```

## 6. Context 使用

```go
// ✅ 正确：context 作为第一个参数
func (s *Service) ProcessRequest(ctx context.Context, req *Request) (*Response, error) {
    select {
    case <-ctx.Done():
        return nil, ctx.Err()
    default:
    }
    // ...
}
```

## 7. Defer 清理

```go
func processFile(path string) error {
    file, err := os.Open(path)
    if err != nil {
        return err
    }
    defer file.Close()  // 函数返回时执行

    // 处理文件...
    return nil
}

// defer 按 LIFO 顺序执行
func transaction(db *sql.DB) error {
    tx, err := db.Begin()
    if err != nil {
        return err
    }
    defer tx.Rollback()  // 安全：提交后无操作

    // 工作...

    return tx.Commit()
}
```

## 8. Linting

```yaml
# .golangci.yml
linters:
  enable:
    - errcheck
    - govet
    - ineffassign
    - staticcheck
    - unused
    - gosimple
    - gofmt
    - goimports
```

## 9. 常用命令

```bash
# 格式化代码
go fmt ./...

# 运行 linter
golangci-lint run

# 运行测试
go test ./...

# 带覆盖率测试
go test -coverprofile=coverage.out ./...
go tool cover -html=coverage.out
```
