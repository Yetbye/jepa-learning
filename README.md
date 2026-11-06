# JEPA Learning

两件东西,同一条线索。

**① 一份从零开始的数学课**:从"什么是损失函数"讲到"对抗攻击与 argmin 是同一个东西",全部公式手工推导,配可交互演示。

**② 一篇论文**:把课程最后一章留下的问题——为什么层间结论不可比——写成一个可证的分解与一个数小时的判别性检验。

🔗 <https://yetbye.github.io/jepa-learning/>

---

## 目录

| 路径 | 内容 |
|---|---|
| [`index.html`](index.html) | 数学课:15 章 + 代码附录,公式逐个推导,3 个可交互 Canvas 演示 |
| [`papers/ridge-probe/`](papers/ridge-probe/index.html) | 论文《层间线性探针排序的正则化依赖:精确分解与可复现性—信号量权衡》 |
| [`taomate/`](taomate/index.html) | 论文精读:TaoMate,锚点引导的记忆与阶段并行推理 |

## 论文:核心主张

层间线性探针的排序不是一个免参数的量。本文把它拆成两部分:

| 量 | 含义 | 性质 |
|---|---|---|
| `R²(λ, train)` | 训练集上的岭探针分数 | 有精确闭式 Σ_j τ_j(2−τ_j)w_j,验证至 5×10⁻¹⁶ |
| `R²(λ, test)` | held-out 分数 | 无法由同一闭式表达;泛化比从 0.963 降到 0.739 |

**命题 1**:λ·R² → 2C,其中 C = ‖ZᵀX‖²_F / ‖X‖²_F 只依赖训练集。强正则下的层序因此退化为 C 的排序。

### 实测结论

在 DINOv2-base / NYU 深度、654 幅图像、5.03×10⁵ 训练 token、三个 75/25 划分上:

- 跨划分层序 Kendall τ 随正则从 **0.726 升到 1.000**
- 但 τ ≥ 0.95 时最优层 R² 只剩近 OLS 的 **26%**
- 且此时的排序收敛到 C 的排序(τ 对 C:0.590 → 0.974)
- 近 OLS 处相邻层差距只有种子标准差的 **0.12 倍** —— "最优层"不可辨识

### 这说明了什么

"哪一层最好"在近 OLS 下不可复核;要让它可复核,必须牺牲信号,而那个可复核的排序来自另一个泛函,不是可解码性本身。因此层序应当与"它在何种测量下成立"一起报告。

### 与已有工作的分工

探针容量/复杂度轴已由 [Voita & Titov](https://arxiv.org/abs/2003.12298)、[Hewitt & Liang](https://arxiv.org/abs/1909.03368)、[Belinkov](https://arxiv.org/abs/2102.12452) 处理;"线性可读出是否蕴含线性表征"由 [Kutluozen et al.](https://arxiv.org/abs/2610.22655) 处理;超参敏感这一现象由 [Lee et al.](https://arxiv.org/abs/2304.03456)、[Mordacq et al.](https://arxiv.org/abs/2606.03338) 报告。本文处理的是**正则强度对层间排序的影响**,给出闭式刻画与实测权衡。

### 状态

15 条参考文献全部经原文摘要核实。全部数值为实测或已证,无 `to be run` 占位;逐项溯源见 [EVIDENCE.md](papers/ridge-probe/EVIDENCE.md),术语来源见 [TERMINOLOGY.md](papers/ridge-probe/TERMINOLOGY.md)。

## 技术说明

- MathJax 3.2.2 已本地化(`assets/mathjax/tex-svg.js`),两个页面均离线可用,不依赖 CDN。
- 页面为单文件 HTML,图表为内联 SVG,无外部图片。
- 论文按 `academic-paper-html` 技能的两道门控产出:术语来源表、数值状态表。

## 部署

GitHub Pages 由 `.github/workflows/deploy.yml` 自动部署 `main` 分支根目录。
