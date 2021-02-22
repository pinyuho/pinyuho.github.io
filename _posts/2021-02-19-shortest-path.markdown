---
layout: post
title: <演算法筆記> 如何找最短路徑? Dijkstra's algorithm的時間複雜度討論 + min heap介紹
image: 6.jpg
date: 2021-02-19 20:50:27 +0800
tags: [algorithm, graph, data-structure]
categories: notes
---
## Problem: Finding shortest path 
### Given:
- directed graph G = (V, E) 
- 一個指定的vertex v (作為起點)

### Goal: 
- 找到所有從v點到圖上任意點的最小路徑。("最小"指的並不是邊數最少，而是所有經過的邊加起來的weight sum最小)

### Solution1 `Acyclic case` :
(為簡化討論，限定此處為Acyclic graph，也就是圖中不會有cycle) 
- 想法：先做一次**拓樸排序**(traverse by DFS，把每一個traverse到並且in degree為0的vertex放入Queue作為候選點(in degree為0表示該點上面的點都處理完了)，並且依據traverse到的順序標記)，接著
1. 使用induction的概念，base case為: 比較現在的點w，如果 _w.SP+len(w, z) < z.SP_，則更新SP(shortest path)的值。
2. 使用資料結構Queue，將traverse到並且in degree = 0的點放入Queue，接著做與1.相同的比較：如果 _w.SP+len(w, z) < z.SP_，則更新SP(shortest path)的值。

- Pseudocode 1
{% highlight js %}
Algorithm Acyclic_Shortest_Paths(G; v; n);
{Initially, w:SP = infinity, for every node w.}
{A topological sort has been performed on G}
begin
     let z be the vertex labeled n;
     if z != v then
          Acyclic Shortest Paths(G - z; v; n - 1);
          for all w such that (w, z) belongs to E do //確保Edge(w, z)在Graph中
               if w.SP + length(w, z) < z.SP then
                    z:SP := w:SP + length(w; z) //更新z.SP
     else v.SP := 0 //起點
end
{% endhighlight %}

- Pseudocode 2 (Queue)
{% highlight js %}
Algorithm Imp_Acyclic_Shortest_Paths(G; v);
     for all vertices w do w:SP := infinity;
     initialize v.indegree for all vertices;
     for i := 1 to n do
          if v_i.indegree = 0 then put v_i in Queue; 
     v.SP := 0;
     repeat
          remove vertex w from Queue;
          for all edges (w, z) do
               if w.SP + length(w, z) < z.SP then
                    z.SP := w.SP + length(w, z); //更新z.SP
               z.indegree := z.indegree - 1; //edge(w, z)已被走過
               if z.indegree = 0 than put z in Queue //z放進Queue中，下一輪先處理
     until Queue is empty
{% endhighlight %}

### Solution2 `General case` -> Dijkstra's Algorithm:
- 在Graph中，從v點出發，往所有連到v的vertex traverse，每次在挑選新的點時，要找目前path的weight sum最小的(weight要從v點，也就是起點開始加)，每次找到最小的點就連起來，直到所有點都被連完為止(不一定所有的邊都要連起來)。

- Pseudocode
{% highlight js %}
Algorithm Single Source Shortest Paths(G; v);
// Dijkstra's algorithm
begin
     for all vertices w do
          w.mark := false;
          w.SP := infinity;
     v.SP := 0 //起點

     //在接下來還沒走到的鄰近點中，找weight最小的
     //heap implementation: O(|V|*log|V|)
     while there exists an unmarked vertex do
          let w be an unmarked vertex s.t. w:SP is minimal; 

          w.mark := true;

          //O(|E|*log|V|) =>當找到更小的SP，在heap中要decrease(x, k)
          for all edges (w, z) such that z is unmarked do  
               if w.SP + length(w, z) < z.SP then
                    z.SP := w.SP + length(w, z)
end
{% endhighlight %}

## Min Heap 介紹
在上述的Dijkstra Algorithm中，可以發現，許多要找min的比較，當使用資料結構Heap來implement時，時間複雜度都會從原本的O(n)大幅減少到O(logn)，這是因為min heap是一個上面的node最小的binary tree，而binary tree的高度最多就是logn，因此在tree中找資料，需要做的比較次數就是tree的高度~

而在實作部分，min heap需要的操作為以下這幾項：
1. Insert(k): 插入一個新資料k
2. MIN: 找到min值(也就是Root node值)
3. Extract-MIN: 刪除min值並維持min heap的正確性(將Root和最後一個leaf交換，size--，再做MIN-heapify即可)
4. Decrease(x, k): 把x降為k，並做必要調整，讓min heap保持正確
5. Delete(x): 刪除x pointer指到的node =>Decrease(x, -inf)
6. Union(H1, H2): 把兩個min heap(H1, H2)合併成一個min heap

而這些操作所需要花的時間分別為：
1. O(1)
2. O(1)
3. O(logn)
4. O(logn)
5. O(logn)
6. O(n)