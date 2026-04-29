---
layout: default
title: Next Session
---

{% assign sessions = site.sessions | sort: 'name' | reverse %}
{% assign next = sessions | first %}

{% if next %}
<p class="session-meta">Next up: <strong>{{ next.title }}</strong></p>
{{ next.content }}
{% else %}
No sessions planned yet.
{% endif %}
