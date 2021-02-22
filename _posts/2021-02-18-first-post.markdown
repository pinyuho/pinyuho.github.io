---
layout: post
title: 第一篇文章! 架設Github pages紀錄
date: 2021-02-18 15:12:18 +0800
image: 12.jpg
tags: [life]
categories: others
---
大二上的寒假一時興起想來經營自己的網站，因此這篇文就來記錄一下架網站用到的資源吧！

1. 模板來源：[Flexton][flexton]
2. [Github Pages教學][github-pages-tutorial]
3. 免費網域申請：[Freenom][freenom]
4. [綁定網域教學][domain-setup-tutorial]
5. Jekyll本地預覽：首先要先安裝[Ruby][ruby-installer]，再進入terminal (我是直接用電腦的命令提示字元cmd) 輸入相應的指令，可以參考[這個網站][ruby-and-jekyll-tutorial]，安裝完以後，之後只要進入terminal，輸入

{% highlight js %}
  jekyll serve
  //備註：發生錯誤時使用指令"bundler exec jekyll s"
  //輸入時注意目前所在的資料夾位置，利用 "cd.."(退出一層資料夾)和"cd Users\user\Desktop\pinyuho.github.io"(進入某些資料夾)
{% endhighlight %}
再到網址頁輸入`http://localhost:4000/`就可以看到預覽畫面啦！



[flexton]: http://jekyllthemes.org/themes/flexton/
[github-pages-tutorial]:   https://medium.com/@NorthBei/%E4%B8%8D%E7%94%A8%E6%87%82git%E4%B9%9F%E8%83%BD%E7%94%A8github-pages%E6%9E%B6%E8%A8%AD%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99%E4%B8%A6%E7%B6%81%E5%AE%9A%E7%B6%B2%E5%9F%9F-c60c02bc470c
[domain-setup-tutorial]: https://medium.com/@moojing/%E5%80%8B%E4%BA%BA%E6%8A%80%E8%A1%93%E7%AB%99%E4%B8%80%E6%8A%8A%E7%BD%A9-%E9%83%A8%E8%90%BD%E6%A0%BC%E5%BB%BA%E7%BD%AE%E5%A4%A7%E5%85%A8-%E4%BA%8C-%E5%B0%87-github-page-%E4%B8%B2%E4%B8%8A%E8%87%AA%E5%B7%B1%E7%9A%84%E5%9F%9F%E5%90%8D-8f7e11cf2687
[freenom]: https://www.freenom.com/zu/index.html?lang=zu
[ruby-installer]: https://rubyinstaller.org/downloads/
[ruby-and-jekyll-tutorial]: https://wcc723.github.io/jekyll/2014/01/13/windows-jekyll-server/