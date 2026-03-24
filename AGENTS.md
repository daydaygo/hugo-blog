# AGENTS.md

Hugo 静态博客项目的 AI 编码代理指南。

## 项目概览

- **类型**: Hugo 静态博客
- **主题**: Ananke (git submodule)
- **语言**: 中文内容
- **部署**: GitHub Pages + Netlify + Cloudflare Pages

## 构建命令

### 本地开发

```bash
# 初始化主题子模块 (首次克隆时必须)
git submodule update --init --recursive

# 启动本地开发服务器 (http://localhost:1313)
hugo server

# 启动开发服务器并包含草稿文章
hugo server -D
```

### 构建与部署

```bash
# 构建站点 (输出到 public/)
hugo

# 构建并优化
hugo --gc --minify

# 构建指定 URL (用于部署)
hugo --gc --minify --baseURL "https://example.com/"
```

### 内容管理

```bash
# 创建新文章
hugo new posts/my-new-post.md

# 构建草稿文章
hugo -D

# 检查配置
hugo config
```

### 依赖版本

| 工具 | 版本 |
|------|------|
| Hugo | 0.150.0+ (extended) |
| Go | 1.25.1+ |
| Dart Sass | 1.92.1+ |

安装依赖: `brew install hugo`

## 文件结构

```
hugo-blog/
├── hugo.toml          # 主配置文件
├── content/
│   └── posts/        # 博客文章 (Markdown)
├── themes/
│   └── ananke/       # 主题 (git submodule)
├── archetypes/
│   └── default.md    # 新文章模板
├── public/           # 构建输出 (gitignore)
├── netlify.toml      # Netlify 配置
├── wrangler.toml     # Cloudflare Workers 配置
└── build.sh          # Cloudflare 构建脚本
```

## 部署配置

### Netlify

配置文件: `netlify.toml`

- 自动安装 Dart Sass、Go、Hugo
- 构建命令: `hugo --gc --minify --baseURL "${URL}"`
- 输出目录: `public/`

### Cloudflare Pages

配置文件: `wrangler.toml`, `build.sh`

- 通过 Cloudflare Workers 部署
- 构建脚本安装完整依赖链
- 时区设置: `Asia/Shanghai`

部署步骤:
1. 推送代码到 GitHub
2. 在 Cloudflare Dashboard 创建 Workers 项目
3. 连接 GitHub 仓库
4. 选择仓库并部署

## 代码风格指南

### Markdown 内容

#### Frontmatter (TOML 格式)

```toml
+++
date = '2025-01-15T10:00:00+08:00'
draft = false
title = '文章标题'
+++
```

规则:
- 使用 TOML 格式 (`+++` 分隔符)
- `date` 格式: RFC3339 (`YYYY-MM-DDTHH:MM:SS+08:00`)
- `draft`: `true` 为草稿, `false` 为发布
- `title`: 使用单引号包裹

#### 标题层级

```markdown
## 二级标题

### 三级标题

#### 四级标题
```

规则:
- 一级标题保留给页面 title, 正文从二级开始
- 标题前后各空一行
- 使用 ATX 风格 (`#`)

#### 列表格式

```markdown
- 无序列表项
  - 嵌套项 (两个空格缩进)
  - 另一个嵌套项

1. 有序列表
2. 第二项
```

规则:
- 无序列表使用 `-` 符号
- 嵌套使用两个空格缩进
- 列表项之间不空行

#### 链接与引用

```markdown
[链接文字](https://example.com)

> 引用内容
> 多行引用使用单个 `>`
```

规则:
- 使用内联链接而非引用式链接
- 引用块使用 `>` 符号

### Hugo 配置 (hugo.toml)

```toml
baseURL = 'https://example.com/'
languageCode = 'zh-cn'
title = '站点标题'
theme = 'ananke'
```

规则:
- 使用单引号包裹字符串值
- 保持配置简洁, 复杂配置放到单独文件
- 遵循 Hugo 官方配置规范

### Git 规范

#### Commit 消息

```
<type>: <description>

# 示例
content: 添加新文章《Go并发模式》
fix: 修复文章日期格式错误
config: 更新 Hugo 版本到 0.158.0
```

Type 类型:
- `content`: 内容变更 (新文章、修改文章)
- `fix`: 错误修复
- `config`: 配置变更
- `theme`: 主题相关
- `ci`: CI/CD 变更

#### 分支策略

- `main`: 主分支, 保护分支
- 功能分支从 `main` 创建, PR 合并后删除

### 中文写作规范

- 中英文之间添加空格: `使用 Hugo 构建`
- 中文与数字之间添加空格: `版本 0.150.0`
- 使用全角标点符号
- 专有名词保持原样: `GitHub`, `Hugo`, `Netlify`, `Cloudflare`

## 常见任务

### 添加新文章

1. 创建文件: `hugo new posts/article-slug.md`
2. 编辑 frontmatter 和内容
3. 设置 `draft = false` 发布
4. 本地预览: `hugo server -D`
5. 提交并推送

### 更新主题

```bash
git submodule update --remote themes/ananke
```

### 故障排查

```bash
# 检查 Hugo 版本
hugo version

# 详细输出
hugo --verbose

# 检查配置
hugo config
```

## 注意事项

- **不要**提交 `public/` 目录
- **不要**直接修改 `themes/` 下的文件
- 主题更新通过 git submodule 管理
- 草稿文章 (`draft = true`) 不会被部署