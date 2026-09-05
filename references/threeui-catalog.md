# ThreeUI 目录参考

只有在构建或大幅扩展 ThreeUI 风格的目录应用时，才读取本参考文档。

## 信息架构

公开应用可以围绕以下页面状态组织：

- `browse`：全部目录条目，可按分类或标签筛选
- `shader`：单个条目的实时预览和文档
- `installation`：安装和包使用说明
- `mcp`：适用时提供产品集成文档
- `not-found`：可恢复的路由错误，并提供返回 Browse 的链接

应用外壳包含顶部栏、导航侧边栏、一个可滚动的主内容区、页脚和搜索弹窗。在移动端，侧边栏变为带遮罩层的抽屉。

## 注册表模型

使用数据驱动的注册表。可以采用以下条目结构：

```ts
type CatalogEntry = {
  id: string;
  variantOf?: string;
  category: string;
  label: string;
  thumbnail?: string;
  preview?: string;
  tags: string[];
  description: string;
  runtime: string;
  origin?: string;
  sourceCommit?: string;
  sourceFiles?: string[];
  passes?: string;
  interaction?: string;
  asset?: string;
  assetCount?: number;
  importName?: string;
  contract?: ContractRow[];
  controls?: Control[];
  variants?: Variant[];
};

type ContractRow = {
  name: string;
  type: string;
  value: string;
};

type Variant = {
  id: string;
  label: string;
  description: string;
  thumbnail?: string;
  preview?: string;
  props?: Record<string, boolean | number | string>;
  controls?: Control[];
};
```

公开模型支持的控制器类型包括：

- `range`：带最小值、最大值、步长和显示精度的数值控制器
- `choice`：带标签的选项控制器
- `checkpoint`：用于检查点或预设的离散选项
- `color`：颜色值控制器
- `text`：文本值控制器，可选最大长度和占位文本

以上是设计模型，不是虚构元数据的理由。只暴露实际渲染器支持的字段。

## 交互契约

使用单一来源管理路由状态。选择条目时应：

1. 解析基础条目和可选变体；
2. 更新当前预览和控制器；
3. 推送或替换规范 URL；
4. 关闭搜索弹窗和移动端导航抽屉；
5. 直接加载 URL 或使用浏览器历史记录时恢复相同状态。

选择分类或标签时，应清除互不兼容的筛选条件，并生成稳定 URL。搜索选择条目时，应复用侧边栏和 Browse 卡片使用的同一套路由切换逻辑。

主题状态在产品需要时支持 `light`、`dark` 和 `system`。将解析后的外观应用到文档；在 `system` 模式监听系统外观变化；并容忍 `localStorage` 不可用的情况。

## 预览布局

详情页应让渲染器成为主要视觉区域，说明和控制器围绕它布局，不能遮挡关键交互。应包含：

- 延迟加载渲染器时显示明确的加载状态；
- 错误边界或可恢复的错误状态；
- 仅在存在变体时显示变体选择器；
- 根据条目或变体元数据生成控制器；
- 可复制且已验证的源码或导入信息；
- 与所选包和框架匹配的安装说明。

对于文档型渲染器，隔离文档预览，避免其 CSS 和运行时污染目录外壳。对于画布型渲染器，根据容器尺寸设置画布，处理尺寸变化和设备像素比，并在卸载时清理监听器和动画循环。

## 视觉方向

采用克制、编辑感强的开发者工具界面：清晰的文字层级、安静的表面、紧凑的导航、充足的预览空间、明确的元数据，以及用于传达状态的动效。预览和控制器应保持视觉主导。根据用户产品调整颜色和品牌，不要复制 ThreeUI 的标识、原文案或远程艺术资源。

## 最小验收场景

在宣布目录完成前，手动执行以下流程：

1. 在窄屏视口打开 Browse。
2. 使用 `Ctrl/Cmd+K` 打开搜索，搜索一个已验证条目并进入详情。
3. 修改一个变体和至少一个控制器。
4. 重新加载详情 URL，再使用浏览器后退返回 Browse。
5. 切换主题，关闭并重新打开移动端抽屉，确认没有控制台错误或横向溢出。
