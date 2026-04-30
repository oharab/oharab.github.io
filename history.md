---
layout: default
title: Session History
permalink: /history/
---

# Session History

{% assign sessions = site.sessions | sort: 'date' | reverse %}

{% if sessions.size == 0 %}
No sessions logged yet.
{% else %}
{% for session in sessions %}
## [{{ session.title }}](/?date={{ session.date | date: '%Y-%m-%d' }})
{% endfor %}
{% endif %}
