---
layout: page
title: Archive
subtitle: Blog Posts
---

<div>
  <ul class="posts">
    {% for post in site.posts %}
      {% if post.title != null %}
        <li itemscope>
          <span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></span> &raquo; <a href="{{ post.url  | prepend: site.baseurl }}"><h3>{{ post.title }}</h3></a>
        </li>
      {% endif %}
    {% endfor %}
  </ul>
</div>