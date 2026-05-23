<!-- TL;DR: Go 并发编程规范：Map 安全、Channel、Race Detector -->
---
name: go-concurrency
description: Go 并发编程规范 - Map 并发安全、Channel、Race Detector、Worker Pool、Context。用于 Go 并发开发时避免 race condition 和死锁。
---

# Go Concurrency - 并发编程规范

> **TL;DR**: Go 并发编程必须遵循安全规范，避免 race condition。

## 1. Map 并发安全（Issue #87 核心）

### ❌ 禁止：对 map 并发读写

```go
// 错误：map 不是线程安全的
var IPs = map[string]IP{}
var Users = map[string]User{}

// 并发读写会导致 panic
go func() { IPs["a"] = ip }()
go func() { _ = IPs["a"] }()  // RACE CONDITION!
```

### ✅ 正确：使用 sync.RWMutex

```go
import "sync"

var (
    ipMu sync.RWMutex
    IPs  = map[string]IP{}
)

func GetIP(id string) (IP, bool) {
    ipMu.RLock()
    defer ipMu.RUnlock()
    ip, ok := IPs[id]
    return ip, ok
}

func SetIP(id string, ip IP) {
    ipMu.Lock()
    defer ipMu.Unlock()
    IPs[id] = ip
}
```

### ✅ 替代：sync.Map

```go
import "sync"

var ips sync.Map

// 存储
ips.Store("key", value)

// 读取
if v, ok := ips.Load("key"); ok {
    // ...
}

// 读取或存储
v, _ := ips.LoadOrStore("key", defaultValue)
```

## 2. Channel 规范

### Channel 创建

```go
// 无缓冲 channel（同步）
ch := make(chan int)

// 有缓冲 channel（异步）
ch := make(chan int, 10)

// 关闭发送方
close(ch)

// 接收方检测关闭
v, ok := <-ch  // ok == false 表示已关闭
```

### Goroutine 泄漏

```go
// ❌ 错误：goroutine 泄漏
func leak() {
    ch := make(chan int)
    go func() {
        ch <- 1  // 永远不会被接收
    }()
}

// ✅ 正确：使用 context 或 Done channel
func withContext(ctx context.Context) {
    ch := make(chan int)
    go func() {
        select {
        case <-ctx.Done():
            return  // context 取消时退出
        case ch <- 1:
        }
    }()
}
```

## 3. WaitGroup 规范

```go
var wg sync.WaitGroup

for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()  // 必须调用
        // work
    }(i)
}

wg.Wait()  // 等待所有 goroutine 完成
```

## 4. Race Detector（必须使用）

### 开发时检测

```bash
# 运行测试时启用 race 检测
go test -race ./...

# 运行程序时启用 race 检测
go run -race main.go

# 构建时启用 race 检测
go build -race
```

### CI 必须包含

```yaml
# .github/workflows/go.yml
- name: Race Detector
  run: go test -race -count=1 ./...
```

## 5. Context 规范

```go
// ✅ 正确：context 作为第一个参数
func Process(ctx context.Context, req *Request) (*Response, error) {
    select {
    case <-ctx.Done():
        return nil, ctx.Err()
    default:
    }
    // ...
}

// ✅ 带超时的 context
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()
```

## 6. 常见错误

| 错误 | 正确做法 |
|------|---------|
| map 并发读写 | sync.RWMutex 或 sync.Map |
| goroutine 无退出 | 使用 context 取消 |
| defer 在循环内 | 每次迭代创建新函数 |
| select 无 default | 可能导致死锁 |
| 关闭已关闭的 channel | panic |

## 7. Worker Pool 模式

```go
func workerPool(ctx context.Context, workers int, jobs <-chan Job) <-chan Result {
    results := make(chan Result, len(jobs))

    var wg sync.WaitGroup
    for i := 0; i < workers; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for job := range jobs {
                select {
                case <-ctx.Done():
                    return
                case results <- process(job):
                }
            }
        }()
    }

    go func() {
        wg.Wait()
        close(results)
    }()

    return results
}
```

## 8. 最佳实践

| ✅ 做 | ❌ 不做 |
|------|---------|
| 使用 context 取消 | 使用 time.Sleep 同步 |
| 优先 Channel 通信 | 滥用共享内存 |
| defer wg.Done() | 忘记调用 Done() |
| go test -race | 忽略 race 检测 |
| 关闭发送方 channel | 关闭接收方 channel |
