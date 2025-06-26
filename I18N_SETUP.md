# Astro 博客国际化设置指南

## 📋 概述

本项目已成功实现了中英文双语支持，采用 Astro 官方的 i18n 功能。

## 🏗️ 架构设计

### URL 结构

- **中文（默认）**: `/`, `/blog`, `/rustzen-admin`
- **英文**: `/en`, `/en/blog`, `/en/rustzen-admin`

### 文件结构

```
src/
├── content/
│   ├── blog/
│   │   ├── article-name.zh.md   # 中文文章
│   │   └── article-name.en.md   # 英文文章
│   └── rustzen-admin/
│       ├── article-name.zh.md
│       └── article-name.en.md
├── pages/
│   ├── index.astro              # 中文首页
│   ├── blog/
│   │   ├── index.astro          # 中文博客列表
│   │   └── [...slug].astro      # 中文文章页面
│   ├── rustzen-admin/
│   │   ├── index.astro          # 中文项目页面
│   │   └── [...slug].astro      # 中文项目文章
│   └── en/                      # 英文版本页面
│       ├── index.astro
│       ├── blog/
│       └── rustzen-admin/
├── components/
│   ├── LanguageSwitcher.astro   # 语言切换组件
│   └── ...
└── i18n.ts                     # 国际化配置
```

## 🔧 主要功能

### 1. 语言切换

- 顶部导航栏右侧的语言切换器
- 自动保持当前页面路径，仅切换语言

### 2. 内容过滤

- 中文页面只显示 `.zh.md` 文件
- 英文页面只显示 `.en.md` 文件

### 3. 自动化路由

- 统一的 slug 生成（移除语言后缀）
- 动态路由支持中英文内容

## 📝 使用方法

### 添加新文章

1. 创建中文版本：`src/content/blog/article-name.zh.md`
2. 创建英文版本：`src/content/blog/article-name.en.md`
3. 确保两个文件的 frontmatter 结构一致

### 添加新翻译

在 `src/i18n.ts` 中添加新的翻译键值对：

```typescript
export const ui = {
  zh: {
    'new.key': '中文翻译',
  },
  en: {
    'new.key': 'English translation',
  },
};
```

### 在组件中使用翻译

```astro
---
import { useTranslations } from '../i18n';

const currentLocale = (Astro.currentLocale || 'zh') as 'zh' | 'en';
const t = useTranslations(currentLocale);
---

<h1>{t('page.title')}</h1>
```

## 🎯 关键特性

1. **SEO 友好**: 每种语言都有独立的 URL
2. **性能优化**: 静态生成，无需客户端 JavaScript
3. **可扩展性**: 容易添加更多语言
4. **维护性**: 清晰的文件结构和命名规范

## 🚀 部署说明

构建时会生成完整的静态站点：

```bash
pnpm build
```

生成的文件结构：

```
dist/
├── index.html              # 中文首页
├── blog/
├── rustzen-admin/
└── en/                     # 英文版本
    ├── index.html
    ├── blog/
    └── rustzen-admin/
```

## 🔍 测试

访问以下 URL 验证功能：

- 中文首页：http://localhost:4321/
- 英文首页：http://localhost:4321/en
- 中文博客：http://localhost:4321/blog
- 英文博客：http://localhost:4321/en/blog

## 📚 相关文档

- [Astro i18n 官方文档](https://docs.astro.build/en/guides/internationalization/)
- [Astro 内容集合](https://docs.astro.build/en/guides/content-collections/)
