<!-- TL;DR: 现代 Go 开发：Go 1.21+、Generics、性能优化、微服务、Observability -->
---
name: go-modern
description: 现代 Go 开发 - Go 1.21+ 特性、Generics、性能优化、微服务架构、Observability。用于现代 Go 项目开发和架构设计。
---

# Go Modern - 现代 Go 开发

> **TL;DR**: 使用 Go 1.21+ 现代特性，构建高性能微服务。

## 1. Go 1.21+ 新特性

### Generics（泛型）

```go
// 泛型函数
func Map[T, U](s []T, fn func(T) U) []U {
    result := make([]U, len(s))
    for i, v := range s {
        result[i] = fn(v)
    }
    return result
}

// 使用
numbers := []int{1, 2, 3}
strings := Map(numbers, strconv.Itoa)  // []string{"1", "2", "3"}

// 泛型类型
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() (T, bool) {
    if len(s.items) == 0 {
        var zero T
        return zero, false
    }
    item := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return item, true
}
```

### slog  structured logging

```go
import "log/slog"

// 使用 slog（Go 1.21+）
logger := slog.Default()

// 结构化日志
logger.Info("request completed",
    "method", "GET",
    "path", "/api/users",
    "status", 200,
    "duration_ms", 45,
)

// 带错误
logger.Error("request failed",
    "error", err,
    "path", "/api/users",
)
```

### range over func（Go 1.23+）

```go
// 迭代函数
func Backward[E any](s []E) func(yield func(E) bool) {
    return func(yield func(E) bool) {
        for i := len(s) - 1; i >= 0; i-- {
            if !yield(s[i]) {
                return
            }
        }
    }
}

// 使用
for v := range Backward([]int{1, 2, 3}) {
    fmt.Println(v)  // 3, 2, 1
}
```

## 2. 性能优化

### Benchmark 驱动优化

```go
import "testing"

func BenchmarkStringConcat(b *testing.B) {
    for i := 0; i < b.N; i++ {
        result := ""
        for j := 0; j < 100; j++ {
            result += "a"
        }
    }
}

// 运行
// go test -bench=BenchmarkStringConcat -benchmem
```

### pprof 性能分析

```go
import (
    "net/http"
    _ "net/http/pprof"
)

// 在 main.go 中添加
// go install golang.org/x/tools/cmd/pprof@latest
// pprof -http=:8080 http://localhost:6060/debug/pprof/heap
```

### 内存池化

```go
import "sync"

var bufPool = sync.Pool{
    New: func() interface{} {
        b := make([]byte, 1024)
        return &b
    },
}

// 使用
buf := bufPool.Get().(*[]byte)
defer bufPool.Put(buf)
```

## 3. 微服务架构

### Clean Architecture

```
├── cmd/
│   └── server/
│       └── main.go
├── internal/
│   ├── domain/           # 领域层（实体、值对象）
│   │   ├── user.go
│   │   └── order.go
│   ├── usecase/         # 用例层（业务逻辑）
│   │   ├── create_user.go
│   │   └── order_usecase.go
│   ├── repository/      # 接口定义
│   │   └── user_repository.go
│   └── infrastructure/   # 基础设施（实现）
│       ├── postgres/
│       └── redis/
├── pkg/
│   └── middleware/
└── api/
    └── handlers/
```

### 依赖注入

```go
// 使用 wire 或手动 DI
type Server struct {
    userRepo  UserRepository
    orderUse *usecase.OrderUseCase
    logger   *slog.Logger
}

func NewServer(cfg *Config, repos *Repositories) *Server {
    return &Server{
        userRepo:  repos.UserRepo,
        orderUse: usecase.NewOrderUseCase(repos.OrderRepo),
        logger:   slog.Default(),
    }
}
```

## 4. API 设计

### RESTful

```go
// 使用 net/http 或 gin/fiber
type UserHandler struct {
    useCase *usecase.UserUseCase
}

func (h *UserHandler) RegisterRoutes(r *gin.RouterGroup) {
    users := r.Group("/users")
    {
        users.POST("", h.Create)
        users.GET("/:id", h.GetByID)
        users.PUT("/:id", h.Update)
        users.DELETE("/:id", h.Delete)
    }
}

func (h *UserHandler) Create(c *gin.Context) {
    var req CreateUserRequest
    if err := c.ShouldBindJSON(&req); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    // ...
}
```

### gRPC

```protobuf
syntax = "proto3";

service UserService {
    rpc CreateUser(CreateUserRequest) returns (User);
    rpc GetUser(GetUserRequest) returns (User);
}

message CreateUserRequest {
    string name = 1;
    string email = 2;
}
```

## 5. Observability

### OpenTelemetry

```go
import (
    "go.opentelemetry.io/otel"
    "go.opentelemetry.io/otel/exporters/otlp/otlptrace/otlptracegrpc"
    "go.opentelemetry.io/otel/sdk/trace"
)

func initTracer() *trace.TracerProvider {
    exporter, _ := otlptracegrpc.New(context.Background())
    tp := trace.NewTracerProvider(
        trace.WithBatcher(exporter),
    )
    otel.SetTracerProvider(tp)
    return tp
}
```

### Health Checks

```go
type HealthHandler struct{}

func (h *HealthHandler) RegisterRoutes(r *gin.RouterGroup) {
    r.GET("/health", h.Health)
    r.GET("/ready", h.Ready)
}

func (h *HealthHandler) Health(c *gin.Context) {
    c.JSON(200, gin.H{"status": "healthy"})
}

func (h *HealthHandler) Ready(c *gin.Context) {
    // 检查依赖
    if !db.IsReady() {
        c.JSON(503, gin.H{"status": "not ready"})
        return
    }
    c.JSON(200, gin.H{"status": "ready"})
}
```

### Metrics with Prometheus

```go
import "github.com/prometheus/client_golang/prometheus"
import "github.com/prometheus/client_golang/prometheus/promhttp"

var (
    httpRequestsTotal = prometheus.NewCounterVec(
        prometheus.CounterOpts{
            Name: "http_requests_total",
            Help: "Total HTTP requests",
        },
        []string{"method", "path", "status"},
    )
)

func init() {
    prometheus.MustRegister(httpRequestsTotal)
}
```

## 6. 错误处理最佳实践

```go
// 使用 errors.New 和 fmt.Errorf
var ErrNotFound = errors.New("resource not found")

func GetUser(id string) (*User, error) {
    user, err := repo.Find(id)
    if err != nil {
        if errors.Is(err, sql.ErrNoRows) {
            return nil, ErrNotFound
        }
        return nil, fmt.Errorf("finding user: %w", err)
    }
    return user, nil
}

// 业务错误类型
type BusinessError struct {
    Code    string
    Message string
}

func (e *BusinessError) Error() string {
    return e.Message
}
```

## 7. Testing Best Practices

### 使用 testify

```go
import (
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/mock"
)

// Mock
type MockUserRepo struct {
    mock.Mock
}

func (m *MockUserRepo) Find(id string) (*User, error) {
    args := m.Called(id)
    return args.Get(0).(*User), args.Error(1)
}

// Test
func TestUserService_GetUser(t *testing.T) {
    mockRepo := new(MockUserRepo)
    svc := NewUserService(mockRepo)

    mockRepo.On("Find", "123").Return(&User{ID: "123"}, nil)

    user, err := svc.GetUser("123")

    assert.NoError(t, err)
    assert.Equal(t, "123", user.ID)
    mockRepo.AssertExpectations(t)
}
```

### Integration Testing with Testcontainers

```go
import "github.com/testcontainers/testcontainers-go"

func TestWithDB(t *testing.T) {
    req := testcontainers.ContainerRequest{
        Image: "postgres:15",
        ExposedPorts: []string{"5432/tcp"},
        Env: map[string]string{
            "POSTGRES_PASSWORD": "test",
        },
    }
    // ... setup container
}
```

## 8. CI/CD

```yaml
# .github/workflows/go.yml
name: Go
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: '1.23'
      - name: Lint
        run: golangci-lint run
      - name: Test
        run: go test -race -count=1 ./...
      - name: Build
        run: go build -o bin/server ./cmd/server
```
