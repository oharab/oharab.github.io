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
  injectCheckboxes();
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
const todaySession  = sessions.find(s => s.date === todayStr);
const futureSessions = sessions.filter(s => parseDate(s.date) > today).sort((a, b) => a.date.localeCompare(b.date));
const active = todaySession || futureSessions[0] || null;

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
      <ul>${upcomingList.map(s => `<li><a href="${s.url}">${s.title}</a> — ${formatDate(s.date)}</li>`).join('')}</ul>
    </details>`;
}

const contentEl = document.getElementById('session-content');
if (active) {
  contentEl.innerHTML = active.content;
} else {
  contentEl.innerHTML = '<p>No sessions planned yet.</p>';
}

// ── Checkboxes ────────────────────────────────────────────────────────────────

function injectCheckboxes() {
  if (!active) return;
  const cells = contentEl.querySelectorAll('td');
  let checkboxIndex = 0;

  cells.forEach(td => {
    const text = td.textContent.trim();
    if (text !== '[ ]' && text !== '[x]') return;
    const checked = text === '[x]';
    const idx = checkboxIndex++;
    td.innerHTML = `<input type="checkbox" class="done-check" data-index="${idx}" ${checked ? 'checked' : ''} aria-label="Mark done">`;
    td.style.textAlign = 'center';
    td.style.width = '2.5rem';
  });

  contentEl.querySelectorAll('.done-check').forEach(cb => {
    cb.addEventListener('change', () => handleCheck(cb));
  });
}

async function handleCheck(cb) {
  if (!getToken()) {
    cb.checked = !cb.checked;
    alert('Connect GitHub first using the button in the header.');
    return;
  }

  cb.disabled = true;
  showSaving(true);

  try {
    // Fetch current file from GitHub API (gives us both raw content and SHA)
    const metaRes = await fetch(`https://api.github.com/repos/${GITHUB_REPO}/contents/${SESSIONS_PATH}/${active.filename}`, {
      headers: { 'Authorization': `token ${getToken()}`, 'Accept': 'application/vnd.github+json' }
    });
    if (metaRes.status === 401) { clearToken(); renderAuthButton(); throw new Error('Token expired — reconnect GitHub'); }
    const meta = await metaRes.json();

    // Decode current raw markdown from the API response
    let raw = decodeURIComponent(escape(atob(meta.content.replace(/\n/g, ''))));

    // Rebuild checkbox states from current DOM
    const allBoxes = [...contentEl.querySelectorAll('.done-check')];
    let idx = 0;
    raw = raw.replace(/\[ \]|\[x\]/g, () => {
      const box = allBoxes[idx++];
      return box ? (box.checked ? '[x]' : '[ ]') : '[ ]';
    });

    // Commit updated file
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
    cb.checked = !cb.checked;
    console.error(e);
    alert(`Could not save: ${e.message}`);
  } finally {
    cb.disabled = false;
    showSaving(false);
  }
}

function showSaving(on) {
  document.getElementById('saving-indicator').style.display = on ? 'inline' : 'none';
}

// ── Init ──────────────────────────────────────────────────────────────────────

renderAuthButton();
injectCheckboxes();

document.getElementById('gh-auth-btn').addEventListener('click', () => {
  if (getToken()) {
    clearToken();
    renderAuthButton();
  } else {
    showTokenModal();
  }
});

document.getElementById('token-save').addEventListener('click', saveToken);
document.getElementById('token-input').addEventListener('keydown', e => { if (e.key === 'Enter') saveToken(); });
document.getElementById('token-cancel').addEventListener('click', hideTokenModal);
</script>
