# 小红书尺寸水印联动功能实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 更新尺寸 Tab 标签为「小红书 1080×1440」，并在切换尺寸时自动更新水印文字

**Architecture:** 修改 HTML 标签文案，在 Tab 切换事件中添加水印文字联动逻辑

**Tech Stack:** 纯静态 HTML 应用，无构建系统

---

## File Structure

| File | Responsibility |
|------|----------------|
| `index.html` | 唯一文件，包含所有 HTML/CSS/JS |

---

### Task 1: 更新尺寸 Tab 标签文案

**Files:**
- Modify: `index.html` 尺寸 Tab HTML 区域（约 line 1225-1228）

- [ ] **Step 1: 更新 Tab 标签文案**

找到尺寸 Tab HTML（约第 1225-1228 行），将第二个按钮的文案从「1080×1440」改为「小红书 1080×1440」：

修改前：
```html
<!-- 尺寸选择 Tab -->
<div class="size-tabs">
  <button class="size-tab-btn active" data-size="900x1200">900×1200</button>
  <button class="size-tab-btn" data-size="1080x1440">1080×1440</button>
</div>
```

修改后：
```html
<!-- 尺寸选择 Tab -->
<div class="size-tabs">
  <button class="size-tab-btn active" data-size="900x1200">900×1200</button>
  <button class="size-tab-btn" data-size="1080x1440">小红书 1080×1440</button>
</div>
```

- [ ] **Step 2: 刷新浏览器验证 Tab 显示**

刷新页面，检查预览区尺寸 Tab 是否显示「小红书 1080×1440」标签。

---

### Task 2: 在 Tab 切换事件中添加水印文字联动

**Files:**
- Modify: `index.html` 尺寸 Tab 切换事件处理代码（约 line 1529-1545）

- [ ] **Step 1: 在 Tab 切换事件中添加水印文字更新逻辑**

找到尺寸 Tab 切换事件处理代码（约第 1529-1545 行），在保存 localStorage 后添加水印文字更新逻辑：

修改前：
```javascript
// 尺寸 Tabs 切换功能
document.querySelectorAll(".size-tab-btn").forEach((btn) => {
  btn.addEventListener("click", function () {
    // 更新 Tab 选中状态
    document.querySelectorAll(".size-tab-btn").forEach((b) => b.classList.remove("active"));
    this.classList.add("active");

    // 保存选择到 localStorage
    currentCardSize = this.dataset.size;
    localStorage.setItem('selectedCardSize', currentCardSize);

    // 如果已有生成的卡片，重新生成
    if (cardPreviewContainer.children.length > 0) {
      generateBtn.click();
    }
  });
});
```

修改后：
```javascript
// 尺寸 Tabs 切换功能
document.querySelectorAll(".size-tab-btn").forEach((btn) => {
  btn.addEventListener("click", function () {
    // 更新 Tab 选中状态
    document.querySelectorAll(".size-tab-btn").forEach((b) => b.classList.remove("active"));
    this.classList.add("active");

    // 保存选择到 localStorage
    currentCardSize = this.dataset.size;
    localStorage.setItem('selectedCardSize', currentCardSize);

    // 根据尺寸更新水印文字
    if (currentCardSize === '1080x1440') {
      watermarkTextInput.value = '科技阿研';
    } else {
      watermarkTextInput.value = '公众号:科技研习室';
    }

    // 如果已有生成的卡片，重新生成
    if (cardPreviewContainer.children.length > 0) {
      generateBtn.click();
    }
  });
});
```

- [ ] **Step 2: 刷新浏览器测试水印联动**

1. 刷新页面
2. 上传一张图片，点击「生成卡片」
3. 检查水印文字是否为「公众号:科技研习室」
4. 点击「小红书 1080×1440」Tab
5. 检查水印输入框是否自动变为「科技阿研」
6. 点击「900×1200」Tab
7. 检查水印输入框是否恢复为「公众号:科技研习室」

---

### Task 3: 提交完成

**Files:**
- Modify: `index.html`

- [ ] **Step 1: 确认功能正常**

验证：
1. Tab 标签显示正确
2. 切换尺寸时水印文字自动更新
3. 生成的卡片使用正确的水印

- [ ] **Step 2: 提交代码**

```bash
git add index.html
git commit -m "feat: add Xiaohongshu label and watermark text linkage for 1080x1440 size"
```

---

## Spec Coverage Check

| Spec Requirement | Task |
|------------------|------|
| Tab 标签更新为「小红书 1080×1440」 | Task 1 |
| 切换到小红书尺寸时水印为「科技阿研」 | Task 2 |
| 切换回默认尺寸时水印为「公众号:科技研习室」 | Task 2 |