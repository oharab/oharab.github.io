---
layout: default
title: Session History
permalink: /history/
---

# Session History

{% assign sessions = site.sessions | sort: 'name' | reverse %}

{% if sessions.size == 0 %}
No sessions logged yet.
{% else %}
{% for session in sessions %}
## [{{ session.title }}]({{ session.url }})
{% endfor %}
{% endif %}
