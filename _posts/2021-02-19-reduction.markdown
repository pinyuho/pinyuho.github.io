---
layout: post
title: <演算法筆記> Reduction介紹 + 範例
image: 6.jpg
date: 2021-02-19 20:57:21 +0800
tags: [algorithm]
categories: notes
---
### Reduction 是甚麼：
Reduction就是一個把問題從A轉換到B的過程，要轉換得巧妙就必須完全了解A、B兩個問題各自的性質和input, output，並確實把所有的input, output都從A match到B才算完成。

### 轉換範例(A reduced to B)：
#### 1.
- Problem A : Bipartite 
- Problem B: Network Flow   

 in&out\  Problem | input | output | *note 
------------ | ------------- | ------------ | ------------- 
A | G(V, E, U) | maximum matching | V+U = E (V和U交集成為E，且E的edges可以把V和U連起來) |
B | G(V, E) + 點s + 點t | maximize the flow | G(V, E) = 有向圖, s的in-degree = 0, t的out-degree = 0, 且滿足 1. **[流量限制]** 每個邊的flow <= capacity 2. **[進出相等]** 每個點的flow: 流入=流出

- match A, B: 把A圖左邊加一個點s，並連到原本bipartite graph的V的所有點；右邊加一個點t，並連到原本bipartite graph的U的所有點，完成！新的圖的所有點就是Network flow的Vertex, 新的圖的所有邊就是Network flow的Edge(記得邊上的capacity = 1)。所以只要找到A的最大配對，就會找到B的最大流量，反之亦然。

#### 2.
- Problem A : Network flow
- Problem B : Linear Programming (線性規劃) 

*tips: 把線性規劃的一堆不等式和限制對應到Network圖的點跟邊

LP | LP's Definition | 對應到Network的 G(V, E)
------------ | ------------- | ------------
Objective function (目標函數) | 目標要找到函數: `a．x` 的最大or最小值 (`a．x` 為常數向量a 和充滿未知數的向量x 的內積) | 所有從左排的點流出的flow value總和(中間每個edge的flow value表示為x_i, i是n個邊的集合)
Inequality constraints (不等式的限制) | - | **[流量限制]** `x_i <= c_i`  (1<= i <= n)
Equality constraints (等式的限制) |  `e1．x1 = d1` , `e2．x1 = d2`, ....`e_m．x1 = d_m` | **[進出相等]** 每個流入v的值都等於流出v的值, v是network graph 中所有除了s, t的點 : 每個 `x_i(i邊流出v點) - x_j(j邊流入v點) = 0`, v是Network的所有點V除去{s點, t點}的集合