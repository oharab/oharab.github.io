---
layout: default
title: Today's Session
---

# Today's Session

{% assign sessions = site.sessions | sort: 'name' | reverse %}
{% assign latest = sessions | first %}

{% if latest %}
{{ latest.content }}
{% else %}
No sessions planned yet.
{% endif %}
