<!-- TL;DR: 安全编码：Secrets 管理、密码哈希、CORS、Token 黑名单 -->
---
name: security-coding
description: 安全编码规范 - 防止常见安全漏洞。包含 Secrets 管理、密码哈希、CORS 配置、Token 黑名单等。用于开发时避免安全缺陷。
---

# Security Coding - 安全编码规范

> **TL;DR**: 开发时必须遵循的安全编码规范，防止常见安全漏洞。

## 触发条件

- 开发新功能时
- 涉及用户认证、密码、Token
- 涉及 API、CORS、跨域
- Code Review 自审时

---

## 1. Secrets 管理（JWT Secret、API Key）

### ❌ 禁止

```go
// 错误：硬编码 secret
var jwtSecret = []byte("demo_ip_secret_key_2024")
```

```javascript
// 错误：硬编码 API Key
const API_KEY = "sk-xxxxxxx"
```

### ✅ 正确

```go
// 正确：从环境变量读取
jwtSecret = []byte(os.Getenv("JWT_SECRET"))

// 生成随机 secret
import "crypto/rand"
func GenerateSecret() ([]byte, error) {
    b := make([]byte, 32)
    _, err := rand.Read(b)
    return b, err
}
```

```javascript
// 正确：从环境变量读取
const API_KEY = process.env.API_KEY
```

### 检查清单

- [ ] 无硬编码密钥/密码/Token
- [ ] Secrets 从环境变量读取
- [ ] JWT secret 使用随机生成
- [ ] `.env` 文件在 `.gitignore` 中

---

## 2. 密码存储

### ❌ 禁止

```go
// 错误：明文存储
user.Password = "test123"
```

### ✅ 正确

```go
// 正确：使用 bcrypt 哈希
import "golang.org/x/crypto/bcrypt"

func HashPassword(password string) (string, error) {
    bytes, err := bcrypt.GenerateFromPassword([]byte(password), bcrypt.DefaultCost)
    return string(bytes), err
}

func CheckPassword(password, hash string) bool {
    err := bcrypt.CompareHashAndPassword([]byte(hash), []byte(password))
    return err == nil
}
```

```javascript
// JavaScript: 使用 bcrypt
const bcrypt = require('bcrypt');
const hash = await bcrypt.hash(password, 10);
const match = await bcrypt.compare(password, hash);
```

### 检查清单

- [ ] 密码使用 bcrypt/scrypt/argon2 哈希
- [ ] 不存储明文密码
- [ ] 登录时验证哈希

---

## 3. CORS 配置

### ❌ 禁止

```go
// 错误：允许所有来源（生产环境）
c.Header("Access-Control-Allow-Origin", "*")
```

### ✅ 正确

```go
// 正确：限制具体域名
allowedOrigins := []string{
    "https://example.com",
    "https://app.example.com",
}

corsConfig := cors.New(cors.Options{
    AllowedOrigins:   allowedOrigins,
    AllowedMethods:   []string{"GET", "POST", "PUT", "DELETE"},
    AllowedHeaders:   []string{"Authorization", "Content-Type"},
    AllowCredentials: true,
})
```

```javascript
// Node.js: 限制域名
const corsOptions = {
    origin: function (origin, callback) {
        if (allowedOrigins.indexOf(origin) !== -1 || !origin) {
            callback(null, true)
        } else {
            callback(new Error('Not allowed by CORS'))
        }
    }
}
```

### 检查清单

- [ ] 生产环境 CORS 不使用 `*`
- [ ] 只允许必要的域名
- [ ] 敏感接口禁止跨域

---

## 4. Token 管理（Logout 黑名单）

### ❌ 禁止

```go
// 错误：Logout 无实际效果，token 仍可用
func Logout(token string) {
    // 什么都没做
    return "success"
}
```

### ✅ 正确

```go
// 正确：Token 黑名单
var tokenBlacklist = make(map[string]bool)
var mu sync.RWMutex

func Logout(token string) {
    mu.Lock()
    defer mu.Unlock()
    tokenBlacklist[token] = true
}

func IsTokenBlacklisted(token string) bool {
    mu.RLock()
    defer mu.RUnlock()
    return tokenBlacklist[token]
}

func ValidateToken(token string) bool {
    // 1. 解析 token
    // 2. 检查是否过期
    // 3. 检查是否在黑名单
    return !IsTokenBlacklisted(token)
}
```

### 检查清单

- [ ] Logout 将 token 加入黑名单
- [ ] 每次请求验证 token 时检查黑名单
- [ ] 定期清理过期 token

---

## 5. JWT 安全配置

### ✅ 正确配置

```go
import "github.com/golang-jwt/jwt/v5"

type Claims struct {
    UserID string `json:"user_id"`
    jwt.RegisteredClaims
}

func GenerateToken(userID string) (string, error) {
    claims := Claims{
        UserID: userID,
        RegisteredClaims: jwt.RegisteredClaims{
            ExpiresAt: jwt.NewNumericDate(time.Now().Add(15 * time.Minute)), // 短期有效
            IssuedAt:  jwt.NewNumericDate(time.Now()),
        },
    }

    token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
    return token.SignedString([]byte(os.Getenv("JWT_SECRET")))
}
```

### 检查清单

- [ ] Access token 有效期 ≤ 15 分钟
- [ ] Refresh token 有效期 ≤ 7 天
- [ ] Token 签名使用 HS256 或 RS256
- [ ] 敏感操作需要重新验证

---

## 6. 安全 Headers

### ✅ 推荐 Headers

```go
// 安全 Headers 中间件
func SecurityHeaders() gin.HandlerFunc {
    return func(c *gin.Context) {
        c.Header("X-Frame-Options", "DENY")
        c.Header("X-Content-Type-Options", "nosniff")
        c.Header("X-XSS-Protection", "1; mode=block")
        c.Header("Content-Security-Policy", "default-src 'self'")
        c.Header("Strict-Transport-Security", "max-age=31536000; includeSubDomains")
        c.Next()
    }
}
```

### 检查清单

- [ ] `X-Frame-Options: DENY`
- [ ] `X-Content-Type-Options: nosniff`
- [ ] `Content-Security-Policy` 已配置
- [ ] 生产环境强制 HTTPS

---

## 7. 安全编码自审清单

### 开发完成前必须检查

| 检查项 | 说明 | 优先级 |
|--------|------|--------|
| 无硬编码 Secrets | 密钥/密码/Token 必须从环境变量读取 | P0 |
| 密码哈希 | 用户密码必须使用 bcrypt 等哈希存储 | P0 |
| CORS 限制 | 生产环境禁止 `Access-Control-Allow-Origin: *` | P0 |
| Token 黑名单 | Logout 必须将 token 加入黑名单 | P0 |
| JWT 短期有效 | Access token 有效期 ≤ 15 分钟 | P1 |
| 安全 Headers | 配置 X-Frame-Options、CSP 等 | P1 |
| HTTPS 强制 | 生产环境强制使用 HTTPS | P1 |

---

## 常见错误对照表

| 错误 | 正确做法 |
|------|---------|
| `jwtSecret = "hardcoded"` | `jwtSecret = os.Getenv("JWT_SECRET")` |
| `password = "plaintext"` | `password = bcrypt.Hash(password)` |
| `cors(AllowAll: true)` | `cors(AllowedOrigins: ["https://example.com"])` |
| `Logout() {}` | `tokenBlacklist[token] = true` |
