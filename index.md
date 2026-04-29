---
layout: default
title: Today
---

<div id="session-header"></div>
<div id="upcoming"></div>
<div id="session-content"></div>

<script>
const GITHUB_REPO = 'oharab/oharab.github.io';
const SESSIONS_PATH = '_sessions';

// All sessions injected by Jekyll at build time
const sessions = [
  {% assign sorted = site.sessions | sort: 'date' %}
  {% for s in sorted %}
  {
    date: "{{ s.date | date: '%Y-%m-%d' }}",
    title: {{ s.title | jsonify }},
    url: "{{ s.url }}",
    content: {{ s.content | jsonify }},
    filename: "{{ s.path | split: '/' | last }}"
  }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

// ── Auth ──────────────────────────────────────────────────────────────────────

function getToken() { return localStorage.getItem('gh_token'); }
function setToken(t) { localStorage.setItem('gh_token', t); }
function clearToken() { localStorage.removeItem('gh_token'); }

function renderAuthButton() {
  const btn = document.getElementById('gh-auth-btn');
  if (!btn) return;
  if (getToken()) {
    btn.textContent = 'Disconnect GitHub';
    btn.classList.add('connected');
  } else {
    btn.textContent = 'Connect GitHub';
    btn.classList.remove('connected');
  }
}

function showTokenModal() {
  document.getElementById('token-modal').style.display = 'flex';
  document.getElementById('token-input').value = '';
  document.getElementById('token-input').focus();
}

function hideTokenModal() {
  document.getElementById('token-modal').style.display = 'none';
}

function saveToken() {
  const t = document.getElementById('token-input').value.trim();
  if (!t) return;
  setToken(t);
  hideTokenModal();
  renderAuthButton();
  injectInteractivity();
}

// ── Date logic ────────────────────────────────────────────────────────────────

const today = new Date();
today.setHours(0, 0, 0, 0);

function parseDate(str) {
  const [y, m, d] = str.split('-').map(Number);
  return new Date(y, m - 1, d);
}

function formatDate(str) {
  const [y, m, d] = str.split('-').map(Number);
  return new Date(y, m - 1, d).toLocaleDateString('en-GB', { weekday: 'long', day: 'numeric', month: 'long' });
}

const todayStr = [today.getFullYear(), String(today.getMonth() + 1).padStart(2, '0'), String(today.getDate()).padStart(2, '0')].join('-');
const requestedDate = new URLSearchParams(window.location.search).get('date');
const todaySession = sessions.find(s => s.date === todayStr);
const futureSessions = sessions.filter(s => parseDate(s.date) > today).sort((a, b) => a.date.localeCompare(b.date));
const active = (requestedDate ? sessions.find(s => s.date === requestedDate) : null) || todaySession || futureSessions[0] || null;

// ── Render ────────────────────────────────────────────────────────────────────

const header = document.getElementById('session-header');
if (active) {
  const isToday = active.date === todayStr;
  header.innerHTML = `<p class="session-meta"><strong>${isToday ? "Today's session" : "Next session: " + formatDate(active.date)}</strong></p>`;
} else {
  header.innerHTML = `<p class="session-meta">No upcoming sessions planned.</p>`;
}

const upcomingEl = document.getElementById('upcoming');
const upcomingList = todaySession ? futureSessions : futureSessions.slice(1);
if (upcomingList.length > 0) {
  upcomingEl.innerHTML = `
    <details class="upcoming-box">
      <summary>Upcoming sessions (${upcomingList.length})</summary>
      <ul>${upcomingList.map(s => `<li><a href="/?date=${s.date}">${s.title}</a> — ${formatDate(s.date)}</li>`).join('')}</ul>
    </details>`;
}

const contentEl = document.getElementById('session-content');
if (active) {
  contentEl.innerHTML = active.content;
} else {
  contentEl.innerHTML = '<p>No sessions planned yet.</p>';
}

// ── Interactivity ─────────────────────────────────────────────────────────────

function injectInteractivity() {
  injectCheckboxes();
  injectDemoLinks();
  injectLogInputs();
  injectKneeRatings();
}

function injectDemoLinks() {
  if (!active) return;
  contentEl.querySelectorAll('td').forEach(td => {
    const text = td.textContent.trim();
    if (!/^https?:\/\/(www\.)?(youtu\.be|youtube\.com)/.test(text)) return;
    td.innerHTML = `<a href="${text}" target="_blank" rel="noopener" class="demo-link" aria-label="Watch demo">▶</a>`;
    td.style.textAlign = 'center';
  });
}

function injectCheckboxes() {
  if (!active) return;
  let idx = 0;
  contentEl.querySelectorAll('td').forEach(td => {
    const text = td.textContent.trim();
    if (text !== '[ ]' && text !== '[x]') return;
    const checked = text === '[x]';
    td.innerHTML = `<input type="checkbox" class="done-check" data-index="${idx++}" ${checked ? 'checked' : ''} aria-label="Mark done">`;
    td.style.textAlign = 'center';
    td.style.width = '2.5rem';
  });
  contentEl.querySelectorAll('.done-check').forEach(cb => {
    cb.addEventListener('change', scheduleCommit);
  });
}

function injectLogInputs() {
  if (!active) return;
  // Find the session log table: has headers "Field" and "Value"
  contentEl.querySelectorAll('table').forEach(table => {
    const headers = [...table.querySelectorAll('th')].map(th => th.textContent.trim());
    if (!headers.includes('Field') || !headers.includes('Value')) return;
    table.querySelectorAll('tr').forEach(row => {
      const cells = row.querySelectorAll('td');
      if (cells.length < 2) return;
      const fieldName = cells[0].textContent.trim();
      const currentVal = cells[1].textContent.trim();
      const input = document.createElement('input');
      input.type = 'text';
      input.className = 'log-input';
      input.value = currentVal;
      input.dataset.field = fieldName;
      input.setAttribute('aria-label', fieldName);
      cells[1].textContent = '';
      cells[1].appendChild(input);
      input.addEventListener('change', scheduleCommit);
    });
  });
}

function injectKneeRatings() {
  if (!active) return;
  contentEl.querySelectorAll('p, li').forEach(el => {
    if (!el.innerHTML.includes('rate right knee')) return;
    // Rendered from \_\_ — shows as __ in HTML text
    if (!el.innerHTML.includes('__')) return;
    el.innerHTML = el.innerHTML.replace(
      '__',
      `<input type="number" class="knee-input" min="0" max="10" placeholder="—" aria-label="Knee pain rating">`
    );
    el.querySelector('.knee-input').addEventListener('change', scheduleCommit);
  });
}

// ── Commit ────────────────────────────────────────────────────────────────────

let commitTimer = null;

function scheduleCommit() {
  if (!getToken()) {
    showTokenModal();
    return;
  }
  showSaving(true);
  clearTimeout(commitTimer);
  commitTimer = setTimeout(commitSessionState, 1200);
}

async function commitSessionState() {
  try {
    const metaRes = await fetch(`https://api.github.com/repos/${GITHUB_REPO}/contents/${SESSIONS_PATH}/${active.filename}`, {
      headers: { 'Authorization': `token ${getToken()}`, 'Accept': 'application/vnd.github+json' }
    });
    if (metaRes.status === 401) { clearToken(); renderAuthButton(); throw new Error('Token expired — reconnect GitHub'); }
    const meta = await metaRes.json();

    let raw = decodeURIComponent(escape(atob(meta.content.replace(/\n/g, ''))));

    // 1. Checkboxes — replace [ ] / [x] in order from DOM
    const allBoxes = [...contentEl.querySelectorAll('.done-check')];
    let cbIdx = 0;
    raw = raw.replace(/\[ \]|\[x\]/g, () => {
      const box = allBoxes[cbIdx++];
      return box ? (box.checked ? '[x]' : '[ ]') : '[ ]';
    });

    // 2. Session log — replace each value cell by field name
    contentEl.querySelectorAll('.log-input').forEach(input => {
      const field = input.dataset.field.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
      const val = input.value.trim();
      raw = raw.replace(
        new RegExp(`(\\|\\s*${field}\\s*\\|\\s*)([^|]*)(\\s*\\|)`),
        (_, pre, _old, post) => `${pre}${val}${post}`
      );
    });

    // 3. Knee ratings — replace \_\_ with typed value, restore if empty
    contentEl.querySelectorAll('.knee-input').forEach(input => {
      const val = input.value.trim();
      raw = raw.replace(/\\_\\_(\s*\/\s*10)/, `${val || '\\_\\_'}$1`);
    });

    const commitRes = await fetch(`https://api.github.com/repos/${GITHUB_REPO}/contents/${SESSIONS_PATH}/${active.filename}`, {
      method: 'PUT',
      headers: { 'Authorization': `token ${getToken()}`, 'Accept': 'application/vnd.github+json', 'Content-Type': 'application/json' },
      body: JSON.stringify({
        message: 'track: update session log',
        content: btoa(unescape(encodeURIComponent(raw))),
        sha: meta.sha
      })
    });

    if (!commitRes.ok) throw new Error(`Commit failed: ${commitRes.status}`);
  } catch (e) {
    console.error(e);
    alert(`Could not save: ${e.message}`);
  } finally {
    showSaving(false);
  }
}

function showSaving(on) {
  document.getElementById('saving-indicator').style.display = on ? 'inline' : 'none';
}

// ── Init ──────────────────────────────────────────────────────────────────────

renderAuthButton();
injectInteractivity();

document.getElementById('gh-auth-btn').addEventListener('click', () => {
  if (getToken()) { clearToken(); renderAuthButton(); }
  else { showTokenModal(); }
});

document.getElementById('token-save').addEventListener('click', saveToken);
document.getElementById('token-input').addEventListener('keydown', e => { if (e.key === 'Enter') saveToken(); });
document.getElementById('token-cancel').addEventListener('click', hideTokenModal);
</script>
