# 概率统计A 知识总结

2022-09-02

（搬运自 [作业部落](https://zybuluo.com/donghanyuan0609/note/1654136) ，不知道为啥到博客园上公式渲染全乱了）

## 前五章 概率论部分

### 概率

事件的交并差（跟集合运算差不多），条件概率 $P\left( AB \right) =P\left( A \right) P\left( B\mid A \right) $ ，相互独立 $P(AB)=P(A)P(B)$ 。

<u>"n次抽取，放回与不放回"</u>问题：不论放回与否，第 n 次抽中红球的概率都和第一次一样。（用全概率来推）

例：r 个红球 b 个黑球，每次抽一个，然后补充 c 个与抽中颜色相同的球，问第 n 次抽中红球的概率
（答案是 $\frac{r}{r+b}$ ，与 n 和 c 无关，可以用归纳法，由全概率公式去递推）

看一下全概率公式，还有贝叶斯公式。（全概率比较好理解，贝叶斯最好看一下免得突然考）

### 随机变量

离散型随机变量，连续型随机变量。

牢记一下分布函数的概念： $F_X(x)=P\{X\le x\}$ 。有时候求分布函数要从概率意义出发用定义来求。
（2020考研题，双路独立 $X_1,X_2\sim N(0,1)$， MUX($X_3$，服从二点分布)选出一路， $Y=X_3X_1+\left( 1-X_3 \right) X_2$ ，求 $(X_1,Y)$ 分布函数以及证明 $Y\sim N(0,1)$ ）

分布函数的性质：单调不减，$F(-\infty)=0$，$F(+\infty)=1$.

分布律(分布列)，概率密度函数。

二维联合分布函数：$F_{XY}(x,y)=P\{X\le x, Y\le y\}$ 。
联合分布(函数)，边沿分布(函数)；联合密度函数，边沿密度函数；条件分布函数/条件分布律/条件概率密度。

> 联合分布函数：$F_{XY}(x,y)$或$F(x,y)$；边沿分布函数：$F_X(x)$、$F_Y(y)$；联合密度函数$f_{XY}(x,y)$或$f(x,y)$；边沿密度函数：$f_X(x)$、$f_Y(y)$；
>
> 边沿与联合的关系：$F_X(x)=F(x,+\infty)$, $F_Y(y)=F(+\infty,y)$；$f_X\left( x \right) =\int_{-\infty}^{+\infty}{f\left( x,y \right) \text{d}y}$，$f_Y\left( y \right) =\int_{-\infty}^{+\infty}{f\left( x,y \right) \text{d}x}$ .
>
> 条件分布律: $P\{Y=j\mid X=i\}=\frac{P\left\{ X=i,Y=j \right\}}{P\left\{ X=i \right\}}$ ； $P\{X=i\mid Y=j\}=\frac{P\left\{ X=i,Y=j \right\}}{P\left\{ Y=j \right\}}$ ；
> 条件分布函数： $F_{X\mid Y}\left( x\mid y \right) $ ;  $F_{Y\mid X}\left( y\mid x \right) $ ;（注意这是个极限，书P84）
> 条件概率密度：条件$Y=y$下$X$的 $f_{X\mid Y}\left( x\mid y \right) =\frac{f\left( x,y \right)}{f_Y\left( y \right)}$，需要$f_Y(y)\ne 0$;反过来$f_{Y\mid X}\left( y\mid x \right) =\frac{f\left( x,y \right)}{f_X\left( x \right)}$ ;
>
> 符号$X\mid Y$表示：在$Y=y$的条件下$X$的条件(分布律/分布函数/概率密度)。注意"在$Y=y$条件下"暗含$P\{Y=y\}\ne 0$或$f_Y(y)\ne 0$的限制。

相互独立：$F_X(x)F_Y(y)=F_{XY}(x,y)$，$f_X(x)f_Y(y)=f_{XY}(x,y)$。（密度函数乘积只需"几乎处处相等"）

多个随机变量的函数的独立：若$X_1,X_2,X_3,X_4$相互独立，则$Y_1=f(X_1,X_2)$，$Y_2=g(X_3,X_4)$相互独立（两个式子不共用相同的随机变量，就可以相互独立）
若$X_1\sim N(0,1)$，$X_2\sim N(0,1)$，且$X_1, X_2$相互独立，则$X_1+X_2$与$X_1-X_2$相互独立

**标准化随机变量**：$X^{\ast}=\frac{X-EX}{\sqrt{DX}}$，满足$EX=0,DX=1$。

### 正态分布

一维概率密度函数：$N(\mu,\sigma^2)$
$$
f\left( x \right) =\frac{1}{\sigma \sqrt{2\pi}}\exp \left[ -\frac{\left( x-\mu \right) ^2}{2\sigma ^2} \right]
$$
标准正态分布$N(0,1)$密度函数：$\varphi \left( x \right) =\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$，分布函数$\varPhi \left( x \right) =\frac{1}{\sqrt{2\pi}}\int_{-\infty}^x{e^{-\frac{t^2}{2}}\text{d}t}$ 
（$\varPhi(x)$表示标准正态分布的分布函数，可以直接写）

标准正态分布分位点：**下侧**分位点 $z_{\alpha}=\varPhi^{-1}(\alpha)$ ， $P\left\{ X\le z_{\alpha} \right\} =\alpha $ ；上侧分位点 $z_{1-\alpha}=-z_{\alpha}$ ， $P\left\{ X>z_{1-\alpha} \right\} =\alpha $ ；**双侧**分位点 $z_{1-\frac{\alpha}{2}}$， $P\left\{ \left| X \right|>z_{1-\frac{\alpha}{2}} \right\} =\alpha ,P\left\{ \left| X \right|\le z_{1-\frac{\alpha}{2}} \right\} =1-\alpha $ 。

欧拉泊松积分： $\int_{-\infty}^{+\infty}{e^{-x^2}\text{d}x}=\sqrt{\pi}$ 

数字特征：$EX=\mu$，$DX=\sigma^2$，$EX^2=\mu^2+\sigma^2$ 。
中心2k阶矩（$X\sim N(\mu,\sigma^2)$）：$E\left( (X-\mu)^{2k} \right) =\sigma ^{2k}\left( 2k-1 \right) !!$，例如$E((X-\mu)^6)=15\sigma^6$ 。

正态分布的绝对值：对于$Y=\left|X-\mu \right|$ 且 $X\sim N(\mu,\sigma^2)$，其概率密度函数$f\left( y \right) =\begin{cases}
	\frac{\sqrt{2}}{\sigma \sqrt{\pi}}\exp \left( -\frac{y^2}{2\sigma ^2} \right)&		x\ge 0\\
	0&		x<0\\
\end{cases}$ ，
（原本的密度函数只保留一边且翻倍），其数字特征可以记住：$EY=\sqrt{\frac{2}{\pi}}\sigma $，$DY=\left( 1-\frac{2}{\pi} \right) \sigma ^2$ .
中心2k+1阶绝对矩（$X\sim N(\mu,\sigma^2)$）：$E\left| X-\mu \right|^{2k+1}=\sqrt{\frac{2}{\pi}}\sigma ^{2k+1}\cdot \left( 2k \right) !!$。


二维正态分布：$(X,Y)\sim N(\mu_1,\sigma_1^2;\mu_2,\sigma_2^2;\rho)$。其中$\rho$为相关系数。
$$
f\left( x,y \right) =\frac{1}{2\pi \sigma _1\sigma _2\sqrt{1-\rho ^2}}\cdot \exp \left\{ -\frac{1}{2\left( 1-\rho ^2 \right)}\left[ \left( \frac{x-\mu _1}{\sigma _1} \right) ^2-2\rho \frac{x-\mu _1}{\sigma _1}\frac{y-\mu _2}{\sigma _2}+\left( \frac{y-\mu _2}{\sigma _2} \right) ^2 \right] \right\}
$$
密度函数不好背可以记其矩阵形式：（n维正态分布）
$$
f\left( \boldsymbol{x} \right) =\frac{1}{\left( 2\pi \right) ^{\frac{n}{2}}\sqrt{\left| \boldsymbol{C} \right|}}\exp \left[ -\frac{1}{2}\left( \boldsymbol{x}-\boldsymbol{\mu } \right) ^T\boldsymbol{C}^{-1}\left( \boldsymbol{x}-\boldsymbol{\mu } \right) \right]
$$
式子中，$\boldsymbol{x}$为随机列向量，$\boldsymbol{\mu}$为期望列向量，$\boldsymbol{C}$为协方差矩阵。

（对于二维）其中 $\boldsymbol{x}=\left( \begin{array}{c}
	x_1\\
	x_2\\
\end{array} \right) $，$\boldsymbol{\mu }=\left( \begin{array}{c}
	\mu _1\\
	\mu _2\\
\end{array} \right) $，$\boldsymbol{C}=\left( \begin{matrix}
	\sigma _{1}^{2}&		\rho \sigma _1\sigma _2\\
	\rho \sigma _1\sigma _2&		\sigma _{2}^{2}\\
\end{matrix} \right) $，$\boldsymbol{C}^{-1}=\frac{1}{\left| \boldsymbol{C} \right|}\left( \begin{matrix}
	\sigma _{2}^{2}&		-\rho \sigma _1\sigma _2\\
	-\rho \sigma _1\sigma _2&		\sigma _{1}^{2}\\
\end{matrix} \right) $，$\left| \boldsymbol{C} \right|=\sigma _{1}^{2}\sigma _{2}^{2}\left( 1-\rho ^2 \right) $ 。

正态分布的性质：

1. 线性性质，正态分布的线性组合仍然是正态分布，期望和方差分别按照各自的性质。
2. 在正态分布中"独立"等价于"不相关"，即$\rho=0$。

### 几种常见的随机变量分布

##### 离散型随机变量

- 两点分布（0-1分布）
- 二项分布（伯努利分布）

$X\sim B(n,p)$ ，$P\left\{ X=k \right\} =\text{C}_{n}^{k}p^kq^{n-k}$，$EX=np$，$DX=npq$。其中$q=1-p$，$k=0,1,\cdots,n$。
要会计算$E(e^{tX})$($e^{tk}$和$p^k$合并，二项式定理)，参考结果：$(pe^t+q)^n$ （18-19,16-17往年题里均有）

求和性质：$B(n_1,p)+B(n_2,p)\sim B(n_1+n_2,p)$。

- 泊松分布

$X\sim \Pi(\lambda)$，$P\left\{ X=k \right\} =e^{-\lambda}\frac{\lambda ^k}{k!}$，$EX=DX=\lambda$。其中$k=0,1,\cdots$。
与二项分布关系：当n很大p很小时，$\lambda = np$用于逼近二项分布。

求和性质：$\Pi(\lambda_1)+\Pi(\lambda_2)\sim \Pi(\lambda_1+\lambda_2)$。

- 超几何分布

正品$M$件，次品$N$件，从中取$n$件，正品数量$k$件：$P\left\{ X=k \right\} =\frac{\text{C}_{N}^{k}\text{C}_{M}^{n-k}}{\text{C}_{M+N}^{n}}$。其中$k=0,1,\cdots,\min \{M,n\}$ .
（公式不要记，用古典概型-事件空间角度理解，自己推）
注意看清N是总数还是次品！（黑球白球问题同理）

数学期望：$N$为次品数则$EX=n\frac{M}{M+N}$，$N$为总数则$EX=n\frac{M}{N}$。

##### 连续型随机变量

- 正态分布
- 均匀分布
- 指数分布

概率密度函数：$f\left( x \right) =\begin{cases}
	\lambda e^{-\lambda x}&		x\ge 0\\
	0&		x<0\\
\end{cases}$，分布函数$F\left( x \right) =\begin{cases}
	0&		x<0\\
	1-e^{-\lambda x}&		x\ge 0\\
\end{cases}$ 。

数字特征：$EX=\frac{1}{\lambda}$，$DX=\frac{1}{\lambda^2}$ 。

### 随机变量函数的分布

离散型没什么好说的，列出分布列然后合并同类项就可以。

连续型主要是找对积分区间，对自变量的概率密度求积分，得到因变量的分布函数。先有分布函数再求导就可以得到概率密度函数。
即：$F_Y\left( y \right) =\int_{Y\le y}{f\left( x \right) \text{d}x}$，$F_Z\left( z \right) =\iint_{Z\le z}{f\left( x,y \right) \text{d}x\text{d}y}$。

P105定理：<u>一维</u>随机变量$X$，密度函数$f(x)$，$Y=g(X)$，$g$为**单调**函数，则
$$
f_Y\left( y \right) =\begin{cases}
	f\left( x \right) \cdot \left| h'\left( y \right) \right|&		y\in I\\
	0&		Other\\
\end{cases}
$$
其中$h$为$g$的反函数，有$X=h(Y)$。
应用上述定理时$g$必须是单调函数。如果是分段单调也可分割区间分别处理。

绝对值$Y=\left| X\right|$，对于正态分布，其概率密度函数变两倍，只取右边。

几个二维随机变量函数的分布（P111-114）

- $Z=X+Y$ （主要还是理解过程，记公式意义不大）

$P\{Z\le z\}=P\{X+Y\le z\}$，积分区域为直线$y= z-x$以下的区域。如果是无穷区域，用$y=z-x$换元后Fubini换序积分，得$f_Z\left( z \right) =\int_{-\infty}^{+\infty}{f\left( x,z-x \right) \text{d}x}$ ，此积分可转化为曲线积分$f_Z\left( z \right) =\int_{\overline{AB}}{f\left( x,y \right) \text{d}x}$ 计算，其中直线$\overline{AB}:x+y=z$ 沿$x$增加的方向。同理$f_Z\left( z \right) =\int_{-\infty}^{+\infty}{f\left( z-y,y \right) \text{d}y} =\int_{\overline{BA}}{f\left( x,y \right) \text{d}y}$ 
对于有限区域直接(分段)算二重积分或者曲线积分就行，不用背公式。

- $Z=\max \{X,Y\}$ 

$P\{Z\le z\}=P\{X\le z, Y\le z\}$ ，
分布函数$F_{\max}(z)=F_{XY}(z,z)$，对于n个独立同分布变量，$F_{\max}(z)=[F(z)]^n$。

- $Z=\min \{X,Y\}$

$P\{Z\le z\}=P\{X\le z \cup Y \le z\}$，
分布函数$F_{\min}(z)=F_X(z)+F_Y(z)-F_{XY}(z,z)$，对于n个独立同分布变量，$F_{\min}(z)=1-[1-F(z)]^n$ 。

### 随机变量的数字特征

- 期望和方差：单个随机变量
- 协方差和相关系数，协方差矩阵：两个或多个随机变量的关系

随机变量的期望：连续型$E\left(X\right)=\int_{-\infty}^{+\infty}{x f_X\left( x \right) \text{d}x}$，离散型$E\left(X\right)=\sum_k k\cdot P \left\{ X=k \right\}$ .

随机变量函数的期望：连续型 $E\left[g(X)\right]=\int_{-\infty}^{+\infty}{g\left( x \right) f_X\left( x \right) \text{d}x}$，离散型 $E\left[g(X)\right]=\sum_k g(k)P \left\{ X=k \right\} $ 
（直接在期望公式中把X替换即可，概率/概率密度项不动。）

> 离散型随机变量函数的期望，计算时可能涉及级数（几何级数，泰勒级数，逐项积分/求导），二项式定理。
>
> 例：已知几何分布$P\left\{X=k\right\}=pq^{k-1}, k=1,2,\cdots; q=1-p $，求$E(e^{\text{i}tX})$。（几何级数）
>
> 解：$E(e^{itX})=\sum_{k=1}^{\infty} e^{\text{i}tk}\cdot p\cdot q^{k-1}=e^{\text{i}t}\cdot p\cdot \sum_{k=1}^{\infty}e^{\text{i}t(k-1)}q^{k-1}=pe^{\text{i}t}\cdot \sum_{k=0}^{\infty}(qe^{\text{i}t})^k=pe^{\text{i}t} \frac{1}{1-qe^{\text{i}t}}$ .
> 其中最后一步依据为几何级数$\frac{1}{1-x}=\sum_{k=0}^{\infty}x^k$，收敛半径为1，而$\left|qe^{\text{i}t}\right|=q<1$ 在收敛域内。
>
> 例：已知二项分布$X\sim B(n,p)$，求$E(e^{tX})$。（二项式定理）
>
> 解：$E\left( e^{tX} \right) =\sum_{k=0}^n{C_{n}^{k}p^kq^{1-k}\cdot e^{tk}}=\sum_{k=0}^n{C_{n}^{k}\left( pe^t \right) ^kq^{1-k}}=\left( pe^t+q \right) ^n$ 。
>
> 例：已知泊松分布$X\sim \Pi(\lambda)$，证明期望$EX=\lambda$，且判断$E\left( \frac{1}{X+1} \right) $与$\frac{1}{\lambda +1}$的关系（$e^x$的泰勒级数）
>
> 解：$EX=e^{-\lambda}\sum_{k=0}^{\infty}{\frac{\lambda ^k}{k!}\cdot k}=e^{-\lambda}\sum_{k=1}^{\infty}{\frac{\lambda ^k}{\left( k-1 \right) !}}=\lambda \cdot e^{-\lambda}\sum_{k=1}^{\infty}{\frac{\lambda ^{k-1}}{\left( k-1 \right) !}}=\lambda e^{-\lambda}\sum_{k=0}^{\infty}{\frac{\lambda ^k}{k!}}$ 
> $=\lambda e^{-\lambda}\cdot e^{\lambda}=\lambda $，其中最后一步由指数函数$e^x$的泰勒级数$e^x=\sum_{n=0}^{\infty}{\frac{x^n}{n!}}$得到。
> 而$E\left( \frac{1}{X+1} \right) =e^{-\lambda}\sum_{k=0}^{\infty}{\frac{\lambda ^k}{k!}\cdot \frac{1}{k+1}}=e^{-\lambda}\sum_{k=0}^{\infty}{\frac{\lambda ^k}{\left( k+1 \right) !}}=\frac{e^{-\lambda}}{\lambda}\sum_{k=1}^{\infty}{\frac{\lambda ^k}{k!}}=\frac{e^{-\lambda}}{\lambda}\cdot \left( e^{\lambda}-1 \right)=\frac{1}{\lambda}-\frac{e^{-\lambda}}{\lambda}$ ， 
> 下面比较$\frac{1}{\lambda}-\frac{e^{-\lambda}}{\lambda}$和$\frac{1}{\lambda +1}$的大小关系：$\frac{1}{\lambda}-\frac{1}{\lambda +1}=\frac{1}{\lambda \left( \lambda +1 \right)}$，$\frac{e^{-\lambda}}{\lambda}=\frac{1}{\lambda e^{\lambda}}$，由于$\lambda>0$，$e^\lambda>\lambda+1$ ，所以$\frac{1}{\lambda \left( \lambda +1 \right)}>\frac{1}{\lambda e^{\lambda}}$，$\therefore \frac{1}{\lambda}-\frac{1}{\lambda +1}>\frac{e^{-\lambda}}{\lambda}$，$\therefore \frac{1}{\lambda}-\frac{e^{-\lambda}}{\lambda}>\frac{1}{\lambda +1}$，即$E\left( \frac{1}{X+1} \right) >\frac{1}{\lambda +1}$ 。（18-19上）

方差的计算公式：$DX=E(X-EX)^2=EX^2-(EX)^2$

**期望的线性性质：**
数乘 $E(aX)=aEX$，
相加 $E(X+Y)=EX+EY$（**<u>不需要</u>**相互独立），
线性组合 $E(aX+bY+c)=aEX+bEY+c$（不用独立）

**乘积的期望**： $E(XY)=EX\cdot EY$（要求**<u>相互独立</u>**）

**方差性质：**（并不保线性）
系数平方 $D(aX)=a^2DX$，
平移不变 $D(X+C)=DX$，
相加 $D(X\pm Y)=DX+DY$（此时要求$X,Y$**<u>相互独立</u>**）
线性组合 $D(aX+bY+c)=a^2DX+b^2DY$ （相互独立）

##### 矩的概念

原点矩，中心矩；k阶矩；绝对矩；混合矩；

##### 协方差

协方差定义：$\text{Cov}\left( X,Y \right) =E\left[ \left( X-EX \right) \cdot \left( Y-EY \right) \right] $

计算公式：
用期望：$\text{Cov}\left( X,Y \right) =E\left( XY \right) -EX\cdot EY$，
用方差：$D\left( X\pm Y \right) =DX+DY\pm \text{2Cov}\left( X,Y \right) $，（别忘了公式里有个2倍）

性质：
对称 $\text{Cov}\left( X,Y \right) =\text{Cov}\left( Y,X \right) $，
倍乘 $\text{Cov}\left( aX,bY \right) =ab\text{Cov}\left( X,Y \right) $，
相加 $\text{Cov}\left( X_1+X_2,Y \right) =\text{Cov}\left( X_1,Y \right) +\text{Cov}\left( X_2,Y \right) $ ，
方差 $\text{Cov}\left( X,X \right)=DX$

##### 相关系数

定义：$\rho =\frac{\text{Cov}\left( X,Y \right)}{\sqrt{DX}\cdot \sqrt{DY}}$ 。

表示两随机变量的线性相关程度，$\left| \rho \right|\le 1$恒成立。$\rho$是无量纲数。
$\rho=1$:正线性；$\rho=-1$:负线性；$\rho>0$:正相关；$\rho<0$:负相关；$\rho=0$:不相关。
注意：不相关不一定相互独立。反例：<u>非矩形</u>形状的<u>均匀分布</u>，不满足$f_{XY}(x,y)=f_X(x)f_Y(y)$。
对于**正态分布**：$\rho=0$ 不相关即为相互独立。

代入标准化随机变量有：$\rho_{XY} =\text{Cov}\left( X^{\ast},Y^{\ast} \right) $ 。

##### 协方差矩阵

二维随机向量：$\boldsymbol{C}=\left( \begin{matrix}
	\sigma _{1}^{2}&		\rho \sigma _1\sigma _2\\
	\rho \sigma _1\sigma _2&		\sigma _{2}^{2}\\
\end{matrix} \right) $，n维：$c_{ij}=\text{Cov}\left( X_i,X_j \right) $。

### 几个不等式

- $\left| E\left( XY \right) \right|\le \left( EX^2 \right) ^{\frac{1}{2}}\cdot \left( EY^2 \right) ^{\frac{1}{2}}$ ，（柯西-施瓦茨）
- $\left( E\left| X+Y \right|^2 \right) ^{\frac{1}{2}}\le \left( EX^2 \right) ^{\frac{1}{2}}+\left( EY^2 \right) ^{\frac{1}{2}}$ ，
- $\left| \text{Cov}\left( X,Y \right) \right|\le \sqrt{DX}\cdot \sqrt{DY}$ ，
- $\left| P\left( AB \right) -P\left( A \right) P\left( B \right) \right|\le \left\{ P\left( A \right) \left[ 1-P\left( A \right) \right] \right\} ^{\frac{1}{2}}\left\{ P\left( B \right) \left[ 1-P\left( B \right) \right] \right\} ^{\frac{1}{2}}$ ，

## 六七八九章 数理统计部分

### 第六章

#### 切比雪夫不等式

$$
P\left\{ \left| X-EX \right|\ge \varepsilon \right\} \le \frac{DX}{\varepsilon ^2}
$$

导出形式：$P\left\{ \left| X-EX \right|<\varepsilon \right\} \ge 1-\frac{DX}{\varepsilon ^2}$ 。
意义：用二阶偏差"控制"一阶偏差。

#### 大数定律

##### 切比雪夫大数定律

一列相互独立的随机序列$X_1,X_2,\cdots ,X_n,\cdots $，令$Y_n=\frac{1}{n}\sum_{i=1}^n{X_i}=\bar{X}_n$。
由方差一致有界（$D\left( X_i \right) \le C$）得：$\underset{n\to\infty}{\lim} DY_n=0$，或$\forall \varepsilon >\text{0, }\underset{n\rightarrow \infty}{\lim}P\left\{ \left| Y_n-EY_n \right|<\varepsilon \right\} =1$ 。

注：方差一致有界的条件可以放松，只要$C$是$n$的低阶无穷大即可（$\underset{n\rightarrow \infty}{\lim}\frac{C}{n}=0$）。

**依概率收敛**：$\forall \varepsilon >\text{0, }\underset{n\rightarrow \infty}{\lim}P\left\{ \left| X_n-X \right|<\varepsilon \right\} =1$，记为$X_n\xrightarrow{P}X$。

##### 辛钦大数定律

$\{X_n\}$独立同分布，$EX_i=\mu$，$DX_i=\sigma^2$，则$\underset{n\rightarrow \infty}{\lim}P\left\{ \left| \bar{X}-\mu \right|<\varepsilon \right\} =1$ （$\bar{X}\xrightarrow{P}\mu $）

##### 伯努利大数定律

伯努利n次独立重复试验，$n_A$表示事件A发生的次数，p为A发生的概率，则$\underset{n\rightarrow \infty}{\lim}P\left\{ \left| \frac{n_A}{n}-p \right|<\varepsilon \right\} =1$。

#### 中心极限定理

随机序列$\{X_n\}$独立同分布，$EX_i=\mu$，$DX_i=\sigma^2\ne 0$。
令$Y_n=\sum_{i=1}^n{X_i}$，当n充分大时，近似地有：$Y_{n}^{\ast}\sim N\left( \text{0,}1 \right) $，$Y_n\sim N\left( n\mu ,n\sigma ^2 \right) $ 。

#### 棣莫弗-拉普拉斯定理

伯努利n次独立重复试验，$\mu_n$表示事件A发生次数，p是事件A发生的概率，则$\forall[a,b]$，
$$
\underset{n\rightarrow \infty}{\lim}P\left\{ a<\frac{\mu _n-np}{\sqrt{np\left( 1-p \right)}}\le b \right\} =\varPhi \left( b \right) -\varPhi \left( a \right) 
$$
理解：由二项分布，$E\mu_n=np$，$D\mu_n=np(1-p)$；由中心极限定理，$\mu_n^{\ast}\sim N(0,1)$，即得上式。

一个便于记忆的形式：
$$
P\left\{ N^{\ast}<\mu _{n}^{\ast}\le M^{\ast} \right\} \approx \varPhi \left( M^{\ast} \right) -\varPhi \left( N^{\ast} \right) 
$$
（其中M和N均为整数，代表一个"事件发生次数"；$\mu _{n}^{\ast}=\frac{\mu _n-np}{\sqrt{np\left( 1-p \right)}}$，$N^{\ast}=\frac{N-np}{\sqrt{np\left( 1-p \right)}}$）
上面这个约等式也可以是单边概率，即$N\to-\infty$或$M\to+\infty $。

本部分题型主要参考课后题，一般就是用定理去估计一件事的概率，或者是给定概率求N之类的。

### 第七章 统计总体与样本

#### 样本统计量

样本均值：$\bar{X}=\frac{1}{n}\sum_{i=1}^n{X_i}$，
样本方差：$S^2=\frac{1}{n-1}\sum_{i=1}^n{\left( X_i-\bar{X} \right) ^2}$，（注意分母是$n-1$）！！！
（k阶原点、中心矩：$A_k=\frac{1}{n}\sum_{i=1}^n{X_{i}^{k}}$，$B_k=\frac{1}{n}\sum_{i=1}^n{\left( X_i-\bar{X} \right) ^k}$）注意$B_2 \ne S^2$。

样本统计量的数字特征：$E\bar{X}=\mu$，$D\bar{X}=\frac{\sigma^2}{n}$；$ES^2=\sigma^2$。（任何分布的总体都可以）

#### 样本统计量的分布（正态，卡方，t，F）

此处只针对正态总体。

##### 四种分布

- 正态分布，标准正态分布$N(0,1)$及其分位点（下侧：$z_\alpha$，双侧：$z_{1-\frac{\alpha}{2}}$）
- 卡方分布$\chi^2(n)$及其期望、方差、分位点$\chi_\alpha^2(n)$ 

n路标准正态分布（$X_i\sim N(0,1)$）之和服从自由度为n的卡方分布，$\sum_{i=1}^n{X_{i}^{2}}\sim \chi ^2\left( n \right)$。
若$X\sim \chi^2(n)$，则$EX=n$，$DX=2n$。$\chi^2(n_1)+\chi^2(n_2)\sim \chi^2(n_1+n_2)$。

标准正态的平方是卡方分布：$X\sim N(0,1)$则$X^2\sim\chi^2(1)$。

- t分布$t(n)$及其分位点（下侧：$t_\alpha(n)$，双侧：$t_{1-\frac{\alpha}{2}}$）

形式：$\frac{N\left( \text{0,}1 \right)}{\sqrt{\chi ^2\left( n \right) /n}}\sim t(n)$，期望和方差不用知道。

- F分布

形式：$\frac{\chi ^2\left( n_1 \right) /n_1}{\chi ^2\left( n_2 \right) /n_2}\sim F\left( n_1,n_2 \right) $，t分布的平方是F分布（$T\sim t(n)$，$T^2\sim F(1,n)$）

##### 统计量的分布

正态：$\bar{X}\sim N\left( \mu ,\frac{\sigma ^2}{n} \right) $，$X_j-\bar{X}\sim N(0, \frac{n-1}{n}\sigma^2)$ ，
标准正态：$\bar{X}^{\ast}=\frac{\bar{X}-\mu}{\sigma \sqrt{n}}\sim N\left( \text{0,}1 \right) $，$X_{i}^{\ast}=\frac{X_i-\mu}{\sigma}\sim N\left( \text{0,}1 \right) $，

卡方：$\frac{n-1}{\sigma ^2}S^2\sim \chi ^2\left( n-1 \right)$，$DS^2=\frac{2\sigma^4}{n-1}$。
	一对自由度差异：$\sum_{i=1}^n{\left( \frac{X_i-\bar{X}}{\sigma} \right) ^2\sim \chi ^2\left( n-1 \right)}$，$\sum_{i=1}^n{\left( \frac{X_i-\mu}{\sigma} \right) ^2\sim \chi ^2\left( n \right)}$。(用正态线性函数的结论)

t分布：$\frac{\bar{X}-\mu}{S/\sqrt{n}}\sim t\left( n-1 \right)$ ，
$\frac{\left( \bar{X}-\bar{Y} \right) -\left( \mu _1-\mu _2 \right)}{\sqrt{\left( m-1 \right) S_{1}^{2}+\left( n-1 \right) S_{2}^{2}}}\cdot \sqrt{\frac{mn\left( m+n-2 \right)}{m+n}}\sim t\left( m+n-2 \right) $ ，$\frac{\bar{X}-\bar{Y}-\left( \mu _1-\mu _2 \right)}{S_1\sqrt{\text{1/}n+\text{1/}m}}\sim t\left( m-1 \right) $，$\sqrt{\frac{n}{n+1}}\frac{X_{n+1}-\bar{X}}{S}\sim t\left( n-1 \right) $

F分布：$n\frac{\left( \bar{X}-\mu \right) ^2}{S^2}\sim F\left( \text{1,}n-1 \right)$

对于正态总体 $N(\mu, \sigma^2)$ ，设其样本均值为 $\bar{X}$ ，样本方差为 $S^2$ ，则有 $\bar{X}$ 与 $S^2$ 相互独立。

### 参数的点估计

#### 矩估计

一个参数：直接用EX。注意：$\hat{\theta^2}$如果能直接估计出来，不要先估计$\hat{\theta}$再平方，会有累积误差。

两个参数：$\left\{ \begin{array}{c}
	EX=\bar{X}\\
	DX=S^2\\
\end{array} \right. $，$\left\{ \begin{array}{c}
	EX=\bar{X}\\
	DX=B_2\\
\end{array} \right. $，或$\left\{ \begin{array}{c}
	EX=\bar{X}\\
	EX^2=A_2\\
\end{array} \right. $都可以（第一种最好，第二种不是无偏估计）

#### 极大似然估计

构造似然函数$L(\theta)$，找到使得$L(\theta)$取最大值的$\hat{\theta}$。一般情况下令$L'(\theta)=0$，也有特殊情况无法求导处理。

似然函数的构造：离散型 $L\left( \theta \right) =\prod_{i=1}^n{P\left\{ X_i=x_i \right\}}$，连续型 $L\left( \theta \right) =\prod_{i=1}^n{f\left( x_i \right)}$。
(把所有样本的概率/概率密度<u>全乘起来</u>)

可以求导的典型例子：指数分布；不能求导的典型例子：均匀分布

常考的可以求导的形式：$P\cdot e^Q$，其中P,Q均为含$x$与$\theta$的仅乘除的式子（单项式或者分式），例如指数分布。

不能用求导的例子P203：$f\left( x \right) =\begin{cases}
	\frac{1}{\theta}&		0\le x\le \theta\\
	0&		\text{Others}\\
\end{cases}$ ，$\hat{\theta}=\max \left\{ x_1,\cdots ,x_n \right\} $ 。

#### 点估计的优良性

- 无偏估计：$E(\hat{\theta})=\theta$。
- 最小方差无偏估计：$D(\hat{\theta})$最小
- 一致估计（相合估计）：$\hat{\theta}\xrightarrow{P}\theta $。（用大数定律）

一些结论：（设总体期望为$\mu$，总体方差为$\sigma$）

- $\bar{X}$是$\mu$的无偏估计，也是最小方差线性无偏估计
- $S^2$是$\sigma^2$的无偏估计，而$B_2$不是无偏的。
- 用样本的线性组合对总体无偏估计，系数越平均，方差越小
- 正态总体的$\bar{X}$是$\mu$的一致估计，$S^2$是$\sigma^2$的一致估计
- 正态总体的方差：$\hat{\sigma^2}=\frac{1}{n-1}\sum_{i=1}^n{\left( X_i-\bar{X} \right) ^2}$(矩估计)和$\hat{\sigma^2}=\frac{1}{n}\sum_{i=1}^n{\left( X_i-\mu _0 \right) ^2}$(极大似然)都是$\hat{\sigma^2}$的无偏估计，后者更优。

### 区间估计与假设检验

我把这两块写到一起了，因为他们的本质和用到的方法都是一样的。

区间估计里的"置信概率":$1-\alpha$；假设检验里的"检验水平(显著性水平)":$\alpha$；这两者里的$\alpha$是等价的。

假设检验要按步骤写，写原假设和备择假设；选取检验用的统计量；确定检验水平和拒绝域；代入样本值检验。

假设检验的两类错误：错误拒绝H0（第一类），错误接受H0（第二类）。第一类错误的概率要会算（和$\alpha$有关）

大体上分为三类：知(总体)方差检验期望，未知方差检验期望，检验方差。（选取的统计量和分布不同）
每种检验/估计分为双边、左边、右边。（选取双侧分位点或下侧/上侧分位点）

- 已知$\sigma^2$检验$\mu_0$：

选取的统计量：$U=\frac{\bar{X}-\mu _0}{\sigma /\sqrt{n}}$，服从的分布：$N(0,1)$。

双侧假设检验：$P\left\{ \left| U \right|>z_{1-\frac{\alpha}{2}} \right\} =\alpha $，双侧$\mu$置信区间：$\left[ \bar{x}-z_{1-\frac{\alpha}{2}}\frac{\sigma}{\sqrt{n}},\bar{x}+z_{1-\frac{\alpha}{2}}\frac{\sigma}{\sqrt{n}} \right]$。

左边检验（$H_1:\mu<\mu_0$）：$P\left\{ U<-z_{1-\alpha} \right\} =\alpha $；置信区间：$\left( -\infty ,\bar{x}+z_{1-\alpha}\frac{\sigma}{\sqrt{n}} \right] $.
右边检验（$H_1:\mu>\mu_0$）：$P\left\{ U>z_{1-\alpha} \right\} =\alpha$；置信区间：$\left[ \bar{x}-z_{1-\alpha}\frac{\sigma}{\sqrt{n}},+\infty \right) $.

- 未知$\sigma^2$检验$\mu_0$：

选取的统计量：$T=\frac{\bar{X}-\mu _0}{s/\sqrt{n}}$，服从的分布：$t(n-1)$。(注意自由度，是n-1)

双侧假设检验：$P\left\{ \left| T \right|>t_{1-\frac{\alpha}{2}} \right\} =\alpha $，双侧$\mu$置信区间：$\left[ \bar{x}-t_{1-\frac{\alpha}{2}}\frac{s}{\sqrt{n}},\bar{x}+t_{1-\frac{\alpha}{2}}\frac{s}{\sqrt{n}} \right]$。

- 检验$\sigma^2$：

选取的统计量：$W=\frac{\left( n-1 \right) s^2}{\sigma ^2}$，服从的分布：$\chi^2(n-1)$。(注意自由度，是n-1)

双侧假设检验：$P\left\{ W>\chi _{1-\frac{\alpha}{2}}^{2}\cup W<\chi _{\frac{\alpha}{2}}^{2} \right\} =\alpha $，双侧$\sigma^2 $置信区间：$\left[ \frac{\left( n-1 \right) s^2}{\chi _{1-\frac{\alpha}{2}}^{2}\left( n-1 \right)},\frac{\left( n-1 \right) s^2}{\chi _{\frac{\alpha}{2}}^{2}\left( n-1 \right)} \right]$，双侧$\sigma$置信区间：$\left[ \sqrt{\frac{\left( n-1 \right) s^2}{\chi _{1-\frac{\alpha}{2}}^{2}\left( n-1 \right)}},\sqrt{\frac{\left( n-1 \right) s^2}{\chi _{\frac{\alpha}{2}}^{2}\left( n-1 \right)}} \right]$。

卡方这里分位点选取并不唯一，也可以选$\chi _{1-\frac{2}{3}\alpha}^{2}, \chi _{\frac{\alpha}{3}}^{2}$，只要保证两个$\alpha$的系数(的绝对值)加起来等于一就可以。（或者两个系数相减等于$1-\alpha$，本质一样）

##### 此部分考试的注意要点

根据往年题经验，一般是考一个选择题，让判断一个随机变量(统计量)在某区间里的概率对不对。判断起来并不难，主要就是看选取的统计量和采用的分位点分布种类是否匹配。比如说如果统计量$T$搭配了正态分布分位点$z_{1-\frac{\alpha}{2}}$肯定是不对的。然后就是双侧分位点选取的灵活处理。题目有可能不是用的两边对称的$1-\frac{\alpha}{2}$双侧分位点，这时候可以简单画图验证一下。
（正态分布和t分布都是单峰对称的，$(-\infty,+\infty)$，而卡方分布是单峰不对称的，$(0,+\infty)$。）
先排除统计量与分布对应不上的，再看分位点选取是否正确。

## 最后三章 随机过程部分

### 随机过程

随机过程$X(t)$，相当于"一族"随机变量，与样本和时间有关（$X(e,t)$）。
**样本(函数)**：固定$e$保留时间$t$变化，即$x(t)=X(e_0,t)$；是个关于t的函数
**状态(变量)**：固定t保留样本$e$变化，即$X(t_1)=X(e,t_1)$；是个随机变量

状态空间：$\{X(e,t)\mid e\in S,t\in T\}$，即$X$的值域。样本空间$S=\{e\}$；参数集$T$：参数$t$取值范围。

随机序列：参数$t$离散的情况，即$\{X_n\}$。

n维分布函数；两个随机过程的有限维联合分布函数；要会写一维$F_1(x_1;t_1)$和二维$F_2(x_1,x_2;t_1,t_2)$ 。

独立过程（任何n个状态都独立）

##### 数字特征

单个过程
（对于二阶矩存在的过程，只要知道$\mu$和$R_X$就可以确定其他三个）

- 期望：$\mu_X(t)=E[X(t)]$；

- 二阶矩：$\Psi_X^2(t)=E[X^2(t)]$；$\Psi_X^2(t)=R_X(t,t)$；
- 方差：$\sigma_X^2(t)=D[X(t)]=\Psi_X^2(t)-\mu_X^2(t)$；$\sigma_X^2(t)=C_X(t,t)$；
- 自相关函数：$R_X(t_1,t_2)=E[X(t_1)\cdot X(t_2)]$；
- 自协方差：$C_X(t_1,t_2)=\text{Cov}(X(t_1),X(t_2))=R_X(t_1,t_2)-\mu_X(t_1)\cdot \mu_X(t_2)$；

两个过程

- 互相关函数：$R_{XY}(t_1,t_2)=E[X(t_1)\cdot Y(t_2)]$
- 互协方差：$C_{XY}(t_1,t_2)=\text{Cov}(X(t_1),Y(t_2))=R_{XY}(t_1,t_2)-\mu_X(t_1)\cdot \mu_Y(t_2)$；

两过程相互独立：$R_{XY}(t_1,t_2)=\mu_X(t_1)\cdot \mu_Y(t_2)$；
任意时刻$C_{XY}(t_1,t_2)=0$则$X(t)$与$Y(t)$不相关。

### 平稳过程

#### 严平稳

条件：
(1)一维分布函数$F(x_1;t)=F(x_1)$不依赖于参数t；
(2)二维分布函数$F(x_1,x_2;t_1,t_2)=F(x_1,x_2;\tau)$仅依赖于参数间距$\tau=t_2-t_1$；

性质：
(1)$\mu_X$，$\Psi_X^2$，$\sigma_X^2$与时间t无关；
(2)$R_X(\tau)$，$C_X(\tau)$只与间距$\tau=t_2-t_1$有关，与$t_1$的原点选取无关。

（$R_X(\tau)=E[X(t)X(t+\tau)]$）

严平稳过程举例：<u>伯努利序列</u>。

#### 广义平稳

条件：
(1)原点二阶矩$E[X^2(t)]$存在
(2)期望$E[X(t)]=\mu_X$为常数
(3)自相关函数$E[X(t)X(t+\tau)]=R_X(\tau)$只与$\tau$有关，与$t$无关

性质：前三个数字特征都是常数；后两个数字特征只与间距有关。
注 $\sigma_X^2=C_X(0)$，$\Psi_X^2=R_X(0)$; $R_X(\tau)$与$C_X(\tau)$均为偶函数.

平稳(时间)序列：参数$t$为整数或者可列；

两个平稳的关系：
严平稳+二阶矩存在=广义平稳；广义平稳不一定严平稳；严平稳不一定广义平稳(二阶矩不一定存在)

两个平稳过程之间的关系：
(1)联合平稳过程：$E[X(t)Y(t+\tau)]=R_{XY}(\tau)$；此时$C_{XY}(\tau)=R_{XY}(\tau)-\mu_X\mu_Y$。
(2)标准互协方差：$\rho _{XY}\left( \tau \right) =\frac{C_{XY}\left( \tau \right)}{\sqrt{C_X\left( 0 \right) \cdot C_Y\left( 0 \right)}}$，等于零时则两个平稳过程不相关。

注：检验(广义)平稳过程，一定要说明二阶矩$E[Y^2(t)]$存在且有限（直接代$R_X(0)$说明即可，但不能不写）！

#### 正态(平稳)过程

正态过程：$\forall t$都有$X(t)$服从正态分布；
正态序列；独立正态过程；

正态平稳过程：正态过程$X(t)$为(广义)平稳过程。

记个结论：正态过程$X(t)$其中$t\in (-\infty,+\infty)$，满足$E[X(t)]=\mu_X=0$，则：

1. 一维分布：$X(t)\sim N(0, R_X(0))$；
2. 二维分布：$(X(t_1),X(t_2))\sim N\left(0, R_X(0);0,R_X(0);\frac{R_X(\tau)}{R_X(0)}\right)$.

#### 遍历过程

##### 时间均值和时间相关函数的概念

- 时间均值：$\overline{X\left( t \right) }=\overline{X\left( e,t \right) }=\underset{l\rightarrow +\infty}{\lim}\frac{1}{2l}\int_{-l}^l{X\left( e,t \right) \text{d}t}$，其中$\overline{X\left( t \right) }$是个随机变量。
- 时间相关函数：$\overline{X\left( t \right) X\left( t+\tau \right) }=\underset{l\rightarrow +\infty}{\lim}\frac{1}{2l}\int_{-l}^l{X\left( e,t \right) X\left( e,t+\tau \right) \text{d}t}$，是一个关于$\tau$的随机过程。

对于单边的情况（$t>0$，$\left\{ X\left( t \right) ,t\in \left[ \text{0,}+\infty \right) \right\} $），将$\frac{1}{2l}\int_{-l}^l$换成$\frac{1}{l}\int_{0}^l$即可。

##### 平稳过程的各态遍历性

- 均值的各态遍历性：$P\left\{ \overline{X\left( t \right) }=\mu _X \right\} =1$；
- 自相关函数的各态遍历性：$P\left\{ \overline{X\left( t \right) X\left( t+\tau \right) }=R_X\left( \tau \right) \right\} =1$；

满足两个各态遍历性：**遍历过程**。

确定遍历过程的数字特征：只需获得一个样本函数即可。

- $\hat{\mu}_X=\frac{1}{l}\int_0^l{x\left( t \right) \text{d}t}$；$\hat{R}_X\left( \tau \right) =\frac{1}{l-\tau}\int_0^{l-\tau}{x\left( t \right) x\left( t+\tau \right) \text{d}t}$；

书上的例子：

1. 随机相位正弦波$X(t)=a\sin \left( \omega t+\Theta \right)$，$\Theta\sim U[0,2\pi]$.
2. 随机相位周期过程$X(t)=S(t+\Theta)$，周期为$T$，$\Theta\sim U[0,T]$.

例4：平稳过程$X(t)$的$R_X(\tau)$以T为周期，则$\forall t$，$P\{X(t+T)=X(T)\}=1$。

不是遍历过程的平稳过程：$X(t)=Y$，$Y$是个随机变量。

##### 各态遍历性判别定理

给一个随机过程$\{X(t),t\in T\}$，

- 二阶矩过程：$E[X^2(t)]$存在且有限.
- 均方连续：$\forall t_0\in T$，$\underset{t\rightarrow t_0}{\lim}E\left| X\left( t \right) -X\left( t_0 \right) \right|^2=0$.
  均方连续平稳过程的$\overline{X(t)}$的期望和方差：
  $E\left[ \overline{X\left( t \right) } \right] =E\left[ X\left( t \right) \right] =\mu _X$，$D\left[ \overline{X\left( t \right) } \right] =\underset{l\rightarrow +\infty}{\lim}\frac{1}{l}\int_0^{2l}{\left( 1-\frac{\tau}{2l} \right) \left[ R_X\left( \tau \right) -\mu _{X}^{2} \right] \text{d}\tau}$.
- 均值各态遍历定理：均方连续平稳过程，时间均值各态遍历性的充要条件:
  对于$-\infty<t<+\infty$：$D\left[ \overline{X\left( t \right) } \right] =\underset{l\rightarrow +\infty}{\lim}\frac{1}{l}\int_0^{2l}{\left( 1-\frac{\tau}{2l} \right) \left[ R_X\left( \tau \right) -\mu _{X}^{2} \right] \text{d}\tau}=0$.
  对于$0<t<+\infty$：$\underset{l\rightarrow +\infty}{\lim}\frac{1}{l}\int_0^{l}{\left( 1-\frac{\tau}{l} \right) \left[ R_X\left( \tau \right) -\mu _{X}^{2} \right] \text{d}\tau}=0$.

### 随机正弦波的性质

随机正弦波基本形式：$X\left( t \right) =X\cos \left( \omega t+\Theta \right)$，其中$X,\Theta$都是随机变量（随机振幅，随机相位）或者常数。
（这里写成sin和cos都等价，书上都写的余弦，这里就写余弦了）

#### 随机相位：

$$
X(t)=a\cos \left( \omega t+\Theta \right)
$$

其中$\Theta$为随机变量，一般题目会给均匀分布$\Theta\sim U[0,2\pi]$或$U[-\pi,\pi]$，也可能是其他的。

- 期望：$E\left[ X\left( t \right) \right] =a\int_0^{2\pi}{\cos \left( \omega t+\theta \right) \frac{1}{2\pi}\text{d}\theta}=0$，(对sin/cos一个周期积分，恒为零)
- 自相关函数：$R_X=E\left[ X\left( t \right) X\left( t+\tau \right) \right] =a^2E\left[ \cos \left( \omega t+\Theta \right) \cdot \cos \left( \omega \left( t+\tau \right) +\Theta \right) \right] =\frac{a^2}{2}\cos \omega \tau $.
- 二阶矩：$E[X^2(t)]=R_X(0)=\frac{a^2}{2}$ .
- 时间均值：
  $\overline{X\left( t \right) }=\underset{l\rightarrow +\infty}{\lim}\frac{1}{2l}\int_{-l}^l{a\cos \left( \omega t+\Theta \right) \text{d}t}=\underset{l\rightarrow +\infty}{\lim}\frac{a}{2l}\cdot \frac{1}{\omega}\sin \left( \omega t+\Theta \right) \mid_{-l}^{l}\\=\underset{l\rightarrow +\infty}{\lim}\frac{a}{2\omega}\frac{\sin \left( \omega l+\Theta \right) -\sin \left( -\omega l+\Theta \right)}{l}=0$.
  注：最后一步分子是个不超过2的数，分母趋于无穷，所以极限为零。
- 时间相关函数：
  $\overline{X\left( t \right) X\left( t+\tau \right) }=\underset{l\rightarrow +\infty}{\lim}\frac{1}{2l}\int_{-l}^l{a\cos \left( \omega t+\Theta \right) \cdot a\cos \left( \omega \left( t+\tau \right) +\Theta \right) \text{d}t}\\=\underset{l\rightarrow +\infty}{\lim}\frac{a^2}{2l}\int_{-l}^l{\frac{\cos \omega \tau +\cos \left( \omega \left( 2t+\tau \right) +2\Theta \right)}{2}\text{d}t}$$=\frac{a^2}{2}\cos \omega \tau $.
  注，最后一步和时间均值那个积分算起来差不多，不含t的一项被提出积分号外边留下来了，含有t的一项去掉积分号以后分母带l，分子是个有限的(不超过2)的数，取极限以后变成零了。

如果是$X(t)=X\cos \left( \omega t+\Theta \right)$，则数字特征(即$E(\cdots)$)里出现$a^2$的地方换成$EX^2$ ($X$和$\Theta$要相互独立)，时间均值和时间相关函数里的$a$换成$X$（当心，如果振幅是随机的，则自相关函数不一定具有各态遍历性！）

#### 随机振幅：

$$
Z\left( t \right) =X\cos 2\pi t+Y\sin 2\pi t
$$

一般来说$X,Y$会给正态分布或者均匀分布，然后一般还会给$\mu=0$。

- 期望：$E[Z(t)]=\cos 2\pi t\cdot EX+\sin 2\pi t\cdot EY=0$，
- 自相关：$E\left[ Z\left( t \right) Z\left( t+\tau \right) \right] =E\left\{ \left( X\cos 2\pi t+Y\sin 2\pi t \right) \cdot \left( X\cos 2\pi \left( t+\tau \right) +Y\sin 2\pi \left( t+\tau \right) \right) \right\}\\ =EX^2\cos 2\pi t\cos 2\pi \left( t+\tau \right) +EY^2\sin 2\pi t\sin 2\pi \left( t+\tau \right) + \\ E\left( XY \right)\cdot \left( \cos 2\pi t\sin 2\pi \left( t+\tau \right) +\sin 2\pi t\cos 2\pi \left( t+\tau \right) \right)\\=EX^2\cos 2\pi t\cos 2\pi \left( t+\tau \right) +EY^2\sin 2\pi t\sin 2\pi \left( t+\tau \right) +E\left( XY \right) \cdot \sin 2\pi \left( 2t+\tau \right) $.
  如果题目中有$EX^2=EY^2$，且$X,Y$相互独立，$E(XY)=EXEY=0$，则
  $E\left[ Z\left( t \right) Z\left( t+\tau \right) \right] =EX^2\left( \cos 2\pi t\cos 2\pi \left( t+\tau \right) +\sin 2\pi t\sin 2\pi \left( t+\tau \right) \right) =EX^2\cos 2\pi \tau $.

> #### 三角函数和差化积与积化和差
>
> 这里因为用到了，就稍微提一下，能背下来最好，实在背不下来可以现场推导。
>
> 如果要推导的话首先要牢记和角差角公式：
$$
\cos \left( A\pm B \right) =\cos A\cos B\mp \sin A\sin B 
\\
\sin \left( A\pm B \right) =\sin A\cos B\pm \sin B\cos A
$$
> 还有变换：$X=A+B$，$Y=A-B$。
>
> 对于和差化积，以$\cos X+\cos Y$为例，令$X=A+B$，$Y=A-B$，套cos的和差角公式，
> 得到$\cos(A+B)+\cos(A-B)=2\cos A\cos B$，然后由$A=\frac{X+Y}{2},B=\frac{X-Y}{2}$得$\text{2}\cos \frac{X+Y}{2}\cos \frac{X-Y}{2}$.
>
> 对于积化和差，以$\cos A\cos B$为例，令$X=A+B$，$Y=A-B$，还是套cos的和差角公式，
> 得$\cos A\cos B=\frac{1}{2}\left[\cos(A+B)+\cos(A-B)\right]$.

### 马尔可夫链

本章讨论的随机过程$\{X(t),t\in T\}$的状态空间$S$是有限或可列集（或者说$X(t_0)$是离散型随机变量）
（$X(t)=j,j\in S$为一个状态；状态数就是看j有几个取值，题目可能给转移矩阵问状态数，就是阶数）

马尔可夫链定义（**马尔可夫性**）：也称"**无后效性**"，将来时刻只与现在时刻有关，与过去无关
$$
P\left\{ X\left( t_{n+1} \right) =j_{n+1}\mid X\left( t_1 \right) =j_1,X\left( t_2 \right) =j_2,\cdots ,X\left( t_n \right) =j_n \right\} \\=P\left\{ X\left( t_{n+1} \right) =j_{n+1}\mid X\left( t_n \right) =j_n \right\}
$$
**转移概率**：$p_{ij}^{\left( n \right)}\left( t_m \right) =P\left\{ X\left( t_{m+n} \right) =j\mid X\left( t_m \right) =i \right\} $，角标$(n)$表示n步转移，$t_m$表示起始时刻。
书上写的含义：$X(t)$在时刻$t_m$时由状态$i$经过$n$步转移到达状态$j$的($n$步转移)概率。

性质：非负性 $p_{ij}^{\left( n \right)}\left( t_m \right) \ge 0$，规范性:$\sum_{j\in S}{p_{ij}^{\left( n \right)}\left( t_m \right) =1}$.

**齐次马尔可夫链**：一步转移概率$p_{ij}$不依赖于起始时刻$t_m$.

**转移矩阵**：$\boldsymbol{P}=(p_{ij})$，其元素均非负，且<u>每行和为1</u>.

例：伯努利序列，$S=\{0,1\}$，转移矩阵为$\boldsymbol{P}=\left[ \begin{matrix}
	q&		p\\
	q&		p\\
\end{matrix} \right] $ 。

**科尔莫戈罗夫-查普曼方程**：$p_{ij}^{\left( n+l \right)}\left( t_m \right) =\sum_k{p_{ik}^{\left( n \right)}\left( t_m \right) p_{kj}^{\left( l \right)}\left( t_{m+n} \right)}$.
对于齐次马尔可夫链，通常使用矩阵形式，$\boldsymbol{P}^{\left( n \right)}=\boldsymbol{P}^n$，n步转移矩阵就是1步转移矩阵的n次方。

有限维概率分布：由初始时刻分布与转移概率确定。

**平稳分布**：(存在)满足$\boldsymbol{\pi }=\boldsymbol{\pi P}$的$\boldsymbol{\pi }=\left( \pi _0,\pi _1,\cdots ,\pi _j,\cdots \right) $，且$\pi _j\ge \text{0,}\sum_j{\pi _j}=1$.
求法：$\boldsymbol{\pi }^T$为转移矩阵$\boldsymbol{P}$的特征值为$\lambda=1$对应的特征向量。(可能多于一个，也可能没有)
求出特征向量后注意要归一化（所有元素之和为1）。（这部分有点类似于线性代数）

> 求平稳分布的一个例子（2018-2019春 解答题五），转移概率矩阵$\boldsymbol{P}=\left[ \begin{matrix}
0&		\frac{2}{3}&		0&		\frac{1}{3}\\
\frac{1}{3}&		0&		\frac{2}{3}&		0\\
0&		\frac{1}{3}&		0&		\frac{2}{3}\\
\frac{2}{3}&		0&		\frac{1}{3}&		0\\
\end{matrix} \right] $ .
> 解：将$\lambda=1$为特征值的特征矩阵为$E-P$ 写出，并进行初等变换：
$$
\left[ \begin{matrix}
	1&		-\frac{2}{3}&		0&		-\frac{1}{3}\\
	-\frac{1}{3}&		1&		-\frac{2}{3}&		0\\
	0&		-\frac{1}{3}&		1&		-\frac{2}{3}\\
	-\frac{2}{3}&		0&		-\frac{1}{3}&		1\\
\end{matrix} \right] \xrightarrow[r_3,r_4\times 3]{\begin{array}{c}
	r_1+r_3\\
	r_2+r_4\\
\end{array}}\left[ \begin{matrix}
	1&		-1&		1&		-1\\
	-1&		1&		-1&		1\\
	0&		-1&		3&		-2\\
	-2&		0&		-1&		3\\
\end{matrix} \right] \xrightarrow[r_2\rightarrow r_4]{r_2+r_1}\left[ \begin{matrix}
	1&		-1&		1&		-1\\
	0&		-1&		3&		-2\\
	-2&		0&		-1&		3\\
	0&		0&		0&		0\\
\end{matrix} \right] 
\\
\xrightarrow{r_1-r_2}\left[ \begin{matrix}
	1&		0&		-2&		1\\
	0&		-1&		3&		-2\\
	-2&		0&		-1&		3\\
	0&		0&		0&		0\\
\end{matrix} \right] \xrightarrow[r_3\div 5]{r_3+2r_1}\left[ \begin{matrix}
	1&		0&		-2&		1\\
	0&		-1&		3&		-2\\
	0&		0&		-1&		1\\
	0&		0&		0&		0\\
\end{matrix} \right] \xrightarrow{\begin{array}{c}
	r_1-2r_3\\
	r_2+3r_3\\
\end{array}}\left[ \begin{matrix}
	1&		0&		0&		-1\\
	0&		-1&		0&		1\\
	0&		0&		-1&		1\\
	0&		0&		0&		0\\
\end{matrix} \right]
$$
> 令$x_4$为自由变量，可得$\begin{cases}
x_1=x_4\\
x_2=x_4\\
x_3=x_4\\
x_4=x_4\\
\end{cases}$ ，即$X=x_4\cdot \left[ \begin{array}{c}
1\\
1\\
1\\
1\\
\end{array} \right] $，基础解系只有一个线性无关的向量，将其"归一化"并转置得：
> 平稳分布为 $\boldsymbol{\pi}=(\frac{1}{4},\frac{1}{4},\frac{1}{4},\frac{1}{4})$.

几个齐次马尔可夫链模型（书上的例题）：

1. 伯努利独立重复试验
2. 一维粒子随机游走
3. 01电报机



### 补充

这部分内容也不知道放哪比较好，就是做往年题时记下来的一些点。

> 例：（18-19春 解答题三）客车载20人，一共10个车站。如果某站没人下车则客车通过该站不停车，每位乘客在各个车站下车是等可能的，且下车与否相互独立 不受别人影响。记$X$为停车次数，求$EX$。
> （本题并非伯努利独立重复试验，但依旧可以采用类似于伯努利的做法，先求每个子事件，再合并）
> 解：设$X_i$表示第$i$站是否有人下车（是则取1否则取0），则某站没人下车的概率为$P\{X_i=0\}=(\frac{9}{10})^{20}$， $E(X_i)=P\{X_i=1\}=1-(\frac{9}{10})^{20}$，而$X=\sum_{i=1}^{10}X_i$，所以$EX=\sum_{i=1}^{10}EX_i=10(1-(\frac{9}{10})^{20})$。
> 某一站没人下车：即每一个人(20次方)都选择其他车站($\frac{9}{10}$)，所以是$(\frac{9}{10})^{20}$。
> 这道题的$\{X_i\}$并非伯努利独立重复序列，即$i \ne j$ 时$X_i$与$X_j$<u>不是相互独立</u>的，
> 因为$P\{X_i=0,X_j=0\}=(\frac{8}{10})^{20}\ne P\{X_i=0\}\cdot P\{X_j=0\}=(\frac{81}{100})^{20}$，但是由于此题只让计算期望，而期望的可加性不需要相互独立，$E(X+Y)=EX+EY$，所以依旧可以拆分子事件算期望最后求和。

orz