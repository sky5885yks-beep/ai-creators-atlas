# 中文 AI 创作者 · GitHub 地图

以“人”为主体的中文 AI 创作者导航：先找到值得跟的人，再顺着他们的代表项目进入 GitHub，而不是在 star 榜里捞针。

- 在线地址：<https://37lfc0vis820xyiwbbv.traesite.cn/>（当前仅自己可见）
- 形态：单文件静态站点（HTML + 内联 CSS/JS，无构建、无后端）
- 数据快照：2026-11-23
- 规模：16 位创作者、5 个分类、48 个代表项目
- 设计文档：[docs/DESIGN.md](docs/DESIGN.md)

## 功能

- 创作者卡片墙，按分类分组
- 分类筛选：教程科普 / 工具与创作 / Prompt 方法论 / 社区与平台 / 开发与综合
- 主阵地筛选：GitHub / 公众号 / 博客 / 官网 / B站 / X / 知乎 等
- 实时搜索：姓名、handle、标签、项目名
- 榜单视图：按 followers 排序
- 详情弹层：简介、主阵地、代表项目、无 GitHub 主号兜底文案
- 键盘：`/` 聚焦搜索，`Esc` 关闭弹层或清空搜索
- 响应式：1080px / 680px 两档断点，支持 `prefers-reduced-motion`

## 目录

```
.
├─ index.html              # 站点全部源码（样式、数据、交互）
├─ docs/
│  └─ DESIGN.md            # 信息架构、数据模型、视觉与交互设计
├─ scripts/
│  └─ check_site.py        # 零依赖静态检查
├─ .github/workflows/ci.yml
├─ CHANGELOG.md
└─ README.md
```

## 本地预览

```bash
python -m http.server 8000
# 打开 http://localhost:8000
```

## 修改数据

所有内容都在 `index.html` 的脚本区，主要数据结构：

- `CATS`：分类定义（`id/no/title/desc`）
- `PEOPLE`：创作者（`id/name/handle/followers/cat/avatar/tag/plat/repos`）
- `LINKS`：主阵地链接
- `EXTRA`：补充信息与兜底文案

新增创作者时：

1. 在 `PEOPLE` 增加一条，`id` 唯一；
2. 在 `LINKS` 补主阵地 URL，没有固定 URL 的平台不要伪造；
3. 代表项目写入 `repos`，字段为 `n/s/d`（名称/star/描述）；
4. 更新页脚快照日期；
5. 本地打开检查搜索、筛选、弹层是否正常。

## 数据口径

- 收录标准：在中文 AI 圈有持续影响力，不要求必须有 GitHub 仓库；
- star / followers 为 2026-11-23 的 GitHub API 快照，会过期；
- 公众号没有可外部直接访问的公开主页，仅标注平台，不伪造链接；
- 站点为非官方整理，与所列创作者无隶属关系。

## 部署

当前部署在 TraeSite，源码以本仓库为准。更新 `index.html` 后重新部署即可；仓库保持私有，站点可见性由部署平台控制。

## 开发检查

```bash
python scripts/check_site.py
```

CI 会在 push / PR 时执行同一脚本，检查标题、描述、H1、重复 `id` 与空链接。
