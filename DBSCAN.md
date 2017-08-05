# DBSCAN

> Density-Based Spatial Clustering of Application with Noise.
>
> 其核心思想是用一个 $\epsilon$ 邻域内的邻居点数衡量所在空间的密度，从而找出形状不规则的cluster，且聚类时不需要事先知道 cluster 的个数。

### 前置定义

首先定义两个重要的参数：Eps($\epsilon$) 和 MinPts(minimum number of points required to form a cluster)，即定义核心点的阈值。

定义参见

https://zh.wikipedia.org/wiki/DBSCAN

http://blog.csdn.net/itplus/article/details/10088625

![QQ20170804-125354@2x](../../QQ20170804-125354@2x.png)

通俗的讲，核心点对应稠密区域内部的点，边界点对应稠密区域边缘的点，而噪音点对应稀疏区域中的点。

![QQ20170804-125512@2x](../../QQ20170804-125512@2x.png)

需要注意的是，核心点位于簇内部，它确定属于某个簇；噪音点是数据集中区域的干扰数据，它不属于任何一个簇；而边界点是一类特殊的点，它位于一个或几个簇的边缘地带，可能属于一个簇，也可能属于另外一个簇，其簇归属并不明确。

**直接密度可达**：设 $x,y \in X，若满足 x \in X_c, y \in N_{\epsilon}(x)，则称 \ y \ 是从 \ x \ 直接密度可达的.$

**密度可达**：直接密度可达的传递闭包

> 注：密度可达不具有对称性，因为密度可达的前置点必须是核心点，但后置点不需要，可以使核心点也可以是边界点。

**密度相连**：$设 \ x,y,z \in X，若 \ y \  和 \ z \ 均是从\  x \ 密度可达的，则称 \ y \ 和\  z \ 是密度相连的，显然密度相连具有对称性。$

**类**：$称非空集合 C \subset X 是 X 的一个大类 (cluster)，如果它满足：对于 x,y \in X$

1.  $\ (Maximality) \ 若 \ x \in C，且\ y\ 是从 \ x \ 密度可达的，则 \ y \in  C.$
2. $(Connectivity) \ 若 \ x \in C\ ,y \in C, \ 则 \ x,y \ 是密度相连的.$

### 算法思想

算法核心思想描述如下：从某个选定的核心点出发，不断向密度可达的区域扩张，从而得到一个包含核心点和边界点的最大化区域，区域中任意两点密度相连，应用DBSCAN算法可以将数据集合 X 划分为 K 个 cluster及噪音点集合，为此，引入 cluster 标记数组。

cluster[i]  = - 1表示这个点属于噪音点，cluster[i] = j 表示这个点属于第 j 个cluster.

wiki给出的算法伪代码如下。

```C++
// 它由一个任意未被访问的点开始，然后探索这个点的 ε-邻域，如果 ε-邻域里有足够的点，则建立一个新的聚类，否则这个点被标签为杂音。注意这个点之后可能被发现在其它点的 ε-邻域里，而该 ε-邻域可能有足够的点，届时这个点会被加入该聚类中。
DBSCAN(D, eps, MinPts) {
   C = 0
   for each point P in dataset D {
      if P is visited
         continue next point
      mark P as visited
      NeighborPts = regionQuery(P, eps)
      if sizeof(NeighborPts) < MinPts
         mark P as NOISE
      else {
         C = next cluster
         expandCluster(P, NeighborPts, C, eps, MinPts)
      }
   }
}

expandCluster(P, NeighborPts, C, eps, MinPts) {
   add P to cluster C
   for each point P' in NeighborPts { 
      if P' is not visited {
         mark P' as visited
         NeighborPts' = regionQuery(P', eps)
         if sizeof(NeighborPts') >= MinPts
            NeighborPts = NeighborPts joined with NeighborPts'
      }
      //如果对于枚举出来的点P'的邻域也是可以扩张的，根据密度可达性，那么可以将这个邻域加入到我们需要的枚举的邻域。
      //根据点的访问顺序计算点所属的聚类。
      if P' is not yet member of any cluster
         add P' to cluster C
   }
}

//这个查询返回的是P所有eps邻域中的点。
regionQuery(P, eps)
   return all points within P's eps-neighborhood (including P)
```

![QQ20170804-134749@2x](../../QQ20170804-134749@2x.png)

注意这里算法中每次查询 $N_{\epsilon}(i) $ 的计算，一种做法是引入一个对称的距离矩阵，需要 $O(n^2)$ 的存储空间，对于大规模数据集不可行。

那么我们可以在查询的过程中优化，通过使用k-d tree这样的数据结构，减少判定密度可达对象时的搜索范围。

**$\epsilon$ 的选取**：DBSCAN算法中使用了统一的 $\epsilon$ 值，因此数据空间中所有节点的邻域大小是一致的。当数据密度和 cluster 间距离的分布不均匀时，若选取那些较小的 $\epsilon$ 值，则较稀 cluster 中的节点的密度会小于 M，就会造成这些点被认为成边界点而不被用于所在类的进一步拓展，接下来的过程中，这些较稀的 cluster 可能被划分成多个性质相似的 cluster；

与此相反，若（根据较稀的那些cluster）选取较大的 $\epsilon$ 值，则离得较近而密度较大的那些 cluster 将可能被合并为同一个 cluster，它们之间的差异将因为选取了较大的 $\epsilon$ 而被忽略。

显然在这种情况下，要选取一个合适的参数是比较困难的，对于高维数据，还会存在（curse of dimensionality）,$\epsilon$ 的选取将会变得更加困难。

**参数 M 的选取**：M 的选取将有一个指导性原则，即$M \geq dim + 1$，$dim$ 表示聚类数据空间的维度（即 $X$ 中元素的长度）.



> DBSCAN算法不适用于**数据集中密度差异**很大的情形，因为在这种情形下，参宿的选取比较困难。DBSCAN算法的目的在于过滤低密度区域，发现稠密度样本点。跟传统的基于层次的聚类和划分聚类的凸形聚类簇不同，该算法可以发现任意形状的聚类簇。

### 应用

下面尝试使用 R 语言应用DBSCAN。

```R
> library(fpc)
> set.seed(665544)
> n <- 600
> x <- cbind(runif(10,0,10) + rnorm(n,sd = 0.2), runif(10,0,0) + rnorm(n,sd = 0.2))
> ds <- dbscan(x,0.2)
> quartz(width = 5, height = 8)
> plot(ds,x)
> plot(ds,x)
```

![QQ20170804-142309@2x](../../QQ20170804-142309@2x.png)

### 解决时间序列异常点



