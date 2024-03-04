---
layout : archive
permalink: /projects/
title: "Projects"
author_profile: true
redirect_from: 
  - /project/
  - /projects.html
---
<!-- Please see a curated lists of the projects/topics below

# Thesis Adapters for Low Precision Neural Networks
You can read my thesis [here](/files/JohnSuThesis_Adapters.pdf)

# Physics Informed Neural Networks
You can read my summer research report [here](/files/Summer_Research_Report_compressed.pdf)
 -->
Please see a curated lists of the projects/topics below.

{% for post in site.posts %}
  <h2>
    <a href="{{ post.url }}">
    {{post.title}}  
    </a>
  </h2>
  <p>{{ post.excerpt }}</p>
{% endfor %}