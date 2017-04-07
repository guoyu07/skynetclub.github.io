---
layout: default
---

<style>
.jipai-clearfix:after{content:"";display:table}
.jipai-clearfix:after{clear:both}
.jipai-clearfix{zoom:1}

ul{list-style:none}

.box{border:1px solid #f2f2f2;padding:10px;margin-bottom:20px;}

table td{text-align:center;border:1px solid #f2f2f2;width:25%;height:80px;}

</style>
<div class="home">

  <div class="box">
  <h1 class="page-heading">skynet非官方网站<iframe src="http://ghbtns.com/github-btn.html?user=skynetclub&repo=skynetclub.github.io&type=star&count=false&size=none" frameborder="0" scrolling="0" width="53px" height="20px"></iframe></h1>
  <p>skynet是云风编写的服务端底层管理框架，底层由C编写，配套lua作为脚本使用，可换python等其他脚本语言。skynet主要工作是管理注册服务，并开启多线程协调服务之间的调用和通讯。</p>
  </div>
  
  <div class="box">
  <h3><a href="/skynet/resource.html" title="skynet资源收集">[持续更新，最后更新20170407] skynet资源收集</a></h3>
  <h3><a href="http://blog.codingnow.com/2016/07/skynet_released.html" title="Skynet 1.0.0 正式发布" target="_blank">[2016.07.11] Skynet 1.0.0 正式发布</a></h3>
  </div>

  <table style="width:100%;">
  <tr>
  <td><a href="http://pan.baidu.com/s/1i3qp7b3" title="skynet 介绍PPT" target="_blank">skynet 介绍PPT</a></td>
  <td><a href="http://blog.codingnow.com/2012/09/the_design_of_skynet.html" title="skynet设计概述" target="_blank">skynet设计概述</a></td>
  <td><a href="https://github.com/cloudwu/skynet/wiki" title="skynet wiki" target="_blank">skynet wiki</a></td>
  <td><a href="/book" title="《skynet入门实践》" target="_blank">《skynet入门实践》</a></td>
  </tr>
  <tr>
  <td><a href="https://github.com/cloudwu/skynet" title="skynet源码仓库" target="_blank">skynet源码仓库</a><br /><iframe src="http://ghbtns.com/github-btn.html?user=cloudwu&repo=skynet&type=star&count=false&size=none" frameborder="0" scrolling="0" width="53px" height="20px"></iframe></td>
  <td><a href="http://blog.codingnow.com/eo/skynet/" title="云风博客skynet专栏" target="_blank">云风博客skynet专栏</a></td>
  <td><a href="http://cloudwu.github.io/lua53doc/" title="lua 5.3 手册" target="_blank">lua 5.3 手册</a></td>
  <td>skynet qq群:340504014</td>
  </tr>
  <tr><td colspan="4"><a href="/donate.html" target="_blank">捐赠|donate</a></td></tr>
  </table>
  
  <h2 class="page-heading">社区精华 | <a href="/topics" title="更多">more</a></h2>
  {% if site.tags['skynet'] %}
      <ul class="post-list">
        {% for post in site.tags['skynet'] %}
          <li>
            <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
            <h3>
              <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
            </h3>
          </li>
        {% endfor %}
      </ul>
  {% else %}
      <p>There are no posts for this tag.</p>
  {% endif %}
  
 
    
  <p class="rss-subscribe"><br/>subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>

</div>
