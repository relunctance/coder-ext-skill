<!-- TL;DR: URL 规范：不暴露内部路径、正确格式、干净 URL -->
---
name: url-standards
description: URL 规范 - 不暴露内部路径结构、正确格式、干净 URL。用于交付时提供正确的访问地址。
---

# URL Standards - URL 规范

> **TL;DR**: 交付时必须提供正确的 URL，不暴露内部路径结构。

## 触发条件

- 交付网页项目时
- 发送飞书群通知时
- 提供访问地址时

---

## 1. URL 格式规范

### ❌ 禁止：暴露内部路径

```markdown
<!-- 错误：暴露内部路径 -->
http://172.25.115.155:8080/src/index.html
http://192.168.1.100:3000/static/pages/login.html
```

### ✅ 正确：干净 URL

```markdown
<!-- 正确：使用根路径或特定路径 -->
http://172.25.115.155:8080/
http://demo.example.com/
http://app.example.com/login
```

### 规则

| 类型 | 正确格式 | 错误格式 |
|------|---------|---------|
| 网页首页 | `http://host/` | `http://host/src/index.html` |
| 登录页 | `http://host/login` | `http://host/src/pages/login.html` |
| API | `http://host/api/users` | `http://host/src/server/routes/users.js` |

---

## 2. 不同服务类型的 URL

### 静态网页

```markdown
<!-- 静态文件服务 -->
http://172.25.115.155:8080/
```

### React/Vue 构建产物

```markdown
<!-- SPA 应用 -->
http://172.25.115.155:3000/
http://app.example.com/

<!-- 如果有 basename -->
http://app.example.com/myapp/
```

### API 服务

```markdown
<!-- API Base URL -->
http://api.example.com/v1
http://172.25.115.155:8080/api
```

### 前后端分离

```markdown
<!-- 前端 -->
http://front.example.com/

<!-- 后端 API -->
http://api.example.com/v1/

<!-- 文档 -->
http://docs.example.com/
```

---

## 3. URL 路径设计原则

### 简洁清晰

```markdown
✅ /users          # 用户列表
✅ /users/:id      # 用户详情
✅ /posts/:id/comments  # 帖子的评论

❌ /getUserListById
❌ /user_detail_page
❌ /api/v1/internal/users/controller
```

### RESTful 规范

| 操作 | 方法 | URL |
|------|------|-----|
| 获取列表 | GET | `/users` |
| 获取详情 | GET | `/users/:id` |
| 创建 | POST | `/users` |
| 更新 | PUT | `/users/:id` |
| 删除 | DELETE | `/users/:id` |

---

## 4. 部署检查清单

### 交付前必须检查

| 检查项 | 说明 |
|--------|------|
| URL 可访问 | 确认 `http://host/` 能打开 |
| 无 404 | 确认主要页面无 404 |
| 无内部路径 | URL 中不包含 `/src/`、`/static/`、`/dist/` |
| HTTPS | 生产环境优先使用 HTTPS |

### 自检命令

```bash
# 检查首页
curl -I http://172.25.115.155:8080/

# 检查主要页面
curl -I http://172.25.115.155:8080/login

# 检查无 404
curl -s -o /dev/null -w "%{http_code}" http://172.25.115.155:8080/
```

---

## 5. 通知中的 URL 格式

### ✅ 正确格式

```markdown
【交付汇报】
📦 项目：demo_ip
🔗 地址：http://172.25.115.155:8080/
📋 页面数：5 个

📄 页面预览：
• 首页：用户登录和注册
• 仪表盘：数据概览
• 用户管理：CRUD 操作
```

### ❌ 错误格式

```markdown
<!-- 错误：暴露内部路径 -->
🔗 地址：http://172.25.115.155:8080/src/index.html

<!-- 错误：包含端口但无必要 -->
🔗 地址：http://172.25.115.155:8080/src/pages/index.html

<!-- 错误：路径太深 -->
🔗 地址：http://172.25.115.155:8080/static/js/chunk-vendors.a1b2c3d4.js
```

---

## 6. 常见错误对照表

| 错误 | 正确做法 |
|------|---------|
| `/src/index.html` | `/` |
| `/static/pages/login.html` | `/login` |
| `/dist/js/app.js` | `/`（前端资源不应直接访问） |
| `http://host:8080/src/...` | 移除 `/src`，直接用根路径 |
| 内部 IP + 复杂路径 | 配置反向代理，使用干净路径 |

---

## 7. 截图规范

### 必须截图的场景

| 页面类型 | 必须截图 | 说明 |
|---------|---------|------|
| 首页/概览 | ✅ | 展示整体效果 |
| 核心功能 | ✅ | 2-3 个主要页面 |
| 响应式验证 | ✅ | 手机端截图 |

### 截图命名

```bash
# 命名规范
screenshots/
├── 01-homepage.png      # 首页
├── 02-login.png         # 登录页
├── 03-dashboard.png     # 仪表盘
├── 04-mobile-home.png   # 移动端首页
└── 05-mobile-menu.png   # 移动端菜单
```

---

## 8. 交付汇报模板

```markdown
【交付汇报】
📦 项目：{项目名称}
🔗 地址：{URL}
📋 页面数：{N} 个

📄 页面预览：
• {页面1}：{功能说明}
• {页面2}：{功能说明}
• {页面3}：{功能说明}

📊 功能状态：
• ✅ {已交付功能}
• ⚠️ {未完成功能}

【截图】
{MEDIA:/path/to/screenshot1.png}
{MEDIA:/path/to/screenshot2.png}
```

---

## 检查清单

| 检查项 | 说明 |
|--------|------|
| URL 可访问 | `curl -I http://host/` 返回 200 |
| 无内部路径 | URL 中不包含 `/src/`、`/static/`、`/dist/` |
| 路径简洁 | 使用 `/login` 而不是 `/src/pages/login.html` |
| 截图完整 | 首页 + 核心功能 + 响应式 |
