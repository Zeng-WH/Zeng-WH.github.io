---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* Ph.D in Computer Science, Hong Kong University of Science and Technology, 2025 - Present
  * Advisor: Prof. Junxian He

Awards
======
* National Scholarship in China (2019, 2023)
* 1st Award, SereTOD Challenge 2022 Track 2, EMNLP 2022
* 4th Award, SMP 2021 Conversational AI Challenge
* 8th Place, CCIR 2021 Intelligent NLU Challenge

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
