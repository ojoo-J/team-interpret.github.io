---
layout: default
title: Curriculum
description: Curriculum for Interpretable AI made by SAIL Members
permalink: /curriculum/
nav: true
---


<div class="post">
  <div class="header-bar" style='padding-bottom:0px;'>
    <h1 style='font-family:"Fira Sans"'>{{ page.title }}</h1>
    <h2 style='font-family:"Fira Sans"'>{{ page.description }}</h2>
    <hr>
    <p style='font-family:"Fira Sans";text-align:left;'> 
      The full list of curriculum can be found in  <a href="https://team-interpret.github.io/curriculum/curriculum/"> 📚 </a>  <br>
      This work is licensed under a Creative Commons International License. 
        <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">
      <img alt="Creative Commons License" style="border-width:0;text-align:center;" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a>
    </p> 
  </div>
  <ul class="post-list">
    {%- assign sorted_pages = site.curriculum | sort: "date" %}
    {%- assign sorted_pages = sorted_pages | reverse -%} 

    {% for post in sorted_pages %}

    {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
    {% assign year = post.date | date: "%Y" %}
    {% assign tags = post.tags | join: "" %}
    {% assign categories = post.categories | join: "" %}

    <div class="column-first" style="border-bottom:#e4e4e4 solid;  " >
      <h2>
        {% if post.redirect == blank %}
          <a class="post-title" href="{{ post.url | prepend: site.baseurl }}" style="text-decoration:none; font-size: 20px; color:#362f5b ">{{ post.title }}</a>
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