---
layout: post
title: <演算法筆記> Eulerian Circuit是什麼? 如何確認Graph中有Eulerian Circuit? 
date: 2021-02-19 19:09:53 +0800
image: 6.jpg
tags: [algorithm, graph]
categories: notes
---
Euler這個名字看起來十分熟悉，沒錯，就是發明尤拉數(e)的那個[Leonhard Euler](https://zh.wikipedia.org/wiki/%E8%90%8A%E6%98%82%E5%93%88%E5%BE%B7%C2%B7%E6%AD%90%E6%8B%89)，他在數學領域有許多卓越的成就，也是微積分課本中會出現的那個神人><。不過今天要討論的並不是微積分，而是在演算法中時常被使用的Graph，以及如何確認圖中有Eulerian Circuit。

***

## Eulerian Circuit (以下簡稱EC)：
其實簡單來說就是以前常玩的"一筆畫遊戲"，在遊戲中要找到一個路徑，可以一次通過圖上所有的點和邊，就是EC的概念：找到一個Graph上的path，讓全部Graph走完時，所有的邊只走過一次。
EC更精確的定義：
* Undirected Graph(無向圖)：有EC <=> 所有vertex的degree數量都是偶數。

* Directed Graph(有向圖)：有EC <=> 所有vertex的 in degree數量 = out degree數量。

* (比較)Hamilton Circuit：全部的點只走過一次。

## Eulerian Circuit pseudocode 實作：
> Describe an efficient implementation of the algorithm discussed in class for finding an Eulerian circuit in a graph. The algorithm should run in linear time and space.  (Hint: try to interweave the discovery of a cycle and that of the separate Eulerian circuits in the connected components with the cycle removed in the induction step.)


* Idea：將Eulerian circuit 看成cycles 的組合。
隨意走過一個大的Cycle，並在大Cycle的每一個點中都找找看有沒有小Cycle，如果有，則把小Cycle塞進大Cycle走的順序中。因此我們需要兩個Funtion：紀錄主要演算法架構的 _**FindEC()**_，以及 _FindEC()_ 中會需要呼叫的 _**TraceCycle()**_。

_**FindEC()**_
{% highlight js %}
Algorithm FindEC(G=(V,E))：//此Function會return找到的EC
     //當degree為奇數(一個點連到奇數個邊)，代表EC不存在
     if any degree(v) = odd or 0 then
          return; //return nothing(表示Path是空的)

     //初始化前置作業
     initialize Path
     initialize a list L //記錄下每個在大Cycle中的點，以便可以使用TraceCycle()找到更小的Cycle
     pick one vertex v

     //確認過EC存在才能使用TraceCycle()，不然TraceCycle()的while loop永遠跑不完
     TraceCycle(v, Path, G, L) //大Cycle

     while L is not empty do
          pick w from L
          if w.degree > 0 then //表示從w開始還有路
               initialize subPath    
               TraceCycle(w, subPath, G, L)
               insert subPath into Path
          else
               pop w //pop w fom list L
     return Path;
{% endhighlight %}

_**TraceCycle()：O(|E|)**_
(TraceCycle不用回傳值，而是直接改變傳進來的Path值)
{% highlight js %}
Function TraceCycle(v, Path, G, L)：//回傳subPath(小Cycle)
     nowPosition := v
     repeat
          pick an edge(nowPosition, w)
          L.append(w)
          put edge(nowPosition, w) in Path
          remove edge(nowPosition, w) from G
          nowPosition := w
     until nowPosition == v
{% endhighlight %}