# 图论

<center>
    author：liano
</center>
## 图的基本概念

### 顶点度数

##### 定理 1.1

> 任给无向图 G，有：
> $$
> \sum_{v\in V(G)} \deg{(v)}=2\varepsilon (G)
> $$

##### 推论 1.1

度数为奇数的顶点个数为偶数

### 路径与连通

##### 定理 1.2

> 图 G 是二分图, 当且仅当 G 中无奇圈

$\Rightarrow $
反证法

$\Leftarrow $
任取 $u_0 ∈ V (G)$, 定义
X = {u|u ∈ V (G), 且 $\text{dist}(u_0, u)$ 为偶数}，Y = {u|u ∈ V (G), 且 $\text{dist}(u_0, u)$ 为奇数}.

用反证法，若存在 u, v ∈ X，u 与 v 在 G 中相邻, 证明 G 中有奇圈

##### 例：

> 设 G 是简单图，$\delta (G) \ge 2$, 则 G 中含圈

设 $P(u, v) = v_0(= u)v_1\cdots v_k(= v)$ 是 G 中最长的轨道。

因为 $\deg(u) \ge \delta(G) \ge 2$，除 $v_1$ 外, u 在 G 中至少还有一个相邻的顶点, 设为 w。
若 w 不在 $P(u, v)$ 上，$P(u, v)$ 加上边 wu 就是 G 中比 $P(u, v)$ 还要长的轨道，与$P(u, v)$ 是最长轨道矛盾。

因而 w 在 $P(u, v)$ 上，设 $w = v_i(i \ne 0, 1)$，则 $P(u, v)$ 上的一段轨道 $P(u, v_i)$ 加上边 uvi 就是 G 中的圈. 证毕.

### 有向图

##### 定理 1.3

> 任给有向图 G，有
> $$
> \sum \deg^+(v)=\sum \deg^-(v)=\varepsilon(G)
> $$

$$
a^2=b^2+c^2
$$

