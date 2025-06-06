---
title: GNU unified diff format
date: 2025-05-28
type: blog
---

**GNU diff format** 是 GNU 版本的 `diff` 工具输出的**差异格式**之一，常用于对比两个文件的不同之处。这个格式是很多版本控制系统（如 Git）和补丁工具（如 `patch`）的基础。

---

### 一、GNU diff format 的基本样式

最常见的格式是 **"Unified Diff Format"（统一差异格式）**，也叫 **"unified format"**，是 GNU diff 工具的默认输出之一。

它的结构大致如下：

```
--- old_file.txt  2025-05-28 12:00:00
+++ new_file.txt  2025-05-28 12:05:00
@@ -1,4 +1,5 @@
 line 1
-line 2
+new line 2
 line 3
 line 4
+line 5
```

---

### 二、格式说明

* `---` 表示原文件，后面是文件名和时间戳。
* `+++` 表示新文件。
* `@@ -1,4 +1,5 @@` 这一行是 **hunk header**，表示：

  * `-1,4`：原文件从第 1 行开始，4 行
  * `+1,5`：新文件从第 1 行开始，5 行
* 每一行前面的符号意义：

  * ` ` 空格：该行在两个文件中都存在（上下文）
  * `-`：只在旧文件中有，被删除
  * `+`：只在新文件中有，新增

---

### 三、用途

GNU diff format 广泛用于：

* **Git** 提交差异展示（即 `git diff`）
* **补丁生成与应用**（通过 `diff` 生成补丁，再用 `patch` 应用）
* 比较配置文件、程序代码、文档版本差异

---

### 四、生成示例

```bash
diff -u old.txt new.txt
```

输出就是 GNU unified diff format。

---

如你有一个 `.diff` 或 `.patch` 文件，那基本就是用这种格式写成的。你也可以用 `patch` 命令自动应用它来更新文件：

```bash
patch < my_patch.diff
```

---

