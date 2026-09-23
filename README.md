# 加州褐鹈鹕高速骑行（SVG + SMIL）

一个完全自包含的单文件网页动画：侧面视角的加州褐鹈鹕骑着自行车高速前进。

- 车轮、曲柄踏板、鹈鹕双腿使用 **SVG 原生 SMIL** 无限循环动画（周期 0.6s），节奏偏快
- 三层视差背景（远景云朵 / 中景海岸山丘 / 近景地面与小石子）自动滚动
- 点击「晴天 / 阴天 / 雨天」按钮由 **JavaScript** 切换天气，切换时动画与背景滚动不会重置或闪烁
- 不引用任何外部图片、音频、CSS 或 JS 库，**双击 `pelican-bike.html` 即可打开**

## 文件

| 文件 | 说明 |
| --- | --- |
| `pelican-bike.html` | 全部内容：SVG 图形 + SMIL 动画 + 内联 CSS / JS |

---

## 分支冲突练习

本仓库预置了两个分支，专门用来练习**同一行代码冲突**的处理：

| 分支 | 修改内容（与 main 相比） |
| --- | --- |
| `feature-a` | 第 6 行 `<title>`、`.stage` 的宽度 |
| `feature-b` | **同样这两行**，但改成了不同的值 |

两个分支改的是同一批行，所以第二个分支合并时必然冲突。

### 一、制造冲突

```bash
git switch main
git merge --no-ff feature-a -m "merge feature-a"   # 第一次合并，顺利通过
git merge feature-b                                # 第二次合并 -> 冲突出现
```

冲突时 Git 会提示：

```
Auto-merging pelican-bike.html
CONFLICT (content): Merge conflict in pelican-bike.html
Automatic merge failed; fix conflicts and then commit the result.
```

### 二、查看冲突

```bash
git status                 # 看到 "both modified: pelican-bike.html"
git diff                   # 查看冲突标记
```

文件里会出现这样的冲突标记：

```
<<<<<<< HEAD
<title>加州褐鹈鹕高速骑行 · 晴天巡航版</title>
=======
<title>加州褐鹈鹕高速骑行 · 雨天冲刺版</title>
>>>>>>> feature-b
```

含义：`<<<<<<< HEAD` 到 `=======` 之间是**当前分支（main）**的内容，
`=======` 到 `>>>>>>> feature-b` 之间是**要合进来的分支**的内容。

### 三、解决冲突

用编辑器打开 `pelican-bike.html`：

1. 手动决定这一行最终要什么内容（留一边、留另一边，或者两边融合成一句）
2. **删掉 `<<<<<<<`、`=======`、`>>>>>>>` 这三行标记**
3. 两处冲突都要处理，处理完检查文件里已经没有任何 `<<<<<<<` 或 `>>>>>>>`

```bash
# 确认标记已清干净（没有输出即正确）
grep -n "<<<<<<<\|>>>>>>>\|=======" pelican-bike.html

git add pelican-bike.html
git commit -m "合并 feature-b 并解决同一行冲突"
```

### 四、收尾

```bash
git log --oneline --graph --all     # 看合并图
git branch -d feature-a feature-b   # 删除练习分支（可选）
```

### 五、中途放弃怎么办

```bash
git merge --abort        # 回到合并前的状态，可以重来
```

> 小提示：冲突并不可怕，它只是 Git 在说「这两处改动我没法替你决定，你来定」。
> 解决冲突的本质就是：手工编辑文件 → 删掉标记 → `git add` → `git commit`。
