---
title: Markdown 写作指南
date: '2024-12-28'
description: 这是一个完整的 Markdown 写作指南，包含代码高亮、格式化等常用功能的示例...
tags: ['Markdown', '写作指南', '参考文档']
keywords: Markdown, 写作指南, 参考文档
---

# Markdown 写作指南

## 文章头部格式 (Frontmatter)

每篇文章都需要包含以下头部信息：

```markdown
---
title: 文章标题
date: '2024-12-28'  # 日期格式：YYYY-MM-DD
author: 作者名
excerpt: 文章摘要，会显示在列表页...
tags: ['标签1', '标签2', '标签3']
readTime: 5  # 预计阅读时间（分钟）
---
```

## 代码块语法高亮

### 常用编程语言

```javascript
// JavaScript
const hello = "world";
console.log(hello);
```

```java
// Java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```

```python
# Python
def hello():
    print("Hello World")
```

```typescript
// TypeScript
interface User {
    name: string;
    age: number;
}
```

### 标记语言

```html
<!-- HTML -->
<div class="container">
    <h1>Hello World</h1>
</div>
```

```xml
<!-- XML -->
<root>
    <child>Content</child>
</root>
```

```css
/* CSS */
.container {
    display: flex;
    justify-content: center;
}
```

### 配置文件

```yaml
# YAML
version: '3'
services:
  web:
    image: nginx
```

```json
// JSON
{
  "name": "project",
  "version": "1.0.0"
}
```

### 数据库

```sql
-- SQL
SELECT * FROM users WHERE age > 18;
```

### 命令行

```bash
# Bash
npm install react
git commit -m "message"
```

```powershell
# PowerShell
Set-Location C:\Projects
```

### 其他格式

```plaintext
纯文本内容，不需要语法高亮
```

```diff
- 这行会显示为红色（删除）
+ 这行会显示为绿色（添加）
```

## 文本格式化

### 标题

# 一级标题
## 二级标题
### 三级标题
#### 四级标题

### 文本样式

**粗体文本**
*斜体文本*
~~删除线文本~~
`行内代码`
### 列表

无序列表：
- 项目 1
- 项目 2
  - 子项目 2.1
  - 子项目 2.2

有序列表：
1. 第一步
2. 第二步
   1. 子步骤 2.1
   2. 子步骤 2.2

### 引用

> 这是一段引用文本
> 可以有多行

### 表格

| 列1 | 列2 | 列3 |
|-----|-----|-----|
| 内容1 | 内容2 | 内容3 |
| 内容4 | 内容5 | 内容6 |

### 链接和图片

[链接文本](https://example.com)

![图片描述](/images/markdown-guide/example.gif)

## 文件组织

建议的文件组织结构：

```plaintext
src/
  └── posts/
      ├── 2024/
      │   ├── article1.md
      │   └── article2.md
      ├── 2025/
      │   └── article3.md
      └── reference/
          └── markdown-guide.md
```

## 注意事项

1. 确保每个代码块都指定了正确的语言
2. 图片路径使用相对路径，从当前文件位置开始
3. 保持文件名全小写，使用连字符分隔
4. 标签使用有意义的关键词
5. 合理估计阅读时间

## 常见问题解决

1. 图片不显示
   - 检查图片路径是否正确
   - 确保图片文件存在
   - 使用正斜杠(/)而不是反斜杠(\)

2. 代码高亮失效
   - 检查语言标识是否正确
   - 确保代码块使用三个反引号
   - 语言标识要小写

3. 格式混乱
   - 检查 frontmatter 格式
   - 确保使用正确的缩进
   - 保持一致的换行风格 