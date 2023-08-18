---
layout: default
title: TS
description: TimeSeries
permalink: /ts/
nav: true
---

  <style>
    h2 {
      font-weight: 500
    }
    .column-second {
      margin-bottom: 10px
    }
  </style>

<div class="post" style='padding-bottom:450px'>
  <div class="header-bar" style='padding-bottom:0px;'>
    <h1 style='font-family:"Fira Sans"'>{{ page.title }}</h1>
    <h2 style='font-family:"Fira Sans"'>{{ page.description }}</h2>
  </div>

  <ul class="post-list">
    {%- assign sorted_pages = site.ts | sort: "date" %}
    {%- assign sorted_pages = sorted_pages | reverse -%} 

    {% for post in sorted_pages %}

    {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
    {% assign year = post.date | date: "%Y" %}
    {% assign tags = post.tags | join: "" %}
    {% assign categories = post.categories | join: "" %}

    <div class="column-first" style="border-bottom:#e4e4e4 solid;  " >
      <h2>
        {% if post.redirect == blank %}
          <a class="post-title" href="{{ post.url | prepend: site.baseurl }}" style="text-decoration:none; font-size: 20px; ">{{ post.title }}</a>
        {% else %}
        <a class="post-title" href="{% if post.redirect contains '://' %}{{ post.redirect }}{% else %}{{ post.redirect | relative_url }}{% endif %}">{{ post.title }}</a>
        {% endif %}
      </h2>
      <p style="line-height: 140%;">{{ post.description }}</p>
      <p class="post-meta"> {{read_time}} min read &nbsp; &middot; &nbsp;
        {{ post.date | date: '%B %-d, %Y' }}    
        <p class="post-tags">
          <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}">
            <i class="fas fa-calendar fa-sm"></i> {{ year }} </a>

            {% if tags != "" %}
            &nbsp; &middot; &nbsp;
              {% for tag in post.tags %}
              
                <i class="fas fa-hashtag fa-sm"></i> {{ tag }} &nbsp;
                {% endfor %}
            {% endif %}

            {% if categories != "" %}
            &nbsp; &middot; &nbsp;
              {% for category in post.categories %}
              <a href="{{ category | prepend: '/blog/category/' | prepend: site.baseurl}}">
                <i class="fas fa-tag fa-sm"></i> {{ category }}</a> &nbsp;
                {% endfor %}
            {% endif %}
        </p>
      </p>
    </div>
      <div class="column-second">
        {% if post.img %}
            <img src="{{ post.img }}" width="170px" height="135px" style="margin-top:10px;margin-left:20px;border-radius: 20px;border:solid 2px;"> 
        {% endif %}
      </div>
    {% endfor %}
    
  </ul>
  {% include pagination.html %}

</div>