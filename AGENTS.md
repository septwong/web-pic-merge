# AGENTS.md

本文件为在本仓库内工作的代码代理提供项目约定与维护指引。

## 项目概览

`web-pic-merge` 是一个纯静态的图片卡片生成网页。它通过浏览器端 Canvas 完成图片合成、背景渐变、文字、水印、二维码拼接、iPhone 套壳、PNG 下载与剪贴板复制。

项目没有构建系统、包管理器或后端服务。主要实现都在 `index.html` 中，刷新浏览器即可看到改动。

## 项目结构

- `index.html`：核心单页应用，包含内联 CSS、HTML 结构和全部 JavaScript 逻辑。
- `README.md`：项目简介与示例图。
- `CLAUDE.md`：旧的代码代理说明，可作为历史参考；以本文件为准。
- `favicon.svg`、`logo.svg`：页面图标与标题 Logo。
- `images/`：示例图和 iPhone 边框资源。
  - `iphone-border-16promax.png`：默认 iPhone 16 Pro Max 边框。
  - `iphone-border-15promax.png`：备用 iPhone 15 Pro Max 边框。
  - `example*.png`：README 和测试用示例图片。
- `docs/`：当前为空，后续文档可放在这里。

## 运行方式

无需安装依赖。直接用浏览器打开 `index.html` 即可。

如果需要通过本地 HTTP 服务验证剪贴板、文件加载或浏览器权限行为，可以在仓库根目录启动一个简单静态服务，例如：

```bash
python3 -m http.server 8000
```

然后访问 `http://localhost:8000/`。

## 主要功能入口

`index.html` 内部大致分为三段：

- CSS：页面布局、响应式适配、面板和预览样式。
- HTML：两大主 Tab，分别是“卡片生成”和“iPhone套壳”。
- JavaScript：文件上传、状态管理、Canvas 绘制、下载复制、弹窗与 Toast。

重点函数：

- `generateCard()`：生成普通卡片。根据当前尺寸、上传图片、二维码、文字、水印和样式设置绘制 Canvas。
- `generateIPhoneCards()`：生成 iPhone 套壳图片。
- `drawIPhoneMockupForCanvas()`：将主图裁进 iPhone 边框并绘制阴影。
- `drawBackground()`：绘制普通卡片背景渐变。
- `drawCardArea()`：绘制卡片区域和阴影。
- `drawWatermark()`：绘制平铺或定位水印。
- `extractDominantColors()`：从上传图片中提取较鲜明的颜色，用于自动背景。
- `wrapTextImproved()`、`wrapChineseText()`、`wrapEnglishText()`：标题换行。

## 状态与尺寸约定

- 普通卡片尺寸由 `currentCardSize` 控制，当前支持：
  - `900x1200`
  - `1080x1440`，标注为“小红书 1080×1440”
- `currentCardSize` 和主 Tab 选择会写入 `localStorage`：
  - `selectedCardSize`
  - `selectedMainTab`
- 普通卡片会复用页面中的隐藏 `<canvas id="canvas">`。
- iPhone 套壳每张图会创建独立的临时 Canvas，尺寸为 `1400x2820`。
- 小红书尺寸当前不显示二维码，默认水印为 `科技阿研`；其他尺寸默认水印为 `公众号:科技研习室`。
- 上传的主图和二维码文件会按文件名中的数字自然排序，逻辑在 `naturalSortComparer()` 中。

## 修改指南

- 尽量保持单文件静态应用的形态，不要引入构建链、框架或依赖，除非用户明确要求。
- 修改 UI 时同步检查 CSS、HTML id/class 和 JavaScript 中的 `document.getElementById` 引用。
- 新增控件时，需要同时处理：
  - HTML 控件本体。
  - 初始值和显示值。
  - 事件监听。
  - `resetForm()` 或 `resetIPhoneForm()` 中的重置逻辑。
  - 生成函数中的实际绘制逻辑。
- Canvas 绘制前后注意使用 `ctx.save()` / `ctx.restore()`，避免阴影、透明度、裁剪路径、旋转等状态泄漏到后续绘制。
- 修改尺寸、边距或 iPhone 边框定位时，务必用真实图片验证导出结果，不只看页面布局。
- 复制图片依赖 `navigator.clipboard.write` 和 `ClipboardItem`，部分浏览器或非安全上下文可能失败；保留下载作为兜底路径。
- `ctx.roundRect` 已被使用；如需支持更旧浏览器，应改用现有 `roundRect()` helper 或增加兼容处理。

## 资源约定

- 边框和示例图片放在 `images/` 下，路径在 HTML 中以相对路径引用。
- 替换 iPhone 边框资源时，检查 `drawIPhoneMockupForCanvas()` 中的边框缩放、截图区域、圆角和阴影参数。
- 保持 SVG 图标轻量，PNG 资源尽量控制体积，避免显著拖慢首次打开。

## 手动验证清单

修改后至少验证以下路径：

1. 直接打开 `index.html`，页面无控制台错误。
2. 上传多张主图，点击“生成卡片”，预览数量与主图数量一致。
3. 上传二维码后生成 `900x1200` 卡片，主图和二维码布局正常。
4. 切换到“小红书 1080×1440”后生成卡片，二维码不显示，水印默认值正确。
5. 测试样式预设、随机渐变、自动取色、标题换行、水印开关和水印位置。
6. 点击预览图能打开大图弹窗，按 ESC 或点击关闭能关闭弹窗。
7. 下载按钮能导出 PNG，复制按钮失败时有可理解的提示。
8. 切换到“iPhone套壳”，上传图片并生成，边框、截图圆角和背景正常。
9. 在窄屏宽度下检查表单、预览、按钮没有明显重叠或溢出。

## 代码风格

- 现有代码以原生 HTML/CSS/JavaScript 为主，继续沿用这个风格。
- 注释可以使用中文，优先解释复杂 Canvas 坐标、状态或兼容性原因。
- 保持用户可见文案为中文。
- 避免做与请求无关的大规模重排或重构；`index.html` 较大，改动时尽量保持局部、可回看。
