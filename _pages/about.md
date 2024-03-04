---
permalink: /
title: "Welcome to my Personal Blog"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

Welcome to my blog! I am currently an Application Engineer for Simuserv serving the Australian/New Zealand region focusing on engineering simulations using finite element analysis (FEA). 

Prior to this, I earned my engineering degree at the University of Auckland in Engineering Science (2016-2019) and then completed a Master's of Engineering at the University of Sydney (2022-2023).

In my spare time I sometimes like to play around with deep learning in particular mixing it with simulation, so called physics-informed neural networks. I also like to teach so please see below on some guides

## FEA
{% for post in site.posts %}
  {% if post.tags[0] == 'FEA' %}
<h3>
  <a href="{{ post.url }}">
  {{post.title}}  
  </a>
</h3>
  {% endif %}
{% endfor %}

## Physics Informed Machine Learning (PINN)
{% for post in site.posts %}
  {% if post.tags[0] == "ML" %}
<h3>
  <a href="{{ post.url }}">
  {{post.title}}  
  </a>
</h3>
  {%endif%}
{% endfor %}