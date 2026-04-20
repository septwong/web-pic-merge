---
name: xiaohongshu-size-watermark
description: Add Xiaohongshu label to 1080x1440 tab and auto-set watermark text when switching sizes
type: project
---

# 小红书尺寸水印联动功能设计

## 概述

在卡片尺寸选择功能基础上，为小红书尺寸添加平台标识，并在切换尺寸时自动更新水印文字。

## Why

1080×1440 尺寸专用于小红书平台，需要明确标注。切换到小红书尺寸时，水印应自动改为对应的品牌「科技阿研」，方便用户使用。

**How to apply:** 更新 Tab 标签文案，并在尺寸切换逻辑中添加水印文字联动。

## UI 设计

### Tab 标签更新

| Tab | 原标签 | 新标签 |
|-----|-------|--------|
| 默认 | 900×1200 | 900×1200（保持不变） |
| 小红书 | 1080×1440 | 小红书 1080×1440 |

### 水印文字联动

切换尺寸时自动更新水印输入框内容：

| 尺寸 Tab | 水印文字 |
|---------|---------|
| 900×1200 | 公众号:科技研习室 |
| 小红书 1080×1440 | 科技阿研 |

## 功能逻辑

### Tab 切换行为

1. 用户点击尺寸 Tab
2. 更新 Tab 选中状态
3. 更新 `currentCardSize` 变量
4. **根据尺寸设置水印文字**
5. 保存到 localStorage
6. 如果已有卡片，重新生成

### 水印文字设置逻辑

```javascript
if (currentCardSize === '1080x1440') {
  watermarkTextInput.value = '科技阿研';
} else {
  watermarkTextInput.value = '公众号:科技研习室';
}
```

## 技术实现

### 修改范围

1. **HTML**：更新尺寸 Tab 标签文案（line ~1225-1228）
2. **JavaScript**：在尺寸 Tab 切换事件中添加水印文字更新逻辑（line ~1529-1545）

### 关键代码修改

尺寸 Tab 切换事件处理中，在保存 localStorage 后添加：

```javascript
// 根据尺寸更新水印文字
if (currentCardSize === '1080x1440') {
  watermarkTextInput.value = '科技阿研';
} else {
  watermarkTextInput.value = '公众号:科技研习室';
}
```

## 兼容性

- 水印文字更新覆盖用户当前输入（按用户要求）
- 不影响其他水印设置（颜色、透明度、大小、位置）
- localStorage 中不保存水印文字（每次切换时重新设置）