<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Discord Webhook Manager</title>
  <style>
    :root {
      --bg-color: #2f3136;
      --card-color: #202225;
      --text-color: #ffffff;
      --highlight-color: #5865F2;
      --error-color: #ED4245;
      --muted-color: #72767d;
    }
    body {
      background: var(--bg-color);
      color: var(--text-color);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      padding: 20px;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .container {
      background: var(--card-color);
      padding: 20px;
      border-radius: 10px;
      width: 100%;
      max-width: 500px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
      text-align: center;
    }
    h1 { margin-bottom: 15px; }
    .tabs { display: flex; justify-content: space-between; margin-bottom: 15px; border-radius: 5px; overflow: hidden; }
    .tab-button {
      flex: 1;
      padding: 10px;
      background: var(--card-color);
      border: none;
      color: var(--muted-color);
      font-weight: bold;
      cursor: pointer;
      transition: background 0.3s, color 0.3s;
    }
    .tab-button.active { background: var(--highlight-color); color: white; }
    .tab-content { display: none; margin-top: 10px; }
    .tab-content.active { display: block; }
    input[type="text"], input[type="url"] {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 5px;
      background: #40444b;
      border: none;
      color: white;
      transition: box-shadow 0.3s;
    }
    textarea {
      width: 100%;
      padding: 12px;
      margin: 10px 0;
      border-radius: 8px;
      background: #40444b;
      border: 1px solid var(--highlight-color);
      color: white;
      height: 80px;
      resize: none;
      transition: box-shadow 0.3s;
    }
    textarea:focus, input:focus {
      outline: none;
      box-shadow: 0 0 8px var(--highlight-color);
      border: 1px solid var(--highlight-color);
    }
    button.action {
      width: 100%;
      margin-top: 10px;
      padding: 12px;
      background: var(--highlight-color);
      border: none;
      color: white;
      font-weight: bold;
      border-radius: 5px;
      cursor: pointer;
      transition: background 0.3s, transform 0.2s;
    }
    button.action:hover {
      background: #4752c4;
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(88, 101, 242, 0.3);
    }
    .accordion {
      background: #40444b;
      color: white;
      cursor: pointer;
      padding: 10px;
      width: 100%;
      text-align: left;
      border: none;
      outline: none;
      font-weight: bold;
      margin-top: 10px;
      border-radius: 5px;
      transition: background 0.3s;
    }
    .accordion:hover { background: #4a4d52; }
    .panel {
      padding: 0 10px;
      background: var(--card-color);
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.3s ease-out;
      border-radius: 0 0 5px 5px;
      text-align: left;
      font-size: 14px;
      color: var(--muted-color);
    }
    pre {
      background: #40444b;
      padding: 10px;
      border-radius: 5px;
      overflow-x: auto;
      color: white;
      margin-top: 5px;
      margin-bottom: 5px;
    }
    code { font-family: monospace; }
    .footer { margin-top: 20px; font-size: 14px; color: var(--muted-color); text-align: center; }
  </style>
</head>
<body>
<div class="container">
  <h1>Discord Webhook Manager</h1>
  <div class="tabs">
    <button class="tab-button active" onclick="openTab('delete')">Delete Webhook</button>
    <button class="tab-button" onclick="openTab('test')">Test Webhook</button>
    <button class="tab-button" onclick="openTab('info')">Webhook Info</button>
  </div>

  <div id="delete" class="tab-content active">
    <input type="url" id="deleteUrl" placeholder="https://discord.com/api/webhooks/...">
    <p style="font-size: 14px;">Enter the webhook URL you want to delete. Due to CORS restrictions, we'll show you how to properly delete it.</p>
    <button class="action" onclick="copyDeleteCurl()">Delete Webhook</button>
  </div>

  <div id="test" class="tab-content">
    <input type="url" id="testUrl" placeholder="Webhook URL">

    <p style="font-size: 14px; margin-top: 10px; color: var(--muted-color);">
      Send a test message to verify your webhook is working.
    </p>

    <input type="text" id="testUsername" placeholder="Username (optional)">

    <textarea id="testMessage" placeholder="Message"></textarea>

    <input type="text" id="embedTitle" placeholder="Embed Title (optional)">

    <textarea id="embedDescription" placeholder="Embed Description (optional)"></textarea>

    <div style="margin: 10px 0; display: flex; align-items: center; gap: 10px;">
      <div style="flex: 0 0 auto;">
        <label for="embedColor" style="font-size: 14px;">Embed Color:</label><br>
        <input type="color" id="embedColor" value="#5865F2" style="width: 50px; height: 36px; border: none; background: none; cursor: pointer;">
      </div>
      <input type="text" id="embedColorHex" value="#5865F2"
        style="flex: 1; height: 36px; padding: 0 10px; background: #40444b; border: 1px solid #5865F2; color: white; border-radius: 5px;">
    </div>

    <button class="action" onclick="sendTestMessage()">Send Test Message</button>
  </div>

  <div id="info" class="tab-content">
    <input type="url" id="infoUrl" placeholder="Webhook URL">
    <button class="action" onclick="fetchWebhookInfo()">Get Info</button>
  </div>

  <button class="accordion">How To Delete Discord Webhooks</button>
  <div class="panel">
    <p>Due to browser security restrictions (CORS), client-side applications can't directly delete Discord webhooks. Here are three ways to delete a webhook:</p>
    <p><strong>Method 1:</strong> Open the webhook URL in your browser and click Delete.</p>
    <p><strong>Method 2:</strong> Using curl command:</p>
    <pre><code>curl -X DELETE webhook_url_here</code></pre>
    <p><strong>Method 3:</strong> In Discord, go to the channel settings, select Integrations, then Webhooks, and delete it there.</p>
  </div>

  <button class="accordion">What are Discord Webhooks?</button>
  <div class="panel">
    <p>Discord Webhooks are a simple way to post messages to channels without needing a bot. They can be used to send automated messages from apps, websites, or services.</p>
    <p>Each webhook has a unique URL that should be kept private, as anyone with the URL can post messages to your channel.</p>
  </div>
</div>

<div class="footer">Made with ❤️ for Discord users</div>

<script>
function openTab(tabName) {
  const tabs = document.getElementsByClassName('tab-content');
  const buttons = document.getElementsByClassName('tab-button');
  for (let tab of tabs) tab.classList.remove('active');
  for (let btn of buttons) btn.classList.remove('active');
  document.getElementById(tabName).classList.add('active');
  event.currentTarget.classList.add('active');
}

const accordions = document.getElementsByClassName('accordion');
for (let acc of accordions) {
  acc.addEventListener('click', function() {
    this.classList.toggle('active');
    const panel = this.nextElementSibling;
    panel.style.maxHeight ? panel.style.maxHeight = null : panel.style.maxHeight = panel.scrollHeight + "px";
  });
}

function copyDeleteCurl() {
  const url = document.getElementById('deleteUrl').value.trim();
  if (!url.startsWith('https://discord.com/api/webhooks/')) return alert('Invalid URL.');
  const command = `curl -X DELETE "${url}"`;
  navigator.clipboard.writeText(command).then(() => alert('cURL command copied! Paste it in your terminal.')).catch(() => alert('Clipboard copy failed.'));
}

async function sendTestMessage() {
  const url = document.getElementById('testUrl').value.trim();
  const username = document.getElementById('testUsername').value.trim();
  const message = document.getElementById('testMessage').value.trim();
  const embedTitle = document.getElementById('embedTitle').value.trim();
  const embedDescription = document.getElementById('embedDescription').value.trim();
  const embedColor = document.getElementById('embedColor').value.replace('#', '') || '5865F2';

  if (!url || !message) return alert('Webhook URL and message are required.');

  const payload = {
    content: message,
    username: username || undefined,
    embeds: [
      {
        title: embedTitle || undefined,
        description: embedDescription || undefined,
        color: parseInt(embedColor, 16)
      }
    ]
  };

  try {
    await fetch(url, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(payload)
    });
    alert('✅ Test message sent!');
  } catch (err) {
    alert('❌ Failed to send message.');
  }
}

async function fetchWebhookInfo() {
  const url = document.getElementById('infoUrl').value.trim();
  if (!url) return alert('Enter webhook URL.');
  try {
    const res = await fetch(url);
    const data = await res.json();
    alert(`Name: ${data.name}\nChannel ID: ${data.channel_id}\nGuild ID: ${data.guild_id}`);
  } catch (err) {
    alert('❌ Failed to fetch info.');
  }
}

// Synchronize embed color picker and text hex
document.getElementById('embedColor').addEventListener('input', function() {
  document.getElementById('embedColorHex').value = this.value;
});
document.getElementById('embedColorHex').addEventListener('input', function() {
  if (this.value.startsWith('#') && this.value.length === 7) {
    document.getElementById('embedColor').value = this.value;
  }
});
</script>
</body>
</html>
