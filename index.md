---
layout: default
title: Home
nav_order: 1
description: "This site has moved to data.poverty-action.org/data-cleaning"
permalink: /
---

<style>
.redirect-notice {
  max-width: 800px;
  margin: 60px auto;
  padding: 40px;
  text-align: center;
  border: 2px solid #0066cc;
  border-radius: 8px;
  background-color: #f8f9fa;
}

.redirect-notice h1 {
  color: #0066cc;
  margin-bottom: 20px;
}

.redirect-notice p {
  font-size: 1.1em;
  line-height: 1.6;
  margin: 20px 0;
}

.redirect-notice .countdown {
  font-size: 2em;
  font-weight: bold;
  color: #0066cc;
  margin: 30px 0;
}

.redirect-notice .new-link {
  display: inline-block;
  padding: 12px 24px;
  margin: 20px 0;
  background-color: #0066cc;
  color: white;
  text-decoration: none;
  border-radius: 5px;
  font-weight: bold;
  font-size: 1.1em;
}

.redirect-notice .new-link:hover {
  background-color: #0052a3;
}

.redirect-notice .contact {
  margin-top: 30px;
  font-size: 0.9em;
  color: #666;
}
</style>

<div class="redirect-notice">
  <h1>This Site Has Moved!</h1>

  <p>The IPA & GPRL Data Cleaning Guide has been migrated to a new location.</p>

  <p><strong>You will be automatically redirected in <span id="countdown">5</span> seconds...</strong></p>

  <p>Or click the button below to go there now:</p>

  <a href="https://data.poverty-action.org/data-cleaning/" class="new-link">Go to New Site</a>

  <p style="margin-top: 30px;">New location: <a href="https://data.poverty-action.org/data-cleaning/">https://data.poverty-action.org/data-cleaning/</a></p>

  <div class="contact">
    Questions? Contact <a href="mailto:researchsupport@poverty-action.org">researchsupport@poverty-action.org</a>
  </div>
</div>

<script>
// Homepage redirect countdown (only runs on homepage)
// The default.html layout handles redirects for all other pages
let seconds = 5;
const countdownElement = document.getElementById('countdown');
const newUrl = 'https://data.poverty-action.org/data-cleaning/';

const countdownTimer = setInterval(function() {
  seconds--;
  countdownElement.textContent = seconds;

  if (seconds <= 0) {
    clearInterval(countdownTimer);
    window.location.href = newUrl;
  }
}, 1000);
</script>
