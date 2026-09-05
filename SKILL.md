---
name: threeui
description: "按照 ThreeUI 的方式构建 React 交互式组件目录，包含浏览、搜索、路由、主题、实时预览、变体、控制器以及源码和安装文档。在用户要求复刻 ThreeUI 体验或构建类似目录时使用，不用于普通 React 界面开发。"
metadata:
  short-description: "构建具有浏览、搜索和实时预览能力的 ThreeUI 风格组件目录"
---

# ThreeUI 风格目录构建器

当用户要求类似 ThreeUI 的体验时，以公开的 ThreeUI 项目作为交互和视觉参考。目标是一个真正可用的组件目录与预览应用，而不是只有静态卡片的营销页面。

## 参考来源与边界

- 公开参考仓库：[https://github.com/MengTo/threeui](https://github.com/MengTo/threeui)
- 公开组件包：`@designcodeio/threeui`
- Community 目录：[https://threeui.com](https://threeui.com)
- 构建目录外壳、注册表、路由或预览控制器时，读取 [references/threeui-catalog.md](references/threeui-catalog.md)。

复刻公开的交互模型和信息架构，并根据用户产品调整品牌、内容和布局。未经许可，不复制私有内容、Pro/Beta 源码、受保护资源、远程缩略图或品牌标识。复用 MIT 授权的 Community 代码或资源时，保留相应许可证和第三方声明。

## 选择任务模式

根据用户需求选择最小实现范围：

1. **目录应用**：构建或扩展完整的浏览、搜索和预览体验。
2. **组件集成**：在现有 React 应用中使用 `@designcodeio/threeui` 的已验证组件，并保持宿主项目结构。
3. **单个预览或页面**：实现一个已验证的组件或渲染器及其控制器和响应式行为；除非用户要求，否则不要搭建完整目录。

如果用户没有指定框架，优先使用项目现有的 React、TypeScript 和构建工具。没有充分理由时，不替换已有技术栈。

## 完整目录的行为要求

构建目录应用时，将以下能力作为一个完整系统实现：

- 顶部栏：菜单入口、紧凑品牌标识、必要时的升级或操作区域，以及主题控制器。
- 响应式侧边栏或导航栏：展示目录分类或精选条目，并提供 Browse、安装文档和产品相关文档路由。
- Browse 页面：由元数据驱动的卡片网格、分类筛选、标签筛选、描述、缩略图或预览，以及空状态。
- 搜索弹窗或命令面板：支持 `Ctrl/Cmd+K` 打开、`Escape` 关闭，并搜索名称、描述、分类和标签。
- 详情页：实时预览、加载和错误状态、标题与描述、运行时和交互信息、变体选择、参数控制器、源码或导入标签页，以及安装说明。
- 可通过 URL 访问的分类筛选、标签筛选、目录条目、变体和文档路由。优先使用项目已有路由；没有路由库时，可以使用 `history.pushState` 和 `popstate` 实现轻量路由。
- 在产品需要时支持浅色、深色和跟随系统外观。安全保存用户选择；当存储不可用时仍提供可用的默认状态。
- 窄屏布局：可折叠导航、可关闭的遮罩层、可读的控制器、无横向溢出，以及适合触控的交互尺寸。

卡片不能只是装饰：点击卡片必须进入可工作的详情或预览路由；控制器必须改变预览，或者明确说明该渲染器不提供对应能力。

## 公开组件的集成方式

使用公开组件包时：

1. 先检查项目使用的包管理器，以及 React 和 TypeScript 配置。
2. 仅在项目尚未安装时添加依赖：

   ```bash
   npm install @designcodeio/threeui
   ```

   如果项目使用其他包管理器，沿用项目现有选择。
3. 共享样式只引入一次；在包支持时，优先使用组件子路径：

   ```tsx
   import { AtTheHorizon } from "@designcodeio/threeui";
   import "@designcodeio/threeui/style.css";

   export function Hero() {
     return <AtTheHorizon />;
   }
   ```

   ```tsx
   import { AtTheHorizon } from "@designcodeio/threeui/components/AtTheHorizon";
   ```

不要虚构组件名称、属性、变体或资源 URL。编码前，必须从已安装包的导出、类型定义、公开仓库或 Community 目录中验证它们。

## 渲染器和资源

将目录元数据与渲染器代码分离。使用稳定 ID 作为注册表键；对耗费较大的 Three.js 或 WebGL 渲染器尽量延迟加载；并让当前渲染器、变体和控制器值与路由状态保持同步。

部分公开组件会渲染完整 HTML 文档，或依赖根路径运行时文件。此类组件应从 `node_modules/@designcodeio/threeui/lib-dist/assets/` 复制必要文件到应用的 public 目录，或者使用已验证的 `sourceUrl`/`assetBaseUrl` 属性。除了开发根路径，还要从部署后的基础路径验证资源解析。

对于 WebGL 或只能在浏览器运行的组件，遵循宿主框架的客户端组件和 SSR 规则。提供加载和失败状态；组件卸载时释放渲染器资源；尽可能尊重减少动态效果设置；并确保关键信息和操作不依赖画布才能访问。

## 访问权限与许可证

公开仓库和 npm 包提供 Community 实现。不要声称 Pro 或 Beta 源码已公开。除非用户明确要求并确认拥有权限，否则不要通过 ThreeUI Pro CLI 登录、下载授权源码或覆盖项目文件。官方命令格式为：

```bash
npx @designcodeio/threeui-cli add <component-slug>
```

未经许可，不要把远程目录缩略图或预览作为本地资源重新分发。复用内容时保留 MIT、SIL Open Font License 和第三方声明。

## 验证要求

对于目录应用，运行项目已有的类型检查、Lint、测试和生产构建，然后确认：

- Browse、分类和标签筛选、搜索、详情路由、变体及文档链接均可用。
- 代表性 URL 可以直接打开，浏览器前进和后退状态正确。
- 浅色、深色和跟随系统主题，以及主题持久化行为正确。
- 移动端导航、键盘快捷键、焦点顺序、标签和减少动态效果行为正确。
- 预览加载和错误状态、WebGL 资源释放、资源 URL、控制台错误和横向溢出均已检查。
- 至少验证一个真实渲染器，以及一个带控制器的渲染器，而不只是模拟卡片。

如果 API 未验证、资源缺失、环境不支持 WebGL 或存在其他限制，必须明确报告，不要把视觉占位内容描述为完整功能。
