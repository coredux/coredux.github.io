---
title: 朝花夕拾:0/1背包和完全背包是如何从二维数组压缩到一维的
layout: post
---

# 如今的前言

自己要开始找工作了，决定把从前的笔记迁移到此处来，也算是一个复习和回顾吧，看着这些几年前的文字，也回忆起了当时写代码的样子，也是挺有意思的。

以前是用博客园的富文本编辑器写的，现在重新用__markdown__ 和__latex__ 公式重新排版了一下，希望看起来更舒服一些。

# 当时的前言

从刚开始接触背包到现在，已经快两个月了，学完背包之后就又继续学其它东西，现在该回头再来复习一下，整理整理自己的思路，使自己对背包更熟练一点，这里自己先复习一下0/1背包和完全背包。

# 0/1背包

0/1背包的主要思路就是:这件物品，取还是不取。用一个二维数组$\mathrm{dp}_{i,v}$ 来表示对第$i$ 个物品，背包容量为$v$ 时的情况。$\mathrm{c}_i$ 表示第$i$件物品的体积，$\mathrm{w}_i$ 表示第$i$ 件物品的价值。那么考虑第$i$ 件物品取与不取:如果不取，那么就可以转化为$i-1$ 件物品、容量仍然为$v$ 、价值没有增加的情况$ \mathrm{dp}_{i-1,v} $ ；如果取，那么转化为$i-1$ 件物品、容量减去第$i$ 件物品的体积后剩余容量、价值加上第$i$ 件物品的价值后的情况$\mathrm{dp}_{i-1,v-\mathrm{c}_i}+\mathrm{w}_i$ 。为了让背包中物品价值最大，我们取二者较大者，也就是

$$\mathrm{dp}_{i,v} = \mathrm{max} \lbrace \mathrm{dp}_{i-1,v} , \mathrm{dp}_{i-1,v-\mathrm{c}_i}+\mathrm{w}_i \rbrace$$

好了，现在思考如何将数组压缩，对于这两种情况下$\mathrm{dp}_{i,v}$ 值的改变，要么是$\mathrm{dp}_{i,v} = \mathrm{dp}_{i-1,v}$ ,要么是$\mathrm{dp}_{i,v} = \mathrm{dp}_{i-1,v-\mathrm{c}_i}+\mathrm{w}_i$

假设下面是就是二维数组dp的一部分,

　$a, b, \mathrm{dp}_{i-1,v},d,e$
　$f,g,\mathrm{dp}_{i,v}\,\,\,\,\,,h,k$

我们可以发现：如果$\mathrm{dp}_{i,v}=\mathrm{dp}_{i-1,v}$ ,那么相当于直接复制$\mathrm{dp}_{i,v}$ 上面的元素$\mathrm{dp}_{i-1,v}$ 的值。而如果$\mathrm{dp}_{i,v}=\mathrm{dp}_{i-1,v-\mathrm{c}_i}+\mathrm{w}_i$ ，注意到，$v-\mathrm{c}_i \leq v $ ,所以, $\mathrm{dp}_{i,v}$ 的值是由上面一行对应位置以及左边的某一个元素加上$\mathrm{w}_i$ 得到，也就是说，我们每次想要更新$\mathrm{dp}_{i,v}$ 会用到的值只有上面一行对应位置以及左边的部分。
　　
所以，我们就能把二维数组压缩为一维数组，只需要每次从后往前更新$\mathrm{dp}_{i,v}$的值。这样就用$\mathrm{dp}_v$来表示容量为$v$的情况下，背包内物品的价值，状态转移方程也就成了：

$$\mathrm{dp}_v = \mathrm{max}\lbrace \mathrm{dp}_v,\mathrm{dp}_{v-\mathrm{c}_i} + \mathrm{w}_i \rbrace$$

# 完全背包

对于完全背包，一件物品可以取多次，我们仍然使用0/1背包的思想:这件物品，取还是不取。唯一的变化是，取了这件物品，还可以取。所以，如果取，仍然是i件物品的问题($\mathrm{dp}_{i,v-\mathrm{c}_i}+\mathrm{w}_i $ );如果不取, $\mathrm{dp}_{i,v}$ 还是$\mathrm{dp}_{i-1,v}$ 都一样(第一次不取，以后也不会取，相当于转化成$i-1$ 件物品的问题，为了和取的情况保持一致，采用$\mathrm{dp}_{i,v}$ 。所以状态转移方程变为了
　　
　$$\mathrm{dp}_{i,v} = \mathrm{max}\lbrace \mathrm{dp}_{i,v},\mathrm{dp}_{i,v-\mathrm{c}_i} + \mathrm{w}_i \rbrace$$
　
那么在数组里，

$a,b,m,\,\,\,\,\,\,d,e$
$f,g,\mathrm{dp}_{i,v},h,k$

同样注意到，$v-\mathrm{c}_{i} \leq v$ 所以，每次更新 $\mathrm{dp}_{i,v}$ 可能用到的值是$\mathrm{dp}_{i,v}$ 以及它的左边的部分，所以也可以压缩为一维，这里要注意了，数组中的$f$、$g$，是$i$ 下的情况，并不是$i-1$ 的情况，$\mathrm{dp}_{i,v}$ 的值取决于$i$下，而不是$i-1$, 所以此时应该从前往后更新dp的值，这样才能保证取第i件物品时，$\mathrm{dp}_{i,v}$ 是由$\mathrm{dp}_{i,v-\mathrm{c}_i} + \mathrm{w}_i$ dp[i]推得，所以尽管状态转移方程仍然为
　　
　　$$\mathrm{dp}_{i,v} = \mathrm{max}\lbrace \mathrm{dp}_{i,v},\mathrm{dp}_{i,v-\mathrm{c}_i} + \mathrm{w}_i \rbrace$$
　　
$v$的循环顺序却应该是从小到大。

基本上，这就是0/1背包和完全背包从二维转化为一维的思路，以后自己还要经常复习。

0/1背包的伪代码：

	for i=1 to N
	    for v=V to 0
	        f[v]=max{dp[v],dp[v-c[i]]+w[i]}
	       
	       
完全背包的伪代码：

	for i=1 to N
	    for v=0 to V
	        f[v]=max{dp[v],dp[v-c[i]]+w[i]}



	