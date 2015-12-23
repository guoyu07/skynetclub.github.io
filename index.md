---
layout: default
---

<div class="home">

  <div style="border:1px solid #f2f2f2;padding:10px;margin-bottom:20px;">
  <h1 class="page-heading">非官方skynet社区。<iframe src="http://ghbtns.com/github-btn.html?user=skynetclub&repo=skynetclub.github.io&type=star&count=false&size=none" frameborder="0" scrolling="0" width="53px" height="20px"></iframe></h1>
  <p>skynet是云风编写的服务端底层管理框架，底层由C编写，配套lua作为脚本使用，可换python等其他脚本语言。skynet主要工作是管理注册服务，并开启多线程协调服务之间的调用和通讯。</p>
  </div>
  
  <h2 class="page-heading">社区精华 | <a href="/topics.html" title="更多">more</a></h2>
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
