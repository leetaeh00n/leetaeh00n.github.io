---
title: "MyBlog"
layout: archive
permalink: categories/Blog
author_profile: true
sidebar_main: true
---

<!-- 여기 site.categories.카테고리이름 -->

{% assign posts = site.categories.python %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}