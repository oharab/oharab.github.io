---
layout: default
title: Today
---

<div id="session-header"></div>
<div id="upcoming"></div>
<div id="session-content"></div>

<script>
// All sessions injected by Jekyll at build time
const sessions = [
  {% assign sorted = site.sessions | sort: 'date' %}
  {% for s in sorted %}
  {
    date: "{{ s.date | date: '%Y-%m-%d' }}",
    title: {{ s.title | jsonify }},
    url: "{{ s.url }}",
    content: {{ s.content | jsonify }}
  }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

const today = new Date();
today.setHours(0, 0, 0, 0);

function parseDate(str) {
  const [y, m, d] = str.split('-').map(Number);
  return new Date(y, m - 1, d);
}

const todayStr = today.toISOString().slice(0, 10);

const todaySession   = sessions.find(s => s.date === todayStr);
const futureSession  = sessions.filter(s => parseDate(s.date) > today).sort((a, b) => a.date.localeCompare(b.date));
const upcoming       = futureSession.slice(todaySession ? 0 : 1); // if today has a session, all future are upcoming

const active = todaySession || futureSession[0] || null;

// Render header banner
const header = document.getElementById('session-header');
if (active) {
  const isToday = active.date === todayStr;
  const dateLabel = isToday ? "Today" : formatDate(active.date);
  header.innerHTML = `<p class="session-meta"><strong>${isToday ? "Today's session" : "Next session: " + dateLabel}</strong></p>`;
} else {
  header.innerHTML = `<p class="session-meta">No upcoming sessions planned.</p>`;
}

// Render upcoming list (sessions after the displayed one)
const upcomingEl = document.getElementById('upcoming');
const upcomingList = todaySession ? futureSession : futureSession.slice(1);
if (upcomingList.length > 0) {
  upcomingEl.innerHTML = `
    <details class="upcoming-box">
      <summary>Upcoming sessions (${upcomingList.length})</summary>
      <ul>
        ${upcomingList.map(s => `<li><a href="${s.url}">${s.title}</a> — ${formatDate(s.date)}</li>`).join('')}
      </ul>
    </details>`;
}

// Render session content
const contentEl = document.getElementById('session-content');
if (active) {
  contentEl.innerHTML = active.content;
} else {
  contentEl.innerHTML = '<p>No sessions planned yet.</p>';
}

function formatDate(str) {
  const [y, m, d] = str.split('-').map(Number);
  return new Date(y, m - 1, d).toLocaleDateString('en-GB', { weekday: 'long', day: 'numeric', month: 'long' });
}
</script>
