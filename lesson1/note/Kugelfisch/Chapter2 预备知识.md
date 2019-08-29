# Chapter 数学基础

## 2.1 概率论
### 2.1.1 概率
非负性，规范性，可列可加性

## 2.2 信息论基础

+ **熵**

$$
H(X)=-\sum_{x \in X} p(x) \log _{2} p(x)
$$
	单位： bit
	描述一个随机变量的不确定性的数量
	自信息

+ **联合熵**
	
$$
H(X, Y)=-\sum_{x \in X} \sum_{y \in Y} p(x, y) \log _{2} p(x, y)
$$
	描述一对随机变量平均所需的信息量
	
+ **条件熵**

$$
\begin{aligned} H(Y | X) &=\sum_{x \in X} p(x) H(Y | X=x) \\ &=\sum_{x \in X} p(x)\left[-\sum_{y \in Y} p(y | x) \log _{2} p(y | x)\right] \\ &=-\sum_{x \in X} \sum_{y \in Y} p(x, y) \log _{2} p(y | x) \end{aligned}
$$
	
+ **连锁规则**

$$
\begin{aligned} H(X, Y) &=-\sum_{i=-X,=r=r}^{y=r,=r} p(x, y) \log [p(x) p(y | x)] \\ &=-\sum_{x=x} \sum_{e x} p(x, y)|\log p(x)+\log p(y | x)] \\ &=-\sum_{x=x, y=r}^{x x} \sum_{v=r}^{p} p(x, y) \log p(x)-\sum_{x=x=x+y} \sum_{x=x} x^{r}(x, y) \log p(y | x) \\ &=-\sum_{i=x}^{x, y} p(x) \log p(x)-\sum_{i=x=y=1}^{N} \sum_{p=x}^{p}(x, y) \log p(y | x) \\ &=H(X)+H(Y | X) \end{aligned}
$$

+ **相对熵**

$$
D(p \| q)=\sum_{x \in X} p(x) \log \frac{p(x)}{q(x)}
$$
$$
0 \log (0 / q)=0, p \log (p / 0)=\infty
$$
	两个随机变量分布的差距，分布相同，相对熵为0

+ **交叉熵**

$$
\begin{aligned} H(X, q) &=H(X)+D(p \| q) \\ &=-\sum_{x} p(x) \log q(x) \end{aligned}
$$
	衡量估计模型与真实概率分布之间的差异
	
对于语言来说：
$$
H(L, q)=-\lim _{n \rightarrow \infty} \frac{1}{n} \sum_{x_{1}^{n}} p\left(x_{1}^{n}\right) \log q\left(x_{1}^{n}\right)
$$
	大量数据和模型q可以让我们使得交叉熵最小

+ **困惑度**

交叉熵+放大指数

语言模型设计就是寻找困惑度最小的模型，最接近真实语言

+ **互信息**

$$
I(X ; Y)=H(X)-H(X | Y)
$$

$$
I(X ; Y)=\sum_{x \in X} \sum_{y \in Y} p(x, y) \log _{2} \frac{p(x, y)}{p(x) p(y)}
$$
	知道了Y之后，X不确定性的减少量（Y透露了多少X的信息）
	
*Example:*
$$
I(x ; y)=\log _{2} \frac{p(x, y)}{p(x) p(y)}=\log _{2} \frac{p(y | x)}{p(y)}
$$
互信息越大，两个汉字之间的联系越紧密，越可能成词

+ **字的耦合度**
$$
\operatorname{Couple}\left(c_{i}, c_{i+1}\right)=\frac{N\left(c_{i} c_{i+1}\right)}{N\left(c_{i} c_{i+1}\right)+N\left(\cdots c_{i} | c_{i+1} \cdots\right)}
$$
	成词的次数/（成词的次数+不成词的次数）
	不考虑不成词的情况
	汉字的结合强度

***耦合度与互信息的差别***

耦合度是考虑两个字连续出现的情况下，成词与不成词的比例关系
互信息是考虑共同出现和非共同出现的关系
如果两个字不经常连续出现，一旦连续出现则大概率成词
则:
互信息很小（不符合预期）耦合度很高（符合预期）

两个单个离散事件(xi, yj)之间的互信息I(xi, yj)可能为负值，这种情况我们通常称之为点式互信息(pointwisemutual information)。两个随机变量(X, Y)之间的互信息不可能为负值，即I(X, Y)>=0。后者通常称为平均互信息。

+ **噪声信道模型**

	优化噪声信道中信号传输的吞吐量和准确率
	- 尽量占用小信道
	- 增加可控冗余


- 信道容量

$$
C=\max _{p(X)} I(X ; Y)
$$

## 2.3 应用举例

### 词汇歧义消解

基于**贝叶斯分类器**进行消解
$$
\underset{s_{i}}{\arg \max } p\left(s_{i} | C\right)
$$

通过概率乘积转换为加法运算（防止下溢出）
$$
\hat{s}_{i}=\underset{s_{i}}{\arg \max }\left[\log p\left(s_{i}\right)+\sum_{v_{k} \in C} \log p\left(v_{k} | s_{i}\right)\right]
$$

基于**最大熵**的消除歧义方法

在不知道分布的情况下，通过已知知识的熵对未知进行推断
即
**当已知部分熵最大的时候，未知部分的熵是最合理的

$$
p^{*}(a | b)=\frac{1}{Z(b)} \exp \left(\sum_{j=1}^{l} \lambda_{j} \cdot \underbrace{\left.f_{j}(a, b)\right)}_{k+\{k, k}\right.
$$