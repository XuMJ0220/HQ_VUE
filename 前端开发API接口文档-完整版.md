# 个人网站后端API接口文档 - 前端开发指南

## 📋 项目概述

这是一个个人网站的后端API系统，提供用户认证、分类管理和笔记管理功能。本文档专为前端开发者编写，包含所有必要的接口信息和使用示例。

## 🔗 基础信息

- **基础URL**: `http://localhost:8080` (开发环境)
- **API版本**: v1
- **认证方式**: JWT Token
- **数据格式**: JSON
- **字符编码**: UTF-8

## 🔐 认证说明

### JWT Token 使用方法

1. 用户登录成功后，后端会返回JWT token
2. 在后续请求中，需要在请求头中携带token：
   ```
   Authorization: Bearer <your-jwt-token>
   ```

### 需要认证的接口
- 所有 `/api/v1/admin/*` 路径下的接口都需要JWT认证

## 📊 统一响应格式

### 成功响应格式
```json
{
  "code": 1008,
  "msg": "查询成功",
  "detail": {
    // 具体数据内容
  }
}
```

### 错误响应格式
```json
{
  "code": 1009,
  "msg": "查询失败",
  "detail": "详细错误信息"
}
```

## 🔑 用户认证接口

### 1. 用户注册
- **接口**: `POST /api/v1/user/signup`
- **描述**: 新用户注册
- **认证**: 无需认证

**请求参数**:
```json
{
  "username": "用户名",
  "password": "密码",
  "re_password": "确认密码",
  "email": "邮箱地址",
  "gender": 1
}
```

**字段说明**:
- `username`: 用户名 (必填)
- `password`: 密码 (必填)
- `re_password`: 确认密码 (必填，必须与password相同)
- `email`: 邮箱地址 (必填，必须是有效邮箱格式)
- `gender`: 性别 (0=未知, 1=男, 2=女)

**成功响应** (1004):
```json
{
  "code": 1004,
  "msg": "注册成功",
  "detail": {
    "user_id": 123456789
  }
}
```

**错误响应**:
- `1000`: 请求参数错误
- `1005`: 注册失败

### 2. 用户登录
- **接口**: `POST /api/v1/user/login`
- **描述**: 用户登录获取JWT token
- **认证**: 无需认证

**请求参数**:
```json
{
  "username": "用户名",
  "password": "密码"
}
```

**成功响应 - 普通用户** (1006):
```json
{
  "code": 1006,
  "msg": "登录成功",
  "detail": {
    "role": "普通用户",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**成功响应 - 管理员** (1002):
```json
{
  "code": 1002,
  "msg": "管理员登录",
  "detail": {
    "role": "管理员",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**错误响应**:
- `1000`: 请求参数错误
- `1007`: 登录失败

## 📁 分类管理接口

### 1. 获取所有分类
- **接口**: `GET /api/v1/admin/categories`
- **描述**: 获取所有分类列表
- **认证**: 需要JWT token

**成功响应** (1008):
```json
{
  "code": 1008,
  "msg": "查询成功",
  "detail": [
    {
      "id": 1,
      "name": "技术分享"
    },
    {
      "id": 2,
      "name": "生活随笔"
    }
  ]
}
```

**错误响应**:
- `1010`: 分类为空
- `1001`: 服务器繁忙

### 2. 获取单个分类
- **接口**: `GET /api/v1/admin/categories/{id}`
- **描述**: 根据ID获取特定分类
- **认证**: 需要JWT token

**路径参数**:
- `id`: 分类ID (整数)

**成功响应** (1008):
```json
{
  "code": 1008,
  "msg": "查询成功",
  "detail": {
    "id": 1,
    "name": "技术分享"
  }
}
```

**错误响应**:
- `1009`: 查询失败
- `1001`: 服务器繁忙

### 3. 创建分类
- **接口**: `POST /api/v1/admin/categories`
- **描述**: 创建新分类
- **认证**: 需要JWT token

**请求参数**:
```json
{
  "name": "新分类名称"
}
```

**成功响应** (1013):
```json
{
  "code": 1013,
  "msg": "添加成功",
  "detail": "添加成功"
}
```

**错误响应**:
- `1014`: 添加失败
- `1001`: 服务器繁忙

### 4. 更新分类
- **接口**: `PUT /api/v1/admin/categories/{id}`
- **描述**: 更新指定分类的名称
- **认证**: 需要JWT token

**路径参数**:
- `id`: 分类ID (整数)

**请求参数**:
```json
{
  "name": "更新后的分类名称"
}
```

**成功响应** (1011):
```json
{
  "code": 1011,
  "msg": "更新成功",
  "detail": "更新成功"
}
```

**错误响应**:
- `1012`: 更新失败
- `1001`: 服务器繁忙

### 5. 删除分类
- **接口**: `DELETE /api/v1/admin/categories/{id}`
- **描述**: 删除指定分类（软删除）
- **认证**: 需要JWT token

**路径参数**:
- `id`: 分类ID (整数)

**成功响应** (1015):
```json
{
  "code": 1015,
  "msg": "删除成功",
  "detail": "删除成功"
}
```

**错误响应**:
- `1016`: 删除失败
- `1001`: 服务器繁忙

## 📝 笔记管理接口

### 1. 获取所有笔记
- **接口**: `GET /api/v1/admin/notes`
- **描述**: 获取所有笔记列表
- **认证**: 需要JWT token

**成功响应** (1019):
```json
{
  "code": 1019,
  "msg": "获取笔记成功",
  "detail": [
    {
      "id": "1",
      "author_name": "作者用户名",
      "category_name": "分类名称",
      "title": "笔记标题",
      "contend_md": "Markdown格式内容",
      "contend_html": "<p>HTML格式内容</p>",
      "status": 1,
      "create_at": "2024-01-01T10:00:00Z",
      "update_at": "2024-01-01T10:00:00Z"
    }
  ]
}
```

**错误响应**:
- `1020`: 获取笔记失败
- `1001`: 服务器繁忙

### 2. 获取单个笔记
- **接口**: `GET /api/v1/admin/notes/{id}`
- **描述**: 根据ID获取特定笔记
- **认证**: 需要JWT token

**路径参数**:
- `id`: 笔记ID (整数)

**成功响应** (1021):
```json
{
  "code": 1021,
  "msg": "获取一则笔记成功",
  "detail": {
    "id": "1",
    "author_name": "作者用户名",
    "category_name": "分类名称",
    "title": "笔记标题",
    "contend_md": "Markdown格式内容",
    "contend_html": "<p>HTML格式内容</p>",
    "status": 1,
    "create_at": "2024-01-01T10:00:00Z",
    "update_at": "2024-01-01T10:00:00Z"
  }
}
```

**错误响应**:
- `1022`: 获取一则笔记失败
- `1001`: 服务器繁忙

### 3. 创建笔记
- **接口**: `POST /api/v1/admin/notes`
- **描述**: 创建新笔记
- **认证**: 需要JWT token

**请求参数**:
```json
{
  "title": "笔记标题",
  "content_md": "# 这是Markdown格式的内容\n\n这里是正文...",
  "category_id": 1,
  "status": 1
}
```

**字段说明**:
- `title`: 笔记标题 (必填)
- `content_md`: Markdown格式内容 (必填)
- `category_id`: 分类ID (必填)
- `status`: 状态 (0=草稿, 1=发布)

**成功响应** (1017):
```json
{
  "code": 1017,
  "msg": "添加笔记成功",
  "detail": {
    "id": "1",
    "author_name": "作者用户名",
    "category_name": "分类名称",
    "title": "笔记标题",
    "contend_md": "Markdown格式内容",
    "contend_html": "<h1>这是Markdown格式的内容</h1><p>这里是正文...</p>",
    "status": 1,
    "create_at": "2024-01-01T10:00:00Z",
    "update_at": "2024-01-01T10:00:00Z"
  }
}
```

**错误响应**:
- `1018`: 添加笔记失败
- `1003`: 请求头中头部验证失败
- `1001`: 服务器繁忙

### 4. 更新笔记
- **接口**: `PUT /api/v1/admin/notes/{id}`
- **描述**: 更新指定笔记
- **认证**: 需要JWT token

**路径参数**:
- `id`: 笔记ID (整数)

**请求参数** (所有字段都是可选的):
```json
{
  "title": "更新后的标题",
  "content_md": "更新后的Markdown内容",
  "category_id": 2,
  "status": 1
}
```

**成功响应** (1025):
```json
{
  "code": 1025,
  "msg": "更新笔记成功",
  "detail": "更新成功"
}
```

**错误响应**:
- `1026`: 更新笔记失败
- `1001`: 服务器繁忙

### 5. 删除笔记
- **接口**: `DELETE /api/v1/admin/notes/{id}`
- **描述**: 删除指定笔记（软删除）
- **认证**: 需要JWT token

**路径参数**:
- `id`: 笔记ID (整数)

**成功响应** (1023):
```json
{
  "code": 1023,
  "msg": "删除笔记成功",
  "detail": "删除成功"
}
```

**错误响应**:
- `1024`: 删除笔记失败
- `1001`: 服务器繁忙

## 📋 数据模型

### RegisterParam (注册参数)
```typescript
interface RegisterParam {
  username: string;     // 用户名 (必填)
  password: string;     // 密码 (必填)
  re_password: string;  // 确认密码 (必填，必须与password相同)
  email: string;        // 邮箱 (必填，必须是有效邮箱格式)
  gender: number;       // 性别 (0=未知, 1=男, 2=女)
}
```

### LoginParam (登录参数)
```typescript
interface LoginParam {
  username: string;     // 用户名 (必填)
  password: string;     // 密码 (必填)
}
```

### CategoryName (分类参数)
```typescript
interface CategoryName {
  name: string;         // 分类名称 (必填)
}
```

### CategoriesParam (分类响应)
```typescript
interface CategoriesParam {
  id: number;           // 分类ID
  name: string;         // 分类名称
}
```

### CreateNoteParam (创建笔记参数)
```typescript
interface CreateNoteParam {
  title: string;        // 标题 (必填)
  content_md: string;   // Markdown内容 (必填)
  category_id: number;  // 分类ID (必填)
  status: number;       // 状态 (0=草稿, 1=发布)
}
```

### UpdateNoteParam (更新笔记参数)
```typescript
interface UpdateNoteParam {
  title?: string;       // 标题 (可选)
  content_md?: string;  // Markdown内容 (可选)
  category_id?: number; // 分类ID (可选)
  status?: number;      // 状态 (可选，0=草稿, 1=发布)
}
```

### NoteResponse (笔记响应)
```typescript
interface NoteResponse {
  id: string;           // 笔记ID (字符串格式)
  author_name: string;  // 作者用户名
  category_name: string; // 分类名称
  title: string;        // 标题
  contend_md: string;   // Markdown内容
  contend_html: string; // HTML内容
  status: number;       // 状态 (0=草稿, 1=发布)
  create_at: string;    // 创建时间
  update_at: string;    // 更新时间
}
```

## 🚨 状态代码说明

| 状态代码 | 说明 |
|---------|------|
| 1000 | 请求参数错误 |
| 1001 | 服务器繁忙 |
| 1002 | 管理员登录 |
| 1003 | 请求头中头部验证失败 |
| 1004 | 注册成功 |
| 1005 | 注册失败 |
| 1006 | 登录成功 |
| 1007 | 登录失败 |
| 1008 | 查询成功 |
| 1009 | 查询失败 |
| 1010 | 分类为空 |
| 1011 | 更新成功 |
| 1012 | 更新失败 |
| 1013 | 添加成功 |
| 1014 | 添加失败 |
| 1015 | 删除成功 |
| 1016 | 删除失败 |
| 1017 | 添加笔记成功 |
| 1018 | 添加笔记失败 |
| 1019 | 获取笔记成功 |
| 1020 | 获取笔记失败 |
| 1021 | 获取一则笔记成功 |
| 1022 | 获取一则笔记失败 |
| 1023 | 删除笔记成功 |
| 1024 | 删除笔记失败 |
| 1025 | 更新笔记成功 |
| 1026 | 更新笔记失败 |

## 💡 前端开发建议

### 1. 状态管理
建议使用状态管理库（如Redux、Zustand等）来管理：
- 用户登录状态
- JWT token
- 当前用户信息

### 2. HTTP请求封装
建议封装HTTP请求库，自动处理：
- JWT token添加
- 错误响应处理
- 请求/响应拦截

### 3. 错误处理
根据返回的状态代码，提供用户友好的错误提示。

### 4. Markdown渲染
前端可以直接使用后端返回的HTML内容，或者使用Markdown渲染库处理MD内容。

### 5. 数据类型注意事项
- 笔记ID在响应中是字符串格式，但在路径参数中是整数
- 性别字段是数字类型 (0/1/2)
- 笔记状态是数字类型 (0/1)

## 🔧 开发环境配置

### 启动后端服务
```bash
cd d:\学习文件夹\Golang\项目\HQ
go run main.go
```

### Swagger文档
访问 `http://localhost:8080/swagger/index.html` 查看在线API文档。

## 📞 联系方式

如有问题，请联系后端开发者或查看项目README文件。

---

**文档版本**: 2.0  
**最后更新**: 2024年12月  
**适用前端框架**: React, Vue, Angular 等