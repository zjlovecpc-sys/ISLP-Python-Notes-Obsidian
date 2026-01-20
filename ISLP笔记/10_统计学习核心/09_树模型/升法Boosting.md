---
tags:
  - 统计学习
  - 树模型
  - 集成学习
  - Boosting
aliases:
  - Gradient Boosting
  - AdaBoost
  - 学习率
  - 收缩参数
---

# 提升法 (Boosting)

## 1. 核心思想：向错误学习
与 Bagging 构建独立的树不同，Boosting 的树是**顺序生长 (Sequentially Grown)** 的。
* **基本逻辑**：每一棵新树都不是为了拟合原数据，而是为了**拟合现有模型的残差 (Residuals)**。
* **直觉**：新树专注于修正之前所有树犯下的错误。

## 2. 算法流程 (以回归为例)
1.  初始化 $\hat{f}(x) = 0$，残差 $r_i = y_i$。
2.  对于 $b = 1, \dots, B$:
    * 拟合一棵只有 $d$ 个分裂（深度）的树 $\hat{f}^b$ 到残差 $(X, r)$ 上。
    * 更新模型：$\hat{f}(x) \leftarrow \hat{f}(x) + \lambda \hat{f}^b(x)$。
    * 更新残差：$r_i \leftarrow r_i - \lambda \hat{f}^b(x_i)$。
3.  输出最终模型：$\hat{f}(x) = \sum_{b=1}^B \lambda \hat{f}^b(x)$。

## 3. 三大调节参数 (Tuning Parameters)
Boosting 极度依赖调参，不像随机森林那样“开箱即用”。

1.  **树的数量 $B$**：
    * 随机森林 $B$ 越大越好（不会过拟合）。
    * Boosting **会过拟合**！$B$ 太大通过拟合噪声导致过拟合。需通过 CV 选择。
2.  **收缩参数 $\lambda$ (Learning Rate)**：
    * 控制学习速度。通常为 0.01 或 0.001。
    * $\lambda$ 越小，通常需要 $B$ 越大。这种“慢速学习”通常效果更好。
3.  **交互深度 $d$ (Interaction Depth)**：
    * 控制每棵树的复杂度（分裂次数）。
    * 通常 $d=1$ (树桩, Stump) 就很有效，意味着只拟合加性模型。$d$ 控制了变量间交互作用的阶数。

## 关联笔记
* ➡️ 实战工具：[[XGBoost与LightGBM实现（待补）]]
* 🔄 对比：[[装袋法与随机森林]] (并行 vs 串行，降方差 vs 降偏差)