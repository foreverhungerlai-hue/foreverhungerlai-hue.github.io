---
title: "Test"
date: 2026-01-23
draft: false
tags:
  - Say hi
# --- 下面是给文章加封面图的方法 (可选) ---
cover:
  image: "/images/kim.jpg"  # 封面图路径 (放在 static/images 下)
  alt: "这是封面描述"
  caption: "这是图片底部的文字-三刷才发现老金也很棒"
  relative: false 
---

## 以防我忘记的使用指南

各种基本添加方法

### 文字内容
直接打字。如果要**加粗**，就用两个星号包起来；如果要*斜体*，就用一个星号。

---

### 如何在正文中插入图片：

假设你把一张叫 `tequila-sunrset.jpg` 的图片放到了 `static/images/` 文件夹里，你就这样写：

![Un Jour JE serai de retour près de Toi ](/images/tequila-sunrset.jpg)

*注意：图片路径必须以 `/` 开头，代表从网站根目录开始找。*

---

### 还可以写代码块：

```css
body {
  color: red;
}