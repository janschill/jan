---
layout: page
title: Experience
---

A complete archive of professional roles and notable open-source work. The shorter version lives on the [about page](/about).

{% assign entries = site.experiences | sort: "order" %}

<ul>
{% for e in entries %}
<li>
  <strong>{{ e.role }}</strong> at <a href="{{ e.website }}">{{ e.company }}</a>
  ({{ e.start | slice: 0, 4 }}–{% if e.end %}{{ e.end | slice: 0, 4 }}{% else %}present{% endif %}{% if e.location %}, {{ e.location }}{% endif %})
  <div>{{ e.content | markdownify }}</div>
  {% if e.links.size > 0 %}
  <small>
  {% for link in e.links %}<a href="{{ link.url }}">{{ link.label }}</a>{% unless forloop.last %} · {% endunless %}{% endfor %}
  </small>
  {% endif %}
</li>
{% endfor %}
</ul>
