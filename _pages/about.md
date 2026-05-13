---
layout: page
title: About Jan Schill
---

Hi, I'm Jan, a software engineer based in Oslo. I build payment infrastructure at Vipps MobilePay, write here about engineering and life, and ship side projects when I want to scratch an itch.

### Now

{% assign current = site.experiences | where: "featured", true | where_exp: "e", "e.end == nil" | sort: "order" %}
{% for e in current %}
**{{ e.role }} at [{{ e.company }}]({{ e.website }})** ({{ e.start | slice: 0, 4 }}–present)<br>
{{ e.content | markdownify }}
{% if e.links.size > 0 %}<small>{% for link in e.links %}<a href="{{ link.url }}">{{ link.label }}</a>{% unless forloop.last %} · {% endunless %}{% endfor %}</small>{% endif %}
{% endfor %}

### Selected work

{% assign selected = site.experiences | where: "featured", true | where_exp: "e", "e.end != nil" | sort: "order" %}
{% for e in selected %}
**{{ e.role }} at [{{ e.company }}]({{ e.website }})** ({{ e.start | slice: 0, 4 }}–{{ e.end | slice: 0, 4 }})<br>
{{ e.content | markdownify }}
{% if e.links.size > 0 %}<small>{% for link in e.links %}<a href="{{ link.url }}">{{ link.label }}</a>{% unless forloop.last %} · {% endunless %}{% endfor %}</small>{% endif %}
{% endfor %}

### Side project

[**Auto-Focus**](https://auto-focus.app) is a macOS app that flips on Do Not Disturb when it sees you've settled into work. Built it because I wanted it to exist. No timers, no manual toggling, no activity data leaves the Mac.

### Earlier

The full archive (including earlier Zendesk roles, the Glint Solar stint, and where I learned to ship) is on the [experience page](/experience).

### Education

**MSc Computer Science**, IT University of Copenhagen (2019–2021)<br>
**BSc Media Informatics**, Hochschule Flensburg (2015–2019)

Email me at <blog@janschill.de>.
