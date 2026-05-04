# 第1章：基础技术快速复习（HTML/CSS/JS/TS/SQL）

**预计学习时间：2 小时**

> 本章定位：
> - 这是“复习与校准章”，不是“从零教学章”。
> - 重点是让你在后续 CMS 开发前，快速对齐所需最小能力。
> - 每个小节只讲“够用知识 + CMS场景落地”。

---

## 本章目标

完成本章后，你应当能够：

1. 手写一个具备语义结构的基础页面，并理解前后台页面结构差异。
2. 用 CSS 完成常见后台布局与基础可读性优化。
3. 用 JavaScript 响应用户操作并驱动页面交互。
4. 用 TypeScript 为前端/后端数据结构增加类型约束。
5. 写出支撑 CMS 核心流程的 SQL CRUD 语句。

---

## 1.1 HTML：语义化与表单是 CMS 的起点

在 CMS 系统里，HTML 主要承担两类任务：

- **前台展示结构**（文章列表、文章详情、导航等）
- **后台管理结构**（登录、编辑表单、管理列表）

### 1.1.1 常见语义标签（够用版）

- 页面骨架：`header`、`main`、`footer`
- 导航区域：`nav`
- 内容块：`section`、`article`
- 辅助内容：`aside`

语义化的价值不在“好看”，而在：

- 可读性高，便于协作；
- 有利于 SEO 与可访问性；
- AI 与开发者都更容易理解页面结构。

### 1.1.2 CMS 后台最关键的 HTML 能力：表单

后台管理页面核心是“编辑数据”，因此你至少要熟悉：

- `form` 提交行为
- `input` / `textarea` / `select`
- `label` 与 `for` 绑定
- `button` 类型（`submit` / `button`）

一个简单但实用的后台文章编辑表单结构：

```html
<form id="post-form">
  <label for="title">标题</label>
  <input id="title" name="title" required />

  <label for="content">正文</label>
  <textarea id="content" name="content" rows="10" required></textarea>

  <label for="status">状态</label>
  <select id="status" name="status">
    <option value="draft">草稿</option>
    <option value="published">已发布</option>
  </select>

  <button type="submit">保存文章</button>
</form>
```

---

## 1.2 CSS：先保证可用，再追求美观

本教程不追求复杂视觉体系。对小型 CMS，CSS 的优先级是：

1. 信息层级清晰（看得懂）
2. 操作路径明确（点得到）
3. 布局稳定（不乱跳）

### 1.2.1 后台页面常见布局

推荐从两栏布局开始：

- 左侧：导航菜单
- 右侧：内容工作区

示例（极简）：

```css
.layout {
  display: grid;
  grid-template-columns: 220px 1fr;
  min-height: 100vh;
}

.sidebar {
  border-right: 1px solid #e5e7eb;
  padding: 16px;
}

.content {
  padding: 24px;
}
```

### 1.2.2 表单与表格的最小可读样式

CMS 后台大多数页面是“表单 + 列表”。

- 输入控件高度统一
- 标签与输入间距一致
- 表格行高、边界清晰

这比花哨动画更能提高管理效率。

---

## 1.3 JavaScript：让后台从“静态”变“可操作”

在本教程中，JavaScript 的核心作用是：

- 响应事件（点击、输入、提交）
- 调用接口（后续章节会接入 API）
- 更新页面状态（加载中、成功、失败）

### 1.3.1 事件处理基础

```js
const form = document.getElementById('post-form');
form.addEventListener('submit', (event) => {
  event.preventDefault();
  // 读取表单并提交
});
```

### 1.3.2 你需要形成的习惯

- 先阻止默认提交，再做自定义逻辑。
- 所有异步操作都要有错误分支。
- 给用户明确反馈（例如“保存中”“保存成功”“保存失败”）。

---

## 1.4 TypeScript：把“约定”变成“可检查规则”

JavaScript 能跑，但 TypeScript 能减少“跑着跑着才出错”。

在 CMS 里，TS 最直接的价值是约束数据模型：

```ts
type PostStatus = 'draft' | 'published';

interface PostInput {
  title: string;
  content: string;
  status: PostStatus;
  categoryId?: number;
}
```

收益：

- 字段名拼写错误更容易被发现；
- 状态值范围更明确；
- 前后端协作接口更稳定。

---

## 1.5 SQL：CMS 的核心 CRUD 能力

无论你用哪种后端框架或 ORM，最终都要落到 SQL 的基本能力。

这里以 `posts` 表为例，复习最小 CRUD：

### 1.5.1 创建（Create）

```sql
INSERT INTO posts (title, content, status)
VALUES ('第一篇文章', '这是正文', 'draft');
```

### 1.5.2 查询（Read）

```sql
SELECT id, title, status, created_at
FROM posts
ORDER BY created_at DESC
LIMIT 20;
```

### 1.5.3 更新（Update）

```sql
UPDATE posts
SET status = 'published', updated_at = CURRENT_TIMESTAMP
WHERE id = 1;
```

### 1.5.4 删除（Delete）

```sql
DELETE FROM posts
WHERE id = 1;
```

### 1.5.5 你必须关注的两个点

1. **条件约束**：没有 `WHERE` 的更新/删除非常危险。
2. **状态设计**：`draft/published` 这种有限状态应在设计早期明确。

---

## 1.6 将五项基础能力映射到 CMS 开发流程

你可以把后续开发理解为下面这条链路：

- HTML：定义页面结构（编辑页、列表页、详情页）
- CSS：确保页面易用（布局、层级、可读性）
- JS：驱动交互（提交、反馈、筛选）
- TS：约束模型（输入输出、状态）
- SQL：落地数据（CRUD、排序、分页）

这五项能力并不是孤立的，而是串起来共同完成“内容管理”这个目标。

---

## 1.7 本章小结

本章我们没有追求“全”，而是对后续开发最关键的基础能力做了快速校准：

- HTML：语义结构 + 表单；
- CSS：后台布局 + 可读性；
- JS：事件与状态反馈；
- TS：类型约束与协作稳定性；
- SQL：最小 CRUD 与安全意识。

如果你能完成本章练习，进入第2章（需求分析与信息架构）会更顺畅。

---

## 本章练习

> 建议：每题都要“能运行/能验证”，不要只写概念答案。

### 练习1（HTML + CSS）
手写一个“文章新建页”，要求：

- 包含标题、正文、状态三个字段；
- 使用语义化结构（至少 `main`、`section`）；
- 页面采用两栏布局（左导航右内容）。

### 练习2（JavaScript）
为练习1中的表单添加提交逻辑：

- 阻止默认提交；
- 打印表单数据；
- 当标题为空时给出错误提示。

### 练习3（TypeScript）
定义 `PostInput` 类型，要求：

- `title`、`content` 必填；
- `status` 仅允许 `draft | published`；
- `categoryId` 可选。

### 练习4（SQL）
基于 `posts` 表写出以下语句：

1. 新增 2 篇草稿文章；
2. 查询最近 10 篇文章；
3. 将指定 id 的文章改为已发布；
4. 删除指定 id 的文章。

### 练习5（综合）
用一段话说明：在“发布文章”这个场景里，HTML/CSS/JS/TS/SQL 分别做了什么。

---

## 参考答案要点（自检）

### 练习1要点
- 字段完整；
- 结构语义清晰；
- 两栏布局可正常显示。

### 练习2要点
- 使用 `event.preventDefault()`；
- 能拿到 `title/content/status`；
- 有最小错误提示逻辑。

### 练习3要点
- 类型定义准确；
- `status` 为联合字面量类型；
- 可选字段标记正确。

### 练习4要点
- CRUD 语句语法正确；
- 更新/删除包含 `WHERE`；
- 查询语句包含排序与限制。

### 练习5要点
- 不只解释“技术本身”，要解释“在发布流程中的职责”。

---

## 下一步

如果你确认第1章正文内容符合预期，我将继续编写：

**第2章：CMS需求分析与信息架构设计**。
