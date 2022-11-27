# 复变函数

<center>
    author：liano
</center>

## 复函数

### 调和函数

$$
u_{x,x}+u_{y,y}=0
$$

### C-R方程

复函数 $f(z)=u(x,y)+\text{i}v(x,y)$ 的可微条件：

- $u,v$ 分别可微

- 满足 C-R 方程：
    $$
    u_x=v_y\\
    u_y=-v_x
    $$
    

### 三角函数

复数形式：
$$
\sin{z}=\frac{e^{\text{i}z}-e^{-\text{i}z}}{2\text{i}}\\
\cos{z}=\frac{e^{\text{i}z}+e^{-\text{i}z}}{2}\\
\sinh{z}=\frac{e^{z}-e^{-z}}{2}\\
\cosh{z}=\frac{e^{z}+e^{-z}}{2}
$$
转换关系：
$$
\sin{(\text{i}z)}=\text{i}\sinh(z)\\
\cos{(\text{i}z)=\cosh{z}}
$$

$$
\cosh^2{z}-\sinh^2{z}=1
$$

> 小技巧：对于 $f(z)=f(x,y)$ 的形式，令 $x=z,y=0$ 可以快速得到 $f(z)$
>

### 指数函数

$$
e^{\text{i}\theta}=\cos{\theta}+\text{i}\sin{\theta}=e^{\text{i}(\theta+2k\pi)}
$$

$$
e^z=e^{x+\text{i}y}=e^x\cdot e^{\text{i}y}\\
Ln(z)=\ln{|z|}+\text{i}(\arg(z)+2k\pi),\quad \text{主值}\ln(z)=\ln{|z|}+\text{i}\arg{z}
$$

## 复积分计算

### 换元

设曲线 $C：z(t)=x(t)+\text{i}y(t),\quad a\leq t\leq b$ 简单光滑，则 $z^{'}(t)=x^{'}(t)+\text{i}y^{'}(t)$，且有：
$$
\int_Cf(z)dz=\int_a^bf\big(z(t)\big)z^{'}(t)dt
$$

### 牛-莱公式

若 $f(z)$ 在单连通区域 D 内解析，对 $f(z)$ 的任一原函数 $H(z)$，有：
$$
F(z)=\int_{z_0}^{z}f(z)dz=H(z)-H(z_0)
$$

###  结论

设 n 是整数，a 是曲线 C 内部任一点，则有：
$$
I=\int_C\frac{dz}{(z-a)^n}=
\begin{cases}
2\pi\text{i},	& n=1\\
0,	& n\neq1
\end{cases}
$$

### 长大不等式

$$
\Big|\int_Cf(z)dz\Big|\leq\int_C|f(z)|ds=\int_C|f(z)|\cdot|dz|=\int_a^b|f\big(z(t)\big)|\cdot|z^{'}(t)|dt
$$

### 柯西积分

#### 定理

$f(z)$ 在闭曲线 C 及其围成的区域 D 内解析（可多联通），则有：
$$
\oint_Cf(z)dz=0
$$

#### 公式

$f(z)$ 在闭曲线 C 及其围成的区域 D 内解析，则对 D 内任一点 a 有：
$$
f^{(n)}(a)=\frac{n!}{2\pi \text{i}}\int_C\frac{f(z)}{(z-a)^{n+1}}dz
$$

> 后面还有更高级简便的方法。

## 级数表示

### 幂级数

#### 收敛判定

##### Abel定理

令 $I=\sum\limits_{n=0}^{+\infty}a_n(z-a)^n$ 为幂级数，则有：

- 若 $I$ 在 $z_0$ 收敛，则 $I$ 在圆 $|z-a|\leq|z_0-a|$ 内绝对收敛
- 若 $I$ 在 $z_0$ 发散，则 $I$ 在圆外域 $|z-a|>|z_0-a|$ 内处处发散

##### 收敛半径

等于实幂级数 $\sum\limits_{n=0}^{+\infty}|a_n|x^n$ 的收敛半径 R，且 $R=\frac{1}{r}$
$$
r=\lim\limits_{n\rightarrow+\infty}\Big|\frac{a_{n+1}}{a_n}\Big|\\
r=\lim\limits_{n\rightarrow+\infty}\sqrt[n]{|a_n|}\\
$$

在收敛圆内解析，可以逐项求任意阶导数（任意阶导数依然解析），逐项积分

#### 泰勒展开

##### 定义式

设 $f(z)$ 在 a 点解析，则在以 a 为中心的最大解析圆域（半径为收敛半径）内，有如下泰勒展开：
$$
f(z)=\sum\limits_{n=0}^{+\infty}a_n(z-a)^n\\
a_n=\frac{f^{(n)}(a)}{n!}
$$

展开式唯一

##### 常用公式

$$
\begin{align}
e^z&=\sum\limits_{n=0}^{+\infty}\frac{z^n}{n!}=1+z+\frac{z^2}{2!}+\frac{z^3}{3!}+\cdots\\
\cos{z}&=\sum\limits_{n=0}^{+\infty}(-1)^n\frac{z^{2n}}{(2n)!}=1-\frac{z^2}{2!}+\frac{z^4}{4!}-\cdots\\
\sin{z}&=\sum\limits_{n=0}^{+\infty}(-1)^n\frac{z^{2n+1}}{(2n+1)!}=z-\frac{z^3}{3!}+\frac{z^5}{5!}-\cdots\\
\frac{1}{1-z}&=\sum\limits_{n=0}^{+\infty}z^n=1+z+z^2+\cdots,\quad \bold{|z|<1}
\end{align}
$$

> 逐项积分，逐项求导，化为常用公式的形式

> 注意把系数单独拎出来，防止化为标准形式时犯错

#### 洛朗展开

##### 定义式

设 $f(z)$ 在圆环域 D：$r<|z-a|<R$ 内解析，则对 $\forall z\in D$，有：
$$
f(z)=\sum\limits_{n=-\infty}^{+\infty}a_n(z-a)^n\\
a_n=\frac{1}{2\pi\text{i}}\int_C\frac{f(z)}{(z-a)^{n+1}}dz
$$
其中 C 为 D 内任一条围绕点 a 的逆时针方向的闭路，展开式唯一

##### 有理真分式分解

形如：
$$
\begin{align}
\frac{1}{(1+x)^2(1-x)}&=\frac{a}{(1+x)^2}+\frac{b}{1+x}+\frac{c}{1-x}\\
\frac{1}{(x^2+1)(x-1)}&=\frac{ax+b}{x^2+1}+\frac{c}{x-1}\\
\frac{1}{(x^2+1)^2(x-1)}&=\frac{ax+b}{(x^2+1)^2}+\frac{cx+d}{x^2+1}+\frac{e}{x-1}\\
\frac{1}{(x+1)^2(x^2+1)^2}&=\frac{ax+b}{(x^2+1)^2}+\frac{cx+d}{x^2+1}+\frac{e}{(x+1)^2}+\frac{f}{x+1}
\end{align}
$$
待定系数法求解

## 留数

#### 定义

考虑 $f(z)$ 在 $r<|z-a|<R$ 上的洛朗展开，当 n = -1 时，有：
$$
\int_Cf(z)dz=2\pi\text{i}a_{-1}
$$
将 $a_{-1}$ 称为 $f(z)$ 在 a 点的留数，记作 $Res[f(z),a]$

#### 留数定理

设 $f(z)$ 在闭路 C 上解析，在 C 内部除 n 个孤立奇点 $a_1,a_2\cdots a_n$ 外解析，则有：
$$
\int_Cf(z)dz=2\pi\text{i}\sum\limits_{k=1}^{n}Res[f(z),a_k]
$$

#### 留数的计算

##### 定义计算

$Res[f(z),a]=a_{-1}$（对所有奇点均适用）

##### 极点

设 a 是 $f(z)$ 的 m 级极点，则：
$$
Res[f(z),a]=\frac{1}{(m-1)!}\lim\limits_{z\rightarrow a}\frac{d^{m-1}\{(z-a)^mf(z)\}}{dz^{m-1}}\
$$

##### 一级极点

若 $f(z)=\frac{P(z)}{Q(z)}$，且 a 为 $f(z)$ 的一级极点，则有：
$$
Res[f(z),a]=\frac{P(a)}{\lim\limits_{z\rightarrow a}\frac{dQ(z)}{dz}}
$$

> 若积分曲线内有本性奇点，比如：$e^{\frac{1}{z}}$ 在 $z=0$ 时，可以尝试 $w=\frac{1}{z}$ 换元，别忘记加个负号

### 定积分的计算

#### 三角积分

$\int_0^{2\pi}R(\cos{\theta},\sin{\theta})d\theta$ 型：$z=e^{\text{i}\theta}$
$$
dz=\text{i}e^{\text{i}\theta}d\theta=izd\theta\\
\cos{\theta}=\frac{z+\frac{1}{z}}{2}\\
\sin{\theta}=\frac{z-\frac{1}{z}}{2\text{i}}
$$

> 积分范围不是 $[0,2\pi]$ 或 $[-\pi,\pi]$ 的情况，换元或利用偶函数性质扩展区间

例 1：
$$
\begin{align}
\int_0^{\pi}\frac{1-\cos{mx}}{5-4\cos{x}}dx&=\frac{1}{2}\int_{-\pi}^{\pi}\frac{1-\cos{mx}}{5-4\cos{x}}dx\\
&=\frac{1}{2}Re\Big(\int_{-\pi}^{\pi}\frac{1-e^{\text{i}mx}}{5-4\cos{x}}dx\Big)\\
&=\frac{1}{2}Re\Big(\int_{-\pi}^{\pi}\frac{1-z^{m}}{5-4(\frac{z+\frac{1}{z}}{2})}\frac{dz}{\text{i}z}\Big)
\end{align}
$$
例 2：
$$
|dz|=|r\text{i}e^{\text{i}\theta}d\theta|=rd\theta=\frac{rdz}{iz}
$$

#### 广义积分

##### 引理 1（plus)：

设 $f(z)$ 在域 D：$|z|>R_0,\quad0\leq \arg(z)\leq\alpha\ (0<\alpha\leq2\pi)$ 内连续，且存在极限：
$$
\lim\limits_{z\rightarrow\infty}zf(z)=A
$$
则对于 D 内的圆弧 $C_R:$ $|z|=R\quad(0\leq \arg(z)\leq\alpha)$ 有：
$$
\lim\limits_{R\rightarrow+\infty}\int_{C_R}f(z)dz=\text{i}A\alpha
$$
特别地，当 A=0 时，为引理 1

##### 推论 1：

当 $f(z)$ 为有理函数 $\frac{P(z)}{Q(z)}$ 且 $Q(z)$ 至少比 $P(z)$ 高两次，则有：
$$
\lim\limits_{R\rightarrow+\infty}\int_{C_R}\frac{P(z)}{Q(z)}dz=0
$$

##### 引理 2（小圆定理）：

如果当 r 充分小时， $f(z)$ 在圆弧 $C_r:$ $z=a+re^{\text{i}\theta},\quad \alpha\leq \theta\leq\beta$ 上连续，且：
$$
\lim\limits_{z\rightarrow a}(z-a)f(z)=k
$$
则有：
$$
\lim\limits_{r\rightarrow 0}\int_{C_r}f(z)dz=\text{i}(\beta-\alpha)k
$$
特别地，取 k= 0 时

##### 推论 2：

设 a 为 $f(z)$ 的 1 级极点，则有：
$$
\lim\limits_{r\rightarrow 0}\int_{C_r}f(z)dz=\text{i}(\beta-\alpha)Res[f(z),a]
$$

##### 引理 3（大圆引理）：

如果当 R 充分大时，$g(z)$ 在圆弧 $C_r:$ $|z|=R,\quad Im(z)>-a\ (a>0)$ 上连续，且：
$$
\lim\limits_{z\rightarrow\infty}g(z)=0
$$
则对任何正数 $\lambda$，都有：
$$
\lim\limits_{R\rightarrow+\infty}\int_{C_R}g(z)e^{\text{i}\lambda z}dz=0
$$

#### 类型 1

$R(x)=\frac{P(x)}{Q(x)}$ ，且 P 比 Q 至少高两次，则取上半圆和实轴围成的闭路，$R\rightarrow +\infty$ 时，由引理 1  和留数定理有：
$$
\int_{-\infty}^{+\infty}R(x)dx=2\pi\text{i}\sum\limits_{k=1}^{n}Res[R(z),a_k]
$$

#### 类型 2

$I=\int_{-\infty}^{+\infty}R(x)\cos{mx}dx$ 或 $I=\int_{-\infty}^{+\infty}R(x)\sin{mx}dx$

取辅助函数 $f(z)=R(z)e^{\text{i}mz}$，求 $f(z)$ 的无穷积分，取实部或虚部即可。

只要求 P 比 Q 至少高一次，同样取上半圆和实轴围成的闭路，由引理 3 和留数定理，依然有：
$$
\int_{-\infty}^{+\infty}f(z)dz=2\pi\text{i}\sum\limits_{k=1}^{n}Res[F(z),a_k]
$$

> 取合适的辅助函数和辅助闭路，要求添加的路径上的积分能估计或能计算。
>
> 常见辅助闭路：半圆，半圆环，长方形，三角形

> Q(x) 如果有零点，需要挖掉，用小圆引理

(Fresnel积分 $\int_0^{+\infty}\cos{x^2}dx=\int_0^{+\infty}\sin{x^2}dx=\frac{\sqrt{2\pi}}{4}$)

(正态积分？ $\int_0^{+\infty}e^{-ax^2}\cos{bx}dx=\frac{1}{2}\sqrt{\frac{\pi}{a}}e^{-\frac{b^2}{4a}}$ )

### 幅角原理

##### 定理 1：

设 a，b 分别是 $f(z)$ 的 m 级零点和 n 级极点，则 a，b 均为 $\frac{f^{'}(z)}{f(z)}$ 的 1 级极点，且有：
$$
Res[\frac{f^{'}(z)}{f(z)},a]=m\\
Res[\frac{f^{'}(z)}{f(z)},b]=-n
$$

##### 定理 2：

设 $f(z)$ 在闭路 C 上解析且不为 0，在 C 内部只有有限多个极点，则有：
$$
\frac{1}{2\pi\text{i}}\int_C\frac{f^{'}(z)}{f(z)}dz=N-P
$$
 其中 N 表示在 C 内部 $f(z)$ 的零点总数，P 表示极点总数

> 由定理 1，显然成立

##### 幅角原理：

在定理 2 的条件下，有：
$$
N-P=\frac{1}{2\pi}\Delta_C\arg{f(z)}
$$
其中 $\Delta_C\arg{f(z)}$ 表示 $f(z)$ 在 z 沿闭路 C 正向绕行一周前后的幅角变化  

##### 罗歇定理：

设 $f(z)$ 和 $\varphi(z)$ 在闭路 C 及其内部解析，且在 C 上有不等式：
$$
|f(z)|>|\varphi(z)|
$$
则在 C 的内部 $f(z)+\varphi(z)$ 和 $f(z)$ 的零点个数相等

### 无穷远点留数（补充内容

##### 定义：

设 $f(z)$ 在圆环域 $R<|z|<\infty$ 内解析，C 为圆环域内绕原点的任何一条简单曲线，有
$$
Res[f(z),\infty]=\frac{1}{2\pi\text{i}}\int_{C^-}f(z)dz
$$
称为 $f(z)$ 在无穷远点的留数。

易推得，
$$
Res[f(z),\infty]=-\frac{1}{2\pi\text{i}}\int_{C}f(z)dz=-a_{-1}
$$

> 当 $\infty$ 为可去奇点时，$Res[f(z),\infty]$ 不一定为 0，例如：$\frac{1}{1-z}$

##### 定理：

若 $f(z)$ 在闭复平面上只有有限个孤立奇点，则 $f(z)$ 在所有奇点（包括 $\infty$ ）的留数和必等于 0

## 拉氏变换

### 基本运算

##### 线性关系

##### 相似定理：

$$
L[f(t)]=F(p)\Rightarrow L[f(at)]=\frac{1}{a}F\big(\frac{p}{a}\big)
$$

##### 位移定理：

$$
L[f(t)\cdot e^{\lambda t}]=F(p-\lambda)
$$

##### 象函数微分法：

$$
L[f(t)\cdot t^n]=(-1)^nF^{(n)}(p)
$$

##### 本函数微分法：

$$
L[f^{(n)}(t)]=p^nL[f(t)]-p^{n-1}f(0)-p^{n-2}f^{'}(0)-\cdots
$$

##### 本函数积分法：

$$
L\Big[\int_{0}^{t}f(t)dt\Big]=\frac{F(p)}{p}
$$

##### 延迟定理：

$$
L[f(t-a)]=e^{-pa}F(p)
$$

### 卷积

##### 定义

$$
f*g=\int_{-\infty}^{+\infty}f(x-t)g(t)dt
$$

##### 性质

交换律，结合律，分配律

##### 卷积定理

$$
L[f*g]=F(p)G(p)
$$

### 反演公式

##### 定理：

设 $F(p)$ 除在左半平面 $Re(p)<a$ 内有奇点 $p_1,p_2,\cdots p_n$ 外解析，且 $\lim\limits_{p\rightarrow\infty}F(p)=0$ ，则有：
$$
f(t)=\sum\limits_{k=1}^{n}Res[F(p)e^{pt},p_k]
$$

> 一般都是直接通过常见的拉氏变换形式直接反推

### 常用公式

$$
\begin{align}
L[1]&=\frac{1}{p}\\
L[t^a]&=\frac{\Gamma(a+1)}{p^{a+1}}\quad(a>-1)\quad \Gamma(n)=(n-1)!\\
L[e^{\lambda t}]&=\frac{1}{p-\lambda}\\
L[\sin{wt}]&=\frac{w}{p^2+w^2}\\
L[\cos{wt}]&=\frac{p}{p^2+w^2}\\
L[\sinh{wt}]&=\frac{w}{p^2-w^2}\\
L[\cosh{wt}]&=\frac{p}{p^2-w^2}\\
\end{align}
$$

## 完结撒花
