+++
title = "概率论"

date = 2018-09-26T00:00:00
draft = false

authors = ['潘峰']

tags = ["design pattern","go"]
summary = "Go语言设计模式"
+++

<!-- # coding: utf-8 -->


[原文连接111](http://nbviewer.jupyter.org/github/hschen0712/machine_learning_notes/blob/master/PRML/Chap1-Introduction/1.2-probability-theory.ipynb)
## 1.2 概率论回顾

概率，又称或然率、机会率、机率（几率）或可能性，是概率论的基本概念。概率是对随机事件发生的可能性的度量，一般以一个在0到1之间的实数表示一个事件发生的可能性大小。越接近1，该事件更可能发生；越接近0，则该事件更不可能发生。如某人有百分之多少的把握能通过这次考试，某件事发生的可能性是多少，这些都是概率的实例。  

## 求和法则与乘法法则
假设有两个离散随机变量`$X$`和`$Y$`，`$X$`的取值范围为`$x_i,(i=1,2,...,M)$`, $Y$ 的取值范围为`$y_j,(j=1,2,...,L)$`。我们考虑在$N$次实验中同时对$X$和$Y$进行采样，设$n_{ij}$表示$X=x_i$且$Y=y_j$发生的次数，$c_i$表示$X=x_i$发生的次数（不管$Y$取值多少），$r_j$表示$Y=y_j$发生的次数。


那么根据频率学派的观点，$X=x_i$且$Y=y_j$发生的概率，即二者的 **联合概率（joint probability）** 定义为点$(X,Y)$落在单元$(i, j)$的次数占总实验次数的比例：


<div>$$
p(X=x_i,Y=y_j)=\frac{n_{ij}}{N}
$$</div>


$$p(X=x_i)=\frac{c_i}{N}$$



这里我们默认$N\to \infty$。类似地，$X=x_i$的概率$p(X=x_i)$由如下公式给出：




$$p(X=x_i)=\frac{c_i}{N}$$




注意到$c_i=\sum_{j=1}^L n_{ij}$，由此我们可以得到概率论中的 **求和法则（sum rule）**：


$$p(X=x_i)=\frac{c_i}{N}=\sum_{j=1}^L \frac{n_{ij}}{N}=\sum_{j=1}^L p(X=x_i, Y=y_j)$$


如果我们只考虑$X=x_i$的样例中$Y=y_j$样本所占的比例，记为$p(Y=y_j|X=x_i)$，也被称为给定$X=x_i$情况下$Y=y_j$的条件概率，则该条件概率可以由落在单元$(i, j)$内的点的个数与落在第$i$列的点的总数的比值给出：
$$p(Y=y_j|X=x_i)=\frac{n_{ij}}{c_i}$$
在定义了条件概率之后，我们回过头来看联合概率，可以发现：
$$ p(X=x_i, Y=y_j)=\frac{n_{ij}}{N}=\frac{n_{ij}}{c_i}\cdot \frac{c_i}{N}=p(Y=y_j|X=x_i)p(X=x_i)$$
上述公式即为概率论中的*乘法法则（product rule）*。

为了表述方便，我们将$X,Y$的具体取值省略，将两个法则写为：
> $$ \begin{aligned}\textbf{sum rule}\quad\quad &p(X)=\sum\limits_{Y}p(X,Y)\\\textbf{product rule}\quad\quad &p(X, Y)=p(Y|X) p(X)\end{aligned}$$

这两个法则十分重要，因为它们是概率背后运作的机制，构成了概率论的理论基石。

## 贝叶斯公式
根据乘法法则以及联合概率的对称性（$p(X,Y)=p(Y,X)$）可得：
$$p(Y|X)p(X)=p(X|Y)p(Y)$$
上式又可以改写为
$$p(Y|X)=\frac{p(X|Y)p(Y)}{p(X)}$$
这个公式就是概率论中鼎鼎有名的*贝叶斯公式（Bayes' theorem）*，它在机器学习和模式识别中发挥着至关重要的作用。其中$p(Y|X)$称为*后验概率（posterior probability）*，$p(X|Y)$称为*似然函数（likelihood function）*，$p(Y)$称为*先验概率（prior probability）*,$p(X)$称为*归一化因子（normalize factor）*。根据加法公式我们可以把分母用分子中的似然函数和先验概率来表示：
$$p(X)=\sum_{Y}p(X|Y)p(Y)$$
如果联合概率可以分解为各自边缘概率的乘积，即$p(X, Y)=p(X)p(Y)$，则我们说$X$和$Y$彼此独立，并且有$p(Y|X)=p(Y)$，也就是说给定$X$情况下$Y$的分布与$X$的取值无关。

## 概率密度
我们可以把概率的定义从离散的情况推广到连续的情形，在这种背景下，我们引入*概率密度函数（probability density）*$p(x)$来描述连续随机变量$X$的概率分布。
> **概率密度** 当$\delta x\to 0$时，如果$X$落在区间$(x,x+\delta x)$的概率等于$p(x)\delta x$，即
$$\lim_{\delta x\to 0} p\big(X\in(x,x+\delta x)\big)=p(x)\delta x$$
则称$p(x)$为$X$的概率密度函数

注意到当$\delta x\to 0$时，$p(x)\delta x$可以视为图中阴影部分的面积:
![](http://7xikew.com1.z0.glb.clouddn.com/prml-1.12.jpg)
那么$X$落在区间$(a,b)$内的概率$p(X\in(a,b))$就是$p(x)$在区间$(a,b)$内的面积，我们可以用概率密度的积分来表示它：
$$p(X\in(a,b))=\int_a^b p(x)dx$$
此外，考虑到概率的性质，概率密度也必须满足：
> $$ p(x)\geq 0\\ \int_{-\infty}^{\infty} p(x)dx=1$$

利用密度函数在一个区间上的积分等于随机变量落在这个区间上的概率这一性质，我们可以定义*累积密度函数（cdf）*：
$$P(z)=\int_{-\infty}^z p(x) dx$$
$P(z)$表示$X$处于$(-\infty, z)$之间的概率，且满足$P'(x)=p(x)$

假设我们知道$x$的概率密度为$f_X(x)$，如果我们对$x$做一个非线性变换$y=g(x)$（这里$g(x)$必须是单调递增函数）,那么我们可以用如下公式计算$y$的概率密度$f_Y(y)$：
$$ f_Y(y)=f_X(g^{-1}(y))\bigg|\frac{d }{dy}g^{-1}(y)\bigg|$$
这个公式称为*变元公式（change of a variable）*，证明过程如下：  
首先将$P(X\leq x)$简记为$P_X(x)$，将$P(Y\leq y)$简记为$P_Y(y)$。
因为概率密度是分布函数的导数，根据定义我们有
$$\begin{aligned}f_Y(y)&=\frac{d}{dy}P_Y(y)=\frac{d}{dy}P(g(X)\leq y)\\&=\frac{d}{dx}P_X(g^{-1}(y))\bigg|\frac{dx}{dy}\bigg|\\&=f_X(g^{-1}(y))\bigg|\frac{d}{dy}g^{-1}(y)\bigg|\end{aligned}$$

同样地，我们可以将加法公式、乘法公式和贝叶斯公式推广到连续随机变量上：
> $$ \begin{aligned}\textbf{sum rule}\quad\quad &p(x)=\int p(x,y) dy\\\textbf{product rule}\quad\quad &p(x, y)=p(y|x) p(x)\\\textbf{Bayes' rule}\quad\quad &p(y|x)=\frac{p(x|y) p(y)}{\int_Y p(x|y) p(y) dy}\end{aligned}$$

## 期望和协方差
函数$f(x)$在概率密度$p(x)$下的加权平均称为$f(x)$的*期望（expectation）*，当$X$为离散随机变量时期望定义为
$$\mathbb{E}[f]=\sum_x p(x) f(x)$$
当$X$为连续随机变量时期望定义为
$$\mathbb{E}[f]=\int p(x)f(x) dx$$
给定$N$个从分布$p(x)$抽样得到的样本$x_1, x_2, ...,x_N$，我们可以用如下公式近似估计期望：
$$ \frac{1}{N}\sum_{n=1}^N f(x_n)\approx \mathbb{E}[f]$$
当$N\to\infty$时，上式等号成立。  
有时我们希望计算多元函数关于某个变量的期望，我们用下标指定要求期望的变量：
$$E_x[f(x,y)]=\int f(x,y)p(x)dx$$
*条件期望（conditional expectation）*定义为
$$ E_x[f|y]=\int p(x|y)f(x)dx$$
函数$f(x)$的方差定义为
$$ var[f]=\mathbb{E}\big[(f(x)-\mathbb{E}[f(x)])^2\big]$$
经过一番计算，方差可以简化为
$$var[f]=\mathbb{E}[f(x)^2]-\mathbb{E}[(f(x)]^2$$

随机变量$x$和$y$的*协方差（covariance）*定义为
$$ cov[x,y]=\mathbb{E}_{x,y}[\{x-\mathbb{E}[x]\}\{y-\mathbb{E}[y]\}=\mathbb{E}_{x,y}[xy]-\mathbb{E}[x]\mathbb{E}[y]$$
随机向量$\mathbf{x}$和$\mathbf{y}$的*协方差（covariance）*定义为
$$cov[\mathbf{x}, \mathbf{y}]=\mathbb{E}_{\mathbf{x},\mathbf{y}}[\{\mathbf{x}-\mathbb{E}[\mathbf{x}]\}\{\mathbf{y}^\top-\mathbb{E}[\mathbf{y}^\top]\}=\mathbb{E}_{\mathbf{x},\mathbf{y}}[\mathbf{x}\mathbf{y}^\top]-\mathbb{E}[\mathbf{x}]\mathbb{E}[\mathbf{y}^\top]$$

## 贝叶斯概率
上一节，我们以频率的观点来理解概率，并从一个简单的例子出发推导出了概率论中许多常用的结论。在频率派看来，概率定义为在在相同环境条件中重复某个实验（无穷）多次的背景下，一件事发生的相对频率。因此为了得到准确的概率值，我们通常必须重复相同的实验很多次，例如我们需要重复投掷一枚硬币成千上万次才能精确估计其均匀程度。  
然而并不是所有事件的概率都能用频率的观点来解释，考虑以下问题：
>2050年南极冰川是否会全部融化？

因为目前是2016年，我们无法对其进行观测，这就导致了该事件的概率是未定义的。而贝叶斯统计学则为我们提供了一种完全不同的视角来看待这个问题。贝叶斯派认为概率是一种不确定性的度量，是人对于某个不确定事件是否会发生的置信度。贝叶斯的主要思路是通过不断收集证据来修正人对某件事的主观认识，比如我们可以通过观察南极冰川融化的速度来量化其不确定性，从而决定是否要减少温室气体的排放。  
考虑上一节介绍的曲线拟合的例子，我们可以用贝叶斯框架对模型参数$\mathbf{w}$作推断。获得观测样本前我们对于模型参数$\mathbf{w}$的假设以**先验分布**$p(\mathbf{w})$的形式表达，采集到的观测数据$\mathcal{D}=\{(x_1,t_1),...,(x_N, t_N)\}$通过**似然函数**$p(\mathcal{D}|\mathbf{w})$发挥作用，注意到它是关于$\mathcal{D}$（已知）的分布，因此它是关于$\mathbf{w}$的函数，反映了在不同的模型参数取值下产生该组观测值的可能性。透过似然函数，我们将关于$\mathbf{w}$的置信度转变为了**后验分布**$p(\mathbf{w}|\mathcal{D})$的形式，它表达了在获得观测数据后对于先验$p(\mathbf{w})$的修正。具体地，依据贝叶斯公式有：  
$$p(\mathbf{w}|\mathcal{D})=\frac{p(\mathcal{D}|\mathbf{w})p(\mathbf{w})}{p(\mathcal{D})}$$
注意到$p(\mathcal{D})$只是个定值，起到归一化作用，我们可以将其用先验和似然的乘积关于$\mathbf{w}$的积分表示出来
$$p(\mathcal{D})=\int p(\mathcal{D}|\mathbf{w})p(\mathbf{w})d\mathbf{w}$$
如果忽略$p(\mathcal{D})$，我们可以将先验、似然、后验之间的关系表达为如下的形式：
$$ posterior \propto likelihood \times prior $$
其中$\propto$表示正比符号，三个量都可以视为$\mathcal{w}$的函数。  
无论是频率派还是贝叶斯派，似然函数都起着重要的作用，然而对似然函数使用方式的不同是两者最本质的区别。以上一节介绍的曲线拟合为例，频率派认为参数$\mathbf{w}$固定而数据是随机产生的，我们通过最大化似然函数的思想利用观测数据去反推这个值。而贝叶斯派则认为数据集是确定的，模型参数$\mathbf{w}$是随机的，我们通过似然函数将先验修改为后验。频率派利用交叉验证来选择合适的模型，但贝叶斯中先验的选择通常出于数学上的方便，而不是是否符合直觉。    
贝叶斯一直面临着两个难题：
1. 合理的推断依赖于合适的先验，如何选取合适的先验却一直被频率派诟病
2. 一个完整的贝叶斯推断过程（比如作预测或比较模型）通常包含参数空间的积分，带来高昂的计算代价

近几十年来随着计算机运算速度的提高，后验概率的计算逐渐变得可行，目前可行的方法有两种：
1. 基于采样的方法 主要代表是MCMC，Gibbs采样。这类方法的优点是精度高，适用于任何形式的后验估计；缺点是效率低，只适用于小规模数据。
2. 基于优化的方法 主要代表是变分贝叶斯。这类方法的优点是速度快，适用于大规模数据；缺点是牺牲了精度以换取速度的提升。


## 高斯分布
高斯分布是概率论中最常用的概率分布之一，其定义如下
$$\frac{1}{(2\pi \sigma^2)^{1/2}} \exp({-\frac{1}{2\sigma^2} (x-\mu)^2}) $$
其中$\mu$是均值，$\sigma^2$是方差，$\sigma$称为标准差，方差的倒数$\beta=1/\sigma^2$称为精度。
可以证明，高斯分布满足概率的以下两个性质：
$$\mathcal{N}(x|\mu,\sigma^2)>0\\\int_{-\infty}^{\infty}\mathcal{N}(x|\mu,\sigma^2)dx=1$$
高斯分布的期望为
$$\mathbb{E}[x]=\int_{-\infty}^{\infty}\mathcal{N}(x|\mu,\sigma^2)\ x\ dx=\mu$$
高斯分布的二阶矩为
$$\mathbb{E}[x^2]=\int_{-\infty}^{\infty}\mathcal{N}(x|\mu,\sigma^2)\ x^2\ dx=\mu^2+\sigma^2$$
方差为
$$var[x]=\mathbb{E}[x^2]-\mathbb{E}[x]^2=\sigma^2$$
设$\mathbf{x}$是$D$维的向量，则其对应的多元高斯分布的概率密度函数为：
$$\mathcal{N}(\mathbf{x}|\mathbf{\mu},\Sigma)=\frac{1}{(2\pi)^{D/2}}\frac{1}{|\Sigma|^{1/2}}\exp\big\{-\frac{1}{2}(\mathbf{x}-\mathbf{\mu})^\top\Sigma^{-1}(\mathbf{x}-\mathbf{\mu})\big\}$$
其中$\mathbf{\mu}$是$D$维的均值向量，$\Sigma$是$D\times D$的协方差矩阵，$|\Sigma|$表示$\Sigma$的行列式。  
现在假设有一组数据$\mathrm{x}=\{x_1,...,x_N\}$,为了确定高斯分布的参数我们假定它们是独立同分布地从同一个分布产生的，于是这组数据的似然函数可以表示为
$$p(\mathrm{x}|\mu,\sigma^2 )=\prod_{n=1}^N \mathcal{N}(x_n|\mu, \sigma^2)$$
频率派最常用的参数估计方法是最大似然估计，其思想是通过最大化似然函数找到参数的估计。由于对似然函数取对数不影响优化，我们可以得到对数似然函数：
$$\ln\ p(\mathrm{x}|\mu,\sigma^2 )=-\frac{1}{2\sigma^2}\sum_{n=1}^N(x_n-\mu)^2-\frac{N}{2}ln\ \sigma^2-\frac{N}{2}\ln\ (2\pi)$$
关于$\mu$优化，得到其最大似然估计
$$\mu_{ML}=\frac{1}{N}\sum_{n=1}^N x_n$$
注意到$\mu$的最大似然估计等价于样本均值。关于$\sigma^2$我们得到
$$\sigma_{ML}=\frac{1}{N}\sum_{n=1}^N (x_n-\mu_{ML})^2$$
也就是样本方差。
注意到最大似然估计是有偏估计：
$$\mathbb{E}[\mu_{ML}]=\mu\\\mathbb{E}[\sigma^2_{ML}]=\bigg(\frac{N-1}{N}\bigg)\sigma^2$$
也就是说$\sigma^2_{ML}$低估了$\sigma^2$，这种有偏性随着样本量增大而逐渐减轻，当$N\to\infty$时这种有偏性消失。

## 重访曲线拟合
让我们回到曲线拟合问题上来，这次我们将从概率的角度来重塑我们对于这个问题的理解，我们将证明当假设误差服从一个高斯分布时，通过最大似然估计（MLE，maximum likelihood estimation）可以得到最小二乘的目标函数。  
假设给定输入变量 $x$，目标变量值$t$服从一个均值为$y(x,\mathbf{w})$的高斯分布:
$$ p(t|x, \mathbf{w},\beta)=\mathcal{N}(t| y(x,\mathbf{w}), \beta^{-1}) $$
其中$\beta=1/\sigma^2$ 为高斯的精度。  
我们进一步假设数据集$\mathrm{x}=\{x_1,...,x_N\}$中的样本点独立同分布地从上述的高斯分布产生。那么该数据集的似然函数为:
$$ p( \mathbf{t}| \mathbf{x}, \mathbf{w},\beta )=\prod_{n=1}^N  \mathcal{N}(t_n| y(x_n,\mathbf{w}), \beta^{-1})  $$

最大似然估计的目标是最大化对数似然函数：
$$\mathbf{w}_{ML}=\arg\max_{\mathbf{w}}\ln\ p( \mathbf{t}| \mathbf{x}, \mathbf{w},\beta )$$
对似然函数取对数的好处有三：
1. 对数函数是单调递增函数，这意味着最大化对数似然等价于最大化似然函数
2. 乘积的对数可以转化为对数的和，在数学上更易于求导
3. 由于概率值介于0，1之间，连续多个概率值的乘积可能导致概率值下溢，取对数可以防止下溢保证结果的精度

通过取对数，我们得到对数似然函数：
$$ \mathcal{L}(\mathbf{w},\beta)=ln \, p( \mathbf{t}| \mathbf{x}, \mathbf{w},\beta )=-\frac{\beta}{2}\sum_{n=1}^N\{y(x_n,\mathbf{w})-t_n\}^2+ \frac{N}{2}ln\, \beta- \frac{N}{2} ln\, (2\pi) $$
首先，我们先来优化$\mathbf{w}$。注意到后两项不依赖于 $\mathbf{w}$，因此我们可以丢掉它们。于是我们的目标变为最大化 $- \frac{\beta}{2}\sum_{n=1}^N\{y(x_n,\mathbf{w})-t_n\}^2$，其等价于最小化$ \frac{\beta}{2}\sum_{n=1}^N \{ y(x_n,\mathbf{w})-t_n \}^2$。基于$\mathbf{w}$不依赖于$\beta$ 且 $\beta>0$的事实，我们可以任意放缩系数$ \frac{\beta}{2}$。不失一般性，我们将系数设为$1/2$。最终，我们的目标函数定义如下:
$$ \mathbf{w}_{ML}=\arg\min_{\mathbf{w}}  \frac{1}{2}\sum_{n=1}^N \{ y(x_n,\mathbf{w})-t_n \}^2 $$
这个函数有点眼熟，它不就是1.1节中的平方和误差么？至此我们证明了当假设误差服从一个高斯分布时，从最大似然估计（MLE，maximum likelihood estimation）出发可以得到最小二乘的目标函数。  
接着，我们还要优化$\beta$:
$$ \frac{\partial  \mathcal{L}(\mathbf{w},\beta)}{\partial \beta} =   -\frac{1}{2}\sum_{n=1}^N\{y(x_n,\mathbf{w})-t_n\}^2+ \frac{N}{2\beta}=0 $$
由此可得
$$ \frac{1}{\beta_{ML}}=\sigma_{ML}^2=  \sum_{n=1}^N\{y(x_n,\mathbf{w})-t_n\}^2$$
将$\mathbf{w}_{ML},\beta_{ML}$代回到本节开头的概率密度式就得到了关于$t$的预测分布：
$$p(t|x, \mathbf{w}_{ML},\beta_{ML})=\mathcal{N}(t| y(x,\mathbf{w}_{ML}), \beta_{ML}^{-1}) $$

接下来介绍一种更“贝叶斯”的方法，我们可以对参数$\mathbf{w}$假设一个先验，为了数学上的方便不妨假设其为一个零均值，协方差为对角阵的多元高斯：
$$p(\mathbf{w}|\alpha)=\mathcal{N}(\mathbf{w}|\mathbf{0},\alpha^{-1}\boldsymbol{I})=\Big(\frac{\alpha}{2\pi}\Big)^{(M+1)/2}\exp(-\frac{\alpha}{2}\mathbf{w}^T\mathbf{w})$$
其中$\alpha$是高斯的精度，也称为超参数（即参数的参数）；$M+1$是$\mathbf{w}$中的参数个数。  
根据贝叶斯公式，后验正比于似然与先验的乘积：
$$p(\mathbf{w}|\mathbf{x},\mathbf{t},\alpha,\beta)\propto p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha)$$
这个公式乍看之下挺难理解，不过只要你学了概率图模型，你就会发现要理解它其实非常容易。首先我们画出这几个变量的概率图模型：
<img src="http://7xikew.com1.z0.glb.clouddn.com/prml-1.2-1.png" width="500">
很显然，$\mathcal{w}$的后验依赖于$\mathrm{t}$和$\alpha$，此时还需注意到$\mathrm{x}$、$\beta$和$\mathbf{w}$是$\mathrm{t}$的共同祖先，因此$\mathcal{w}$也依赖于$\mathrm{x}$和$\beta$。  
我们将后验写为如下形式
$$p(\mathbf{w}|\mathbf{x},\mathbf{t},\alpha,\beta)=\frac{p(\mathbf{w},\mathbf{x},\mathbf{t},\alpha,\beta)}{p(\mathbf{x},\mathbf{t},\alpha,\beta)}\propto p(\mathbf{w},\mathbf{x},\mathbf{t},\alpha,\beta)$$
根据概率图模型的链式法则，我们可以将联合概率分解为
$$p(\mathbf{w},\mathbf{x},\mathbf{t},\alpha,\beta)=p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha)p(\alpha)p(\beta)$$
由于我们只关心与$\mathbf{w}$有关的概率，因此$p(\alpha)$和$p(\beta)$可以省略，于是我们就得到了
$$p(\mathbf{w}|\mathbf{x},\mathbf{t},\alpha,\beta)\propto p(\mathbf{t}|\mathbf{x},\mathbf{w},\beta)p(\mathbf{w}|\alpha)$$

将$p(\mathrm{t}|\mathrm{x},\mathbf{w},\beta)$和$p(\mathbf{w}|\alpha)$的具体形式代入并取对数，通过最大化对数后验分布我们得到关于$\mathbf{w}$的最大后验估计（MAP, maximum a posteriori）：
$$\begin{aligned}\mathbf{w}_{MAP}&=\arg\max_{\mathbf{w}} \ln\ p(\mathbf{w}|\mathbf{x},\mathbf{t},\alpha,\beta)\\&=\arg\max_{\mathbf{w}} - \frac{\beta}{2}\sum_{n=1}^N(t_n-y(x_n,\mathbf{w}))^2-\frac{\alpha}{2}\mathbf{w}^T\mathbf{w}\\&=\arg\min_{\mathbf{w}} \frac{1}{2}\sum_{n=1}^N(t_n-y(x_n,\mathbf{w}))^2+\frac{\alpha}{2\beta}\mathbf{w}^T\mathbf{w}\\&= \arg\min_{\mathbf{w}} \frac{1}{2}\sum_{n=1}^N(t_n-y(x_n,\mathbf{w}))^2+\frac{\lambda}{2}\mathbf{w}^T\mathbf{w}\end{aligned}$$
其中$\lambda=\frac{\alpha}{\beta}$，就是1.1节中的正则化系数。


## 贝叶斯曲线拟合

前面介绍的MLE和MAP都属于点估计，这一节将介绍一种更完全的贝叶斯方法。回顾曲线拟合的目标，我们希望为给定的输入$\hat{x}$预测其对应的输出$\hat{t}$。这里假设参数$\alpha$和$\beta$已知，于是可以省略$\mathbf{w}$的后验概率中的参数，写为$p(\mathbf{w}|\mathbf{x},\mathbf{t})$。通过对下式右端关于$\mathbf{w}$积分，我们可以得到$t$的后验预测分布（posterior predictive distribution）：
$$ p(t|x,\mathbf{x},\mathbf{t})=\int p(t|x,\mathbf{w})p(\mathbf{w}|\mathbf{x},\mathbf{t})d\mathbf{w}$$
这个公式是我读这本书遇到的第一道坎，貌似很多人也在这个公式上卡了很久。我说一下我对这个公式的理解：  
**第一种理解**：我们知道在贝叶斯中数据是已知的，只有参数$\mathbf{w}$是不确定的，因此式中$x,\mathbf{x},\mathbf{t}$都是确定的，为了直观我们可以把已知的都省略，于是原式变为
$$p(t)=\int p(t|\mathbf{w})p(\mathbf{w}) d\mathbf{w}=\int p(t,\mathbf{w})d\mathbf{w}$$
这就很好理解了，就是对$\mathbf{w}$做marginalization（运用概率论的乘法公式和加法公式，连续的情况下求和变为积分）。  
**第二种理解**：概率图模型，需要用到*D-separation*理论（D-Separation是一种用来判断变量是否条件独立的图形化方法）。以下举个D-separation最简单的例子，更多的理论知识请参考PRML第8章
<img src="http://7xikew.com1.z0.glb.clouddn.com/d_separation.png" width="300">
我们要确定上图中$a$和$b$的关系，则可以分为两种情况来讨论  
首先依据链式法则,我们写出该图模型的联合概率
$$p(a,b,c)=p(c)p(a|c)p(b|c)$$
1）如果随机变量$c$已经被观测，则$a$与$b$条件独立，即$p(a,b|c)=p(a|c)p(b|c)$  
证明过程如下：
$$p(a,b|c)=\frac{p(a,b,c)}{p(c)}=\frac{p(c)p(a|c)p(b|c)}{p(c)}=p(a|c)p(b|c)$$
同理，我们还能证明$p(b|a, c)=p(b|c)$
$$p(b|a, c)=\frac{p(a,b,c)}{p(a, c)}=\frac{p(c)p(a|c)p(b|c)}{p(c)p(a|c)}=p(b|c)$$
2）如果随机变量$c$未被观测，通过对$p(a,b,c)$关于$c$积分我们获得$a$和$b$的联合概率
$$p(a,b)=\sum_{c}=p(c)p(a|c)p(b|c)$$
通常情况下，$p(a,b)$是不等于$p(a)p(b)$的，因此$a$和$b$相互不独立  
接下来我们讨论回归模型的概率图模型：

<img src="http://7xikew.com1.z0.glb.clouddn.com/regression-graph-model.png" width="300">

接下来我们来证明原式成立：
$$\begin{aligned}p(t|x,\mathbf{x},\mathbf{t})&=\frac{p(t,x,\mathbf{x},\mathbf{t})}{p(x,\mathbf{x},\mathbf{t})}\\&=\int \frac{p(t,x,\mathbf{x},\mathbf{t}, \mathbf{w})}{p(x,\mathbf{x},\mathbf{t})}d\mathbf{w}\\&=\int \frac{p(t,x,\mathbf{x},\mathbf{t}, \mathbf{w})}{p(x,\mathbf{x},\mathbf{t}, \mathbf{w})}\frac{p(x,\mathbf{x},\mathbf{t}, \mathbf{w})}{p(x,\mathbf{x},\mathbf{t})}d\mathbf{w}\\&=\int p(t|x,\mathbf{x},\mathbf{t}, \mathbf{w})p(\mathbf{w}|x,\mathbf{x},\mathbf{t})d\mathbf{w}\end{aligned}$$
根据图模型的*D-separation*理论，$\mathbf{w}$被观测的条件下，上图中$\mathbf{x}$到$t$（在图中是$\hat{t}$）的通路被阻断，因此$t$与$\mathbf{x}$及$\mathbf{t}$相互独立，则
$$p(t|x,\mathbf{x},\mathbf{t}, \mathbf{w})=p(t|x,\mathbf{w})$$
接着我们考察概率$p(\mathbf{w}|x,\mathbf{x},\mathbf{t})$，由于$t$尚未被观测，根据图模型*D-separation*理论，$\mathbf{w}$和$x$应该是独立的，此外由于$\mathbf{t}$已经被观测，那么$\mathbf{w}$与$\mathbf{x}$条件不独立。于是
$$p(\mathbf{w}|x,\mathbf{x},\mathbf{t})=p(\mathbf{w}|\mathbf{x},\mathbf{t})$$
综上，我们知道
$$p(t|x,\mathbf{x},\mathbf{t})=\int p(t|x,\mathbf{w})p(\mathbf{w}|\mathbf{x},\mathbf{t})d\mathbf{w}$$

在理解了后验预测分布是怎么来的以后，我们需要计算该分布。注意到似然函数$p(t|x,\mathbf{w})$是个高斯分布，而先验$p(\mathbf{w})$也是个高斯分布，贝叶斯统计学里可以证明，当先验和似然都是高斯分布时则后验与先验共轭，即后验也是一个高斯分布（准确来说，只要都属于指数分布族该结论都能成立）。至此，我们知道了$p(t|x,\mathbf{w})$和$p(\mathbf{w}|\mathbf{x},\mathbf{t})$都是高斯分布，这还没完，因为我们还没求积分呢。刚开始看到这个积分，我也十分头大，觉得根本无从下手，于是我就开始在网上漫无目的地寻找线索，也许是因为我的搜索技术高超，最后还真被我找到了点蛛丝马迹。其实这个积分是两个高斯分布的卷积，可以证明两个高斯分布的卷积还是一个高斯分布，经过一大串的计算可以得到如下的结论（具体怎么算的，看我的另一篇[笔记](../Chap3-Linear-Models-For-Regression/summary-baysian-linear-regression.ipynb)）：
$$ p(t|x,\mathbf{x}, \mathbf{t})=\mathcal{N}\big(t|m(x),s^2(x)\big)$$
其中均值和方差都是关于$x$的函数，定义为
$$ m(x)=\beta\phi(x)^T \mathbf{S}\sum_{n=1}^N\phi(x_n)t_n\\s^2(x)=\beta^{-1}+\phi(x)^T\mathbf{S}\phi(x)$$
这里矩阵$\mathbf{S}$由下式给出
$$\mathbf{S}^{-1}=\alpha \mathbf{I}+\beta\sum_{n=1}^N\phi(x_n)\phi(x_n)^T$$
其中$\mathbf{I}$是单位矩阵，$\phi(x)$是关于$x$的特征变换，其中$\phi_i(x)=x^i,i=0,...,M$