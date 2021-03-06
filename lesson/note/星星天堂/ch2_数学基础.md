## 2 信息论基础

### 2.1 熵

熵又称为自信息， 是用来描述一个随机变量的不确定性的数量。表示信源 X 每发出一个符号所提供的的平均信息量。 

一个随机变量的熵越大，它的不确定性就越大。 正确估计其值的可能性就越小。越不确定的随机变量越需要大的信息量用于确定它的值。
$$
H(X) =  -\sum_{x\in R} p(x) \log_2{p(x)} 
$$
公式中以2为底，表示公式定义的熵的单位为2进制比特。 

**自然语言处理中使用熵的概念**；

通常做法，根据已知样本设计特征函数，假设存在 k 个特征函数 $f_i (i=1,2,\cdots k )$,在建模过程中对输出有影响，所建立的模型应满足所有这些特征的约束。所建立的模型 $p$ 应该属于 这 k 个特征函数约束下所有产生模型的集合C。 使熵 $H(p) $值最大的模型用来推断某种语言存在的可能性，或者作为进行某种处理操作的可靠性依据： 
$$
\hat{p} = \begin{matrix}	
\quad	\\	
\arg \max \\	
_{p \in C}	
\end{matrix}
H(p)
$$

### 2.2 联合熵 和条件熵

X,Y 为一对离散型随机变量 $X,Y \sim p(x,y)$ , X,Y的联合熵 H(X,Y) 定义：
$$
H(X,Y) = - \sum_{x \in X} \sum_{y \in Y} p(x,y) \log{p(x,y)}
$$
联合熵描述一对随机变量平均所需要的信息量。

条件熵定义：
$$
H(Y|X) =- \sum_{x \in X} \sum_{y \in Y} p(x,y) \log{p(y|x)}
$$

$$
H(X,Y) = H(X) + H(Y|X)
$$



### 2.3 互信息

根据熵的链式规则： 
$$
H(X,Y) = H(X) + H(Y|X) = H(Y) + H(X|Y)
$$
所以：
$$
H(X) - H(X|Y) = H(Y) - H(Y|X)
$$
这里的差叫做 **X和Y** 的互信息。记做 $I(X;Y)$ 或者定义为： $(X,Y) \sim p(x,y)$,则X,Y 之间的互信息 $I(X;Y) = H(X) - H(X|Y)$。

$I(X;Y)$ 反映的是在知道了 Y值以后 X的不确定性的减少量。理解了Y 值可以透露出多少X 的信息量。
$$
\begin{align}
& I(X;Y) = \sum_{x,y} p(x,y) \log\cfrac{p(x,y)}{p(x),p(y)}
\\
& H(X) = I(X;X)
\end{align}
$$
互信息的链式规则：

定义：  
$$
\begin{align}
I(X;Y |Z) &=  I((X;Y)|Z) 
\\
& =H(X|Z) - H(X|Y,Z)
 \\
& =H(X) - I(X;Z) -（H(X)- I(X;Y,Z))
\\
& = I(X;Y,Z) - I(X;Z)
\end{align}
$$

$$
I(X_{1n};Y) = \sum_{i=1}^n I(X_i;Y|X_1 ,\cdots,X_{i-1})
$$

结论

如果$I(X;Y)>>0$，则表明*X*和*Y*是高度相关的。

如果$I(X;Y)=0$，即$p(x,y)=p(x)p(y)$则说明两者完全独立。

如果$I(X;Y)<<0$，则表明*Y*的出现不但未使*X*的不确定性降低，反而加大了其不确定性，这通常是不利的。



**互信息在词汇聚类（word clustering）、汉语自动分词、词义消歧等问题的研究中具有重要用途**

### 2.4 相对熵

相对熵的概念：**衡量两个随机变量的分布的差距**。

也称为  KL 距离
$$
D(p \mid\mid q) = \sum_{x \in X} p(x) \log(\cfrac{p(x)}{q(x)})
$$
如果 $p(x)$ 和 $q(x)$ 概率分布完全相等的话 就是没有距离

如果其中有分布的话，无穷大没有办法比较



### 2.5 交叉熵

用来衡量估计模型与真实概率分布之间的差异。

如果一个随机变量 $X \sim p(x)$ ,$q(x)$ 为近似 $p(x)$ 的概率分布。那么随机变量 X 和 模型 q 之间的交叉熵定义为：
$$
H(X,q) = H(X) + D(p||q) = -\sum_xp(x)\log q(x)
$$


对于语言来说：$L =(X) \sim p(x)$  与其模型 q 的交叉熵定义为：
$$
H(X,q) =  -\lim_{n \to \infty}\cfrac{1}{n}\sum_{x_1^n}p(x_1^n)\log q(x_1^n)
$$
$x_1^n$ 是 语言L 的词序列， $p(x_1^n)$ 为 $x_1^n$ 的概率(理论值)， $q(x_1^n)$ 为模型 q 对 $x_1^n$ 的概率估计值。

**交叉熵与模型在测试语料中分配给每个单词的平均概率所表达的含义正好相反，模型的交叉熵越小，模型的表现越好。**



### 2.6 困惑度

困惑度： 代替交叉熵衡量语言模型的好坏。

给定语言 L的样本 $l_1^n = l_1 \cdots l_n$  L的困惑度 $PP_q$ 定义：
$$
PP_q = 2^{H(L,q)} \approx 2^{-\frac{1}{n}\log{q(l_1^n)}} = [q(l_1^n)^{\frac{1}{n}}]
$$
后面求解：
$$
\begin{align}
& x = 2^{-\frac{1}{n}\log_2{q(l_1^n)}}
\\
& \log_2{x} =  -\frac{1}{n}\log_2{q(l_1^n)}
\\
& \log_2{x} =  \log_2{q(l_1^n)^{-\frac{1}{n}}}
\\
& \
\end{align}
$$
语言模型设计的任务就是寻找困惑度最小的模型，使其最接近真实语言的情况。

从困惑度的计算式可以看出来，它是对于样本句子出现的概率，在句子长度上标准化了结果。它越小，说明出现概率越大，所得模型就越好。



### 2.7 噪声信道模型



根本问题在于如何估计信道输出中获取多少信息量。 

信息熵可以定量地估计信源每发送一个符号所提供的的平均量。

噪声信道模型： **优化噪声信道中信号传输的吞吐量和准确率，基本假设是一个信道的输出以一定概率依赖于输入。** 

一个信号经过一个信道，会由于压缩编码，噪声引入，然后在解码的时候就会多少有一点失真。

在自然语言处理中，很多问题也都可以归结为这样的模型。给定输出*O*(可能含有误传信息)的情况下，如何从所有可能的输入*I*中选出最可能的那个：
$$
\hat{I} = argmax_I (p(I|O)) = argmax_I (\frac{P(I)p(O|I)}{p(O)}) = argmax_I (p(I)p(O|I))
$$
其中*p*(*I*)成为语言模型，是指在输入语言中“词”序列的概率分布；

另一个*p*(*O*|*I*)成为**信道概率**。

对应到实际的NLP问题，比如说机器翻译在进行汉译英的时候，汉语句子看作是信道输出O，求出最可能的信道输入英语句子I。

**噪声信道模型在NLP中有非常多的用途，除了机器翻译以外，还用于词性标注、语音识别、文字识别等很多问题的研究。**