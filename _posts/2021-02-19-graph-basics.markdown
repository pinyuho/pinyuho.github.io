---
layout: post
title: <演算法筆記> 基本graph名詞介紹 + DFS, BFS筆記
image: 6.jpg
date: 2021-02-19 20:42:46 +0800
tags: [algorithm, graph]
categories: notes
---
## Graph名詞介紹：
G(V, E)：Graph
V：Vertex (節點)
E：Edge (邊)

- Induced subgraph：做法通常是將原來Graph的vertex刪除，並順便刪除該vertex與Graph相連的edge。

> an induced subgraph of a graph is another graph, formed from a subset of the **vertices** of the graph and **all of the edges connecting pairs of vertices** in that subset. ([Wikipedia](https://en.wikipedia.org/wiki/Induced_subgraph#:~:text=3%20Computation-,Definition,have%20both%20endpoints%20in%20S.))

>  an induced subgraph can be constructed by **deleting vertices** (and with them all the incident edges), but no more edges. **If additional edges are deleted, the subgraph is not induced.**

- Spanning tree：是原Graph的subgraph，包含所有原Graph的vertex，並包含連接這些vertex最少的邊。

> a spanning tree T of an undirected graph G is a subgraph that is a tree which **includes all of the vertices of G**, with a **minimum possible number of edges**.

## DFS (Depth-First Search 深度搜尋)
DFS的概念就是一旦看到可以走的點，就先走到不能再走為止。
先記下課堂上的pseudocode：
{% highlight js %}
Algorithm DFS(G, v)：
begin
     mark v;
     perform preWORK on v; 
     //在此時要決定traverse vertex的順序，也就是決定下一個edge(v, w)
     //使用array紀錄每個vertex被走過的時間
     
     for all edges(v, w) do 
          if w is unmarked then
               DFS(G, w)
          perform postWORK for (v, w)
end
{% endhighlight %}

## BFS (Breadth-First Search 廣度搜尋)
pseudocode：
{% highlight js %}
Algorithm BFS(G, v)：
begin
     mark v;
     put v in a queue; 
     //注意：queue是first-come-first-serve，並存放BFS search的所有備選點，當走過了就移出queue
     while the queue is not empty do
          remove vertex w from the queue;
          //把最先traverse到的w點移出queue，並將所有連到w(且還沒走過)的點加入queue
          perform preWORK on w;
          for all edges(w, x) with x unmarked do
               mark x;
               add (w, x) to the BFS tree T;
               put x in the queue
end
           
{% endhighlight %}