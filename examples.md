---
layout: page
title:  "skynet examples"
date:   2015-08-06 16:03:19
categories: skynet
tags: [skynet]
---

<div class="home">

  <h1 class="page-heading">skynet例子</h1>


      <ul class="post-list">
        {% for link in site.data.examples %}
          <li>
            <h3>
              <a class="post-link" href="{{ link.url }}" target="_blank">{{ link.name }}</a>
            </h3>
            <span class="post-meta">{{ link.desc }}</span>
          </li>
        {% endfor %}
      </ul>
        
  
  <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>

</div>