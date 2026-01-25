---
title: "First Post: Test"
date: 2026-01-23
draft: false
tags:
  - Say hi
# --- 封面图设置 ---
cover:
    image: kim.jpg      # 这里的图片名一定要和文件名一模一样（区分大小写！）
    alt: "这是封面描述"
    caption: "这是底部的文字-三刷才发现老金那么酷-图源：折中-https://www.artstation.com/artwork/3EV6oJ"
    relative: true      # 确保这一行缩进对齐，且为 true
---

## 以防我忘记的使用指南

各种基本添加方法
### 文件结构
同一篇中的文字图片放一个文件夹，时间命名。
### 文字内容
直接打字。如果要**加粗**，就用两个星号包起来；如果要*斜体*，就用一个星号。

---

### 如何在正文中插入图片：

现在我们使用 **Page Bundle** 结构，图片就在文章旁边，不需要写 `/images/...` 了。

假设文件夹里有一张图叫 `tequila-sunrset.jpg`，你就这样写：

![Un Jour JE serai de retour près de Toi（图源网络）](/tequila-sunrset.jpg)

*/。*

---

### 还可以写代码块：

```css
body {
  color: red;
}