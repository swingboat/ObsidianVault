# ObsidianVault

个人 Obsidian 知识库，保存学习记录、项目会话总结、视频制作经验和网页摘录。实际知识库位于 `Swingboat/`。

## 开始使用

1. 在 Obsidian 中选择「打开本地仓库」。
2. 选择本项目下的 `Swingboat/` 目录。
3. 通过左侧文件列表或 Obsidian 搜索浏览已有笔记。

已有笔记可直接使用；如需 Copilot 功能，请在 Obsidian 中安装并配置相应社区插件。插件代码及本地设置不纳入版本管理。

## 目录说明

| 目录 | 内容 |
| --- | --- |
| `Swingboat/Clippings/` | 网页摘录 |
| `Swingboat/Copilot/` | Copilot 自定义提示词 |
| `Swingboat/Project Conversations/` | 项目会话索引、知识总结和引用索引 |
| `Swingboat/Python/` | Python 学习记录 |
| `Swingboat/TypeScript/` | TypeScript 学习记录 |
| `Swingboat/Skill/` | Skill 相关资料和提示词 |
| `Swingboat/Video Creation/` | 视频制作项目记录 |
| `Swingboat/.obsidian/` | Obsidian 配置 |

## 内容维护

- 沿用现有分类和命名，新增笔记放入对应主题目录。
- 移动或重命名笔记时，优先通过 Obsidian 操作，并检查内部链接。
- 保留已有 Markdown、双向链接、嵌入和 frontmatter 格式。
- 本地敏感配置不写入笔记或提交到 Git。

## Git 使用

在项目根目录操作：

```sh
git status
git diff
git add <需要保存的文件或目录>
git diff --cached
git commit -m "docs: 更新学习笔记"
```

版本管理包含笔记、提示词和基础 Obsidian 配置；忽略工作区布局、缓存、插件目录、回收站及本地环境变量文件。具体规则见 `.gitignore`。

本项目是文档知识库，无需安装构建依赖。内容变更后，在 Obsidian 中检查 Markdown 显示、内部链接和附件引用。
