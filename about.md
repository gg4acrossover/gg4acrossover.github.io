---
layout: page
permalink: /about/index.html
title: GG4aCrossover
tags: [cross, gg4a]
chart: true
---

{% assign total_words = 0 %}
{% assign total_readtime = 0 %}
{% assign featuredcount = 0 %}
{% assign statuscount = 0 %}

{% for post in site.posts %}
    {% assign post_words = post.content | strip_html | number_of_words %}
    {% assign readtime = post_words | append: '.0' | divided_by:200 %}
    {% assign total_words = total_words | plus: post_words %}
    {% assign total_readtime = total_readtime | plus: readtime %}
    {% if post.featured %}
    {% assign featuredcount = featuredcount | plus: 1 %}
    {% endif %}
{% endfor %}

Tôi là **developer nhân dân** :D, chuyên môn của tôi là lập trình IOS, lập trình game sử dụng cocos2dx. Tôi có hứng thú tìm tòi thêm một chút về javascript nữa. Ngoài lập trình tôi chơi bóng rổ và khoái bi.a, thích đọc sách. Hãy liên lạc với tôi nếu bạn có chung sở thích :D.

Blog hiện có {{ site.posts | size }} posts trong {{ site.categories | size }} mục với tổng cộng {{ total_words }} từ, bạn sẽ mất khoảng ({{ site.wpm }} WPM) <span class="time">{{ total_readtime }}</span> phút để đọc toàn bộ :D. {% if featuredcount != 0 %}There are <a href="{{ site.url }}/featured">{{ featuredcount }} featured posts</a>, you should definitely check those out.{% endif %} Bài post mới nhất {% for post in site.posts limit:1 %}{% if post.description %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}">"{{ post.title }}"</a>{% else %}<a href="{{ site.url }}{{ post.url }}" title="{{ post.description }}" title="Read more about {{ post.title }}">"{{ post.title }}"</a>{% endif %}{% endfor %} được đăng lên vào {% for post in site.posts limit:1 %}{% assign modifiedtime = post.modified | date: "%Y%m%d" %}{% assign posttime = post.date | date: "%Y%m%d" %}<time datetime="{{ post.date | date_to_xmlschema }}" class="post-time">{{ post.date | date: "%d %b %Y" }}</time>{% if post.modified %}{% if modifiedtime != posttime %} và lần edit gần nhất vào <time datetime="{{ post.modified | date: "%Y-%m-%d" }}" itemprop="dateModified">{{ post.modified | date: "%d %b %Y" }}</time>{% endif %}{% endif %}{% endfor %}. Lần commit cuối cùng vào {{ site.time | date: "%A, %d %b %Y" }} at {{ site.time | date: "%I:%M %p" }} [UTC](http://en.wikipedia.org/wiki/Coordinated_Universal_Time "Temps Universel Coordonné").

