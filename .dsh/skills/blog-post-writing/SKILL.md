---
name: blog-post-writing
description: >
  REQUIRED for creating, editing, or reviewing any file under _posts/ in this Jekyll
  knowledge base (octhe.github.io). Supplies the notation and math conventions, formula
  source formatting, punctuation rules, complexity accounting, verification workflow, and
  collaboration etiquette for post files. Do NOT use it for the rest of the repository:
  _includes/, _layouts/, _config.yml, assets/, Gemfile, build tooling, or git operations
  follow their own rules and are out of scope. Triggers: writing or editing a post,
  写文章, 改公式, 记号约定, 伴随矩阵, 共轭转置, 行列式 det, 复杂度统计, 实实乘法/实复乘法/复复乘法,
  倒数平方根, MathJax 编号, 正文排版, 标点符号, 一句话一行, diff 与验证.
---

# 知识库文章写作 Skill

## 适用范围

本 skill **只**用于 `_posts/` 下的文章文件（Markdown 正文与 front matter）。
其它位置不在范围内：`_includes/`、`_layouts/`、`_config.yml`、`assets/`、`Gemfile`、构建脚本、git 操作，都按各自的实际情况处理，不要套用本文的排版与验证流程。
文章里出现的站点级配置（例如 MathJax 编号）只作为背景知识，不代表可以顺手去改模板文件。

这份清单来自实际协作中被反复纠正的地方，写给 AI 助手，也写给未来的自己。
动手写或改之前先看第一节的速查清单。

## 一、速查清单

1. 矩阵元素写成带下标的普通小写字母（$$a_{ij}$$），不要另起 $$p$$、$$q$$、$$u$$、$$v$$、$$w$$ 这类单字母。
2. 矩阵与向量用粗体，标量用普通斜体；伴随写 $$\mathbf{A}^*$$，共轭转置写 $$(\cdot)^H$$。
3. 行列式一律写 $$\det(\cdot)$$，不要用 $$\Delta_2$$、$$\Delta_R$$ 这类希腊字母简写。
4. 一条公式一行，一式一编号；多行结构用 `\begin{cases}` 大括号，两行也括。
5. 公式源码要留空格（等号两侧、二元运算符两侧、因子之间），续行以 `=` 或 `-` 顶格。
6. 中间变量能省就省；不要用 $$w$$（那是 MMSE 权重矩阵），不要拿一个分量的结果当另一个的中间变量。
7. 只用键盘能打出来的字符；正文一句话一行；约定与口径都用表格。
8. 改完要构建、要渲染校验、公式要数值对拍；用户的改动优先，不要整文件重写。

## 二、记号约定

> **记号约定**
>
> | 记号 | 含义 |
> |:---:|:---|
> | $$\mathbf{H}$$、$$\mathbf{x}$$ | 矩阵与向量，一律用**粗体** |
> | $$a_{ij}$$、$$\sigma^2$$ | 标量用普通斜体；矩阵元素写成带下标的普通小写字母 |
> | $$\mathbf{A}^*$$ | $$\mathbf{A}$$ 的**伴随矩阵** |
> | $$(\cdot)^H$$ | **共轭转置**（作用在元素上就是取共轭） |
> | $$\det(\cdot)$$ | 行列式 |
> | $$\lvert\cdot\rvert$$ | 取模 |

## 三、公式怎么写

### 3.1 一条公式一行，一式一编号

不要把几条公式挤在一行里省纸，一条一行。
多行结构（例如方程组的两个分量）用 `\begin{cases}` 括起来，哪怕只有两行也用。
这样每个公式都有独立编号，正文里引用「(21)」「(28)」时不会含糊。

### 3.2 公式源码要留空格

MathJax 会忽略 `\text{}` 之外的所有空白，所以在源码里加空格只影响可读性、不影响渲染。
下面这条是目标样子：

$$
\mathbf{R} = \det(\mathbf{A}_{22}) \mathbf{A}_{11} - \mathbf{A}_{12} \mathbf{A}_{22}^* \mathbf{A}_{12}^H
$$

对照规则：

| 位置 | 改前 | 改后 |
|:---|:---|:---|
| 等号 | `\mathbf{R}=\det(...)` | `\mathbf{R} = \det(...)` |
| 二元加减 | `\mathbf{A}_{11}-\mathbf{A}_{12}` | `\mathbf{A}_{11} - \mathbf{A}_{12}` |
| 一元负号 | `&-a_{34}` | `&-a_{34}`（不动） |
| 因子之间 | `\mathbf{A}_{22}^*\mathbf{A}_{12}^H` | `\mathbf{A}_{22}^* \mathbf{A}_{12}^H` |
| 上标后接字母 | `a_{12}^Hz_1` | `a_{12}^H z_1` |
| 字母后接宏 | `r\det(\mathbf{R})` | `r \det(\mathbf{R})` |
| 关系符 | `2\to24` | `2 \to 24` |
| 续行 | ` =\det(...)` | `= \det(...)`（顶格） |

## 四、文字与标点

- 只用键盘能直接打出来的字符。
  带圈数字、直角引号这类符号改用普通写法：阶段编号写成 （1）（2）（3），引号写成 “ ”。
- 正文一句话一行，同一段落内用换行分开，方便 diff 和逐句修改。
- 记号约定、统计口径、逐项账目都用表格，不要摊成大段文字。

## 五、协作方式

- **用户的改动优先**。
  文件可能正被 vim 编辑，动手前先比对 `.md` 与 `.swp` 的时间戳，改完回看 diff。
- **定点替换**。
  用带唯一性断言的字符串替换，不要重新生成整个文件，否则会覆盖用户的手改。
- **不擅自恢复**。
  用户删掉的内容不要自己塞回去，先问一句。
- **发现错误直接指出**。
  例如 $$\mathbf{R}^* = \det(\mathbf{R}) \mathbf{R}^{-1}$$，不是 $$\det(\mathbf{R}) \mathbf{R}$$。
