---
name: harmonyos-multidevice-adaptation
description: Audit, implement, or verify HarmonyOS ArkUI multi-device layouts. Use for 鸿蒙一多适配, foldable and tablet layouts, orientation and split-screen resizing, window breakpoints, navigation rails, and grids that clip or use too many columns.
---

# HarmonyOS 一多适配

让 ArkUI 页面在手机、折叠屏、平板及其不同窗口尺寸下保持可用。按用户要求做源码审查、代码适配或设备回归。

## 确认可用空间

- 从 `module.json5` 确认声明的设备类型，从构建配置确认兼容 API。读取页面入口、导航容器、内容栅格和弹层，找出已有的断点、宽高限制和窗口监听。优先复用项目的尺寸状态与布局约定。声明设备类型可以确定部署范围，页面表现需要运行验证。
- 用应用窗口的可用宽度决定全局导航和分栏，用分栏后的内容区宽度决定列数和卡片尺寸。排查窄页时记录当前窗口宽度、侧栏占宽及内容容器宽度。短横屏还需要检查可用高度和滚动范围。
- 已有断点时先验证阈值附近的表现。需要建立断点时，可以从 320、600、840 vp 起步，按标题、卡片内容、边距和间距实测调整。涉及折痕或悬停交互时，结合设备形态和实际避让区域处理。

## 调整布局

- 元素能在同一结构内伸缩时，优先复用现有的占比、权重、换行、滚动和最大宽度规则。列表或卡片达到最低可用宽度后再增加列数。一起检查长标题、系统大字体及短横屏下的底部操作。
- `GridRow` 默认按窗口宽度判断断点。如果它在导航侧栏或详情栏内因窗口档位较大而过早增列，考虑用 `breakpoints.reference: BreakpointsReference.ComponentSize` 让栅格按自身宽度判断。核对最窄档的列数及每个 `GridCol` 的跨度。按容器断点运行时，避免在断点回调中改变该容器自身的边距，以免反复跨过同一阈值。 [GridRow API](https://developer.huawei.com/consumer/cn/doc/harmonyos-references/ts-container-gridrow/)
- 从单栏变为侧栏或列表详情双栏时，复用导航栈、选中项和编辑状态。若布局切换会重建组件，把需要跨切换存活的状态交给稳定存在的上层组件或现有状态模型。用项目兼容的 SDK 核对新增 API。

## 验证运行效果

- 按项目原有入口构建和运行。依据适配范围选择手机竖屏、短横屏、折叠展开和合上、平板全屏及分屏。重点测试每个改动断点的两侧和同一次运行中的往返切换。
- 至少在一个受影响页面里保留选中项或输入内容后跨过断点，再执行返回或保存。确认内容可滚动、旧状态仍可用，软键盘弹出时关键按钮仍可到达。
- 记录设备或模拟器、窗口形态、操作和实际结果。有界面问题时，保留前后截图或对应的复现步骤。报告里注明哪些结论来自源码、构建或运行验证。 [华为多设备适配指南](https://developer.huawei.com/consumer/cn/multidevice/adaptive-apps/)
