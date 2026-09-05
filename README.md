# ThreeUI 风格目录构建 Skill

这是一个用于 Codex 的 Skill，帮助 Codex 按照 [ThreeUI](https://github.com/MengTo/threeui) 的整体体验，构建 React 交互式组件目录和预览应用。

## 能力范围

这个 Skill 会指导 Codex 实现：

- 组件分类、标签筛选和 Browse 卡片网格
- 搜索弹窗和 `Ctrl/Cmd+K` 快捷键
- 详情页、实时预览、加载状态和错误状态
- 变体选择和动态控制器
- 组件源码、导入方式和安装文档
- 浅色、深色和跟随系统主题
- URL 路由、浏览器前进后退和移动端导航抽屉
- Three.js、WebGL 组件的资源加载和生命周期处理

它是“构建 ThreeUI 风格体验”的指导 Skill，不是 ThreeUI 完整应用的源码副本。

## 目录结构

```text
threeui/
├─ SKILL.md
├─ agents/
│  └─ openai.yaml
└─ references/
   └─ threeui-catalog.md
```

## 在 Codex 中使用

将这个目录放入 Codex 的 Skill 目录，然后在请求中明确调用：

```text
使用 $threeui 构建一个类似 ThreeUI 的交互式组件目录。
```

如果项目需要使用公开的 ThreeUI 组件包：

```bash
npm install @designcodeio/threeui
```

## 参考项目

- ThreeUI 仓库：[https://github.com/MengTo/threeui](https://github.com/MengTo/threeui)
- ThreeUI Community：[https://threeui.com](https://threeui.com)
- npm 包：`@designcodeio/threeui`

## 许可与使用边界

公开仓库和 npm 包主要提供 Community 内容。使用公开代码或资源时，请保留 MIT、SIL Open Font License 和第三方声明。

不要把 Pro 或 Beta 源码、未经许可的远程缩略图、预览资源或品牌标识作为本 Skill 的本地内容重新分发。需要使用 Pro CLI 时，必须由用户明确要求并确认拥有相应权限。

## 验证

Skill 文件应至少满足：

- 根目录存在 `SKILL.md`；
- frontmatter 包含合法的 `name` 和 `description`；
- `references/threeui-catalog.md` 可从 `SKILL.md` 正确链接；
- `agents/openai.yaml` 的默认提示词包含 `$threeui`；
- 文档中没有未完成的 TODO 占位符。

当前仓库的 Skill 内容已完成上述检查，并已上传到 GitHub。
