# 项目报告 - 组队名片生成器

## 本人负责部分

本项目为黑客松单人项目，本人负责全部工作：

- **需求分析与设计**：确定产品定位（黑客松组队场景）、核心功能（表单填写、实时预览、主题切换、导出分享）、交互流程
- **前端开发**：HTML 结构搭建、TailwindCSS 样式编写、JavaScript 交互逻辑实现
- **功能实现**：表单实时同步、双主题切换、html2canvas 图片导出、Clipboard API 文字复制
- **边界处理**：文字超长换行、空输入占位提示、必填字段校验、XSS 防护、剪贴板降级方案
- **文档撰写**：README.md（技术文档）、report.md（项目报告）、演示脚本

## AI 使用情况

本项目开发过程中使用了 AI 辅助（Qoder），主要体现在：

- **代码生成**：AI 辅助生成 index.html 的完整代码框架，包括 HTML 结构、TailwindCSS 样式、JavaScript 交互逻辑
- **技术方案建议**：AI 提供了 html2canvas 导出方案、Clipboard API + 降级方案的技术选型建议
- **边界场景分析**：AI 协助识别了 html2canvas 渲染兼容性、Clipboard API 浏览器兼容性、TailwindCSS CDN 生产限制等边界风险
- **文档撰写**：AI 辅助撰写 README.md 和 report.md

**人工工作**：需求定义、功能设计、交互流程、代码审查与调试、最终交付物整理

## 最大困难与解决方式

### 困难：html2canvas 导出图片的稳定性

**问题描述**：html2canvas 在将 DOM 转换为 Canvas 时，对于复杂 CSS 样式（特别是渐变背景、`backdrop-filter`、`box-shadow`）的渲染存在兼容性问题。初版实现中，技能标签的毛玻璃效果（`backdrop-filter: blur(4px)`）在导出图片中完全丢失，导致视觉效果与预览不一致。

**解决方式**：

1. **简化 CSS 特效**：将技能标签的 `backdrop-filter` 改为半透明背景色（`rgba(255, 255, 255, 0.2)`），在保持视觉效果的同时确保 html2canvas 能正确渲染
2. **调整渲染参数**：html2canvas 配置中使用 `scale: 2` 提高导出图片清晰度，设置 `backgroundColor: null` 保留渐变背景
3. **测试验证**：在 Chrome、Firefox、Edge 三个浏览器中测试导出效果，确保跨浏览器一致性

**经验总结**：在使用 html2canvas 时，应避免使用其不支持的 CSS 特性（如 `backdrop-filter`、`mix-blend-mode`），优先选择兼容性好的样式方案。对于黑客松演示场景，视觉效果的"可导出性"比"炫酷程度"更重要。
