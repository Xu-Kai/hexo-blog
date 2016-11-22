title: 分治关系递推公式的求解
date: 2016-11-22 13:16:26
tags: algorithms
categories: algorithms
---
在分治算法中，问题通常被分为几个子问题，其中每个子问题也将被递归求解，得到结果后，在使用一个操作来组合子问题的的解得到原始问题的解。假设有$a$个子问题，每个问题的规模是原始问题的$\frac{1}{b}$	，并且用于组合各个子问题解的算法运行时间是$cn^{k}$，其中$a, b, c$和$k$是常数，即
$$T(n)=aT(\frac{n}{b}) + cn^{k}$$
为了简化，假定$n=b^{m}$，则$\frac{n}{b}$总是一个整数($b$是一个比1大的整数)。尝试展开上式：
$$T(n)=a(aT(\frac{n}{b^2}) + c(\frac{n}{b})^{k}) + cn^{k}=a(a(aT(\frac{n}{b^3}) + c(\frac{n}{b^2})^{k})  + c(\frac{n}{b})^{k}) + cn^{k}$$
一般来说，如果展开到$\frac{n}{b^m}=1$，可以得到
$$T(n)=a(a(...T(\frac{n}{b^m}) + c(\frac{n}{b^{m-1}})^{k})  + ...) + cn^{k}$$
假设$T(1)=c$(对不同的值对最终结果的影响仅仅是常数上变化)。那么
$$T(n)=ca^{m} + ca^{m-1}b^{k} + ca^{m-2}b^{2k} + ... + cb^{mk}$$
这意味着
$$T(n)=c\sum_{n=1}^{m}a^{m-i}b^{ik}$$

$$=ca\sum_{n=1}^{m}(\frac{b^{k}}{a})^i$$
这是一个简单的几何级数，分三种情况，分别取决于 $\frac{b^k}{a}$ 是否小于，大于或等于1。
**情况1：** $a>b^k$
这种情况 下，几何级数的因子小于1，所以当$m$趋于无穷时，级数收敛于常数。因此，$T(n)=O(a^m)$。由于$m=\log_2n$，我们得到$a^m=a^{\log_bn}=n^{\log_ba}$(最后一个等式可以通过对两边都取以b为底数的对数得证)。所以有
$$T(n)=O(n^{\log_ba})$$
**情况2：** $a=b^k$
在此情况下，几何级数的因子等于1，从而$T(n)=O(a^mm)$。注意$a=b^k$意味着$\log_ba=k$且$m=O(logn)$。所以有
$$T(n)=O(n^klogn)$$
**情况3：** $a<b^k$
在此情况下，几何级数的因子大于1。为了计算几何级数和和，可以使用标准表达式。用$F$($F$是一个常数)来表示$\frac{b^k}{a}$。由于级数的第一个元素是$a^m$，我们有
$$T(n)=a^m\frac{F^{m+1} - 1}{F-1}=O(a^mF^m)=O((b^k)^m)=O((b^m)^k)=O(n^k)$$。
以上三种情况可以总结在下面定理。
**定理：**
$a$和$b$为整数常数，$a\le1, b \le2$且$c$和$k$是正常数，则递推关系$T(n)=aT(\frac{n}{b}) + cn^{k}$的解是

$$f(n)=\begin{cases}O(n^{\log_ba}),&\text{if}& a>b^k\\\\O(n^{k}\log_ba),&\text{if}&a=b^k\\\\O(n^{k}) &\text{if} &a<b^k \end{cases}$$




> Written with [StackEdit](https://stackedit.io/).
