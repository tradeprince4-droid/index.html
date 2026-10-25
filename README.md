# index.html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NEURAL JOURNAL</title>
<style>
*{box-sizing:border-box}
body{
  margin:0;background:#080c10;color:#00ff9c;
  font-family:monospace;padding:20px;
}
main{max-width:750px;margin:auto}
h1{letter-spacing:4px;text-shadow:0 0 12px #00ff9c}
.panel{
  border:1px solid #00ff9c;padding:18px;
  margin:18px 0;background:#0c1318;
  box-shadow:0 0 12px #00ff9c15;
}
input,textarea,select{
  width:100%;padding:12px;margin:8px 0;
  background:#080c10;color:#00ff9c;
  border:1px solid #245b49;font:inherit;
}
textarea{min-height:130px;resize:vertical}
button{
  background:#00ff9c;color:#080c10;
  padding:12px 16px;border:0;
  font-weight:bold;cursor:pointer;margin:5px 3px 5px 0;
}
.entry{border-bottom:1px solid #245b49;padding:12px 0}
small{color:#7eab9b}
.muted{color:#7eab9b}
</style>
</head>
<body>
<main>
<h1>NEURAL JOURNAL</h1>
<p class="muted">PERSONAL LOG SYSTEM // V1.0</p>

<div class="panel">
  <small>SYSTEM TIME</small>
  <h2 id="clock">--:--:--</h2>
  <small id="date"></small>
</div>

<div class="panel">
  <h3>+ NEW ENTRY</h3>
  <input id="title" placeholder="Entry title">
  <select id="mood">
    <option>Focused</option>
    <option>Happy</option>
    <option>Motivated</option>
    <option>Tired</option>
    <option>Reflective</option>
  </select>
  <textarea id="content" placeholder="What did you learn today?"></textarea>
  <button onclick="saveEntry()">SAVE ENTRY</button>
  <p id="status" class="muted"></p>
</div>

<div class="panel">
  <h3>ARCHIVE DATABASE</h3>
  <input id="search" placeholder="Search entries..."
         oninput="renderEntries()">
  <p id="count">0 ENTRIES</p>
  <div id="entries"></div>
  <button onclick="exportData()">EXPORT BACKUP</button>
</div>
</main>

<script>
const $ = id => document.getElementById(id);
let logs = [];

try {
  logs = JSON.parse(localStorage.getItem("neuralLogs") || "[]");
  if (!Array.isArray(logs)) logs = [];
} catch {
  logs = [];
}

function updateClock(){
  const now = new Date();
  $("clock").textContent = now.toLocaleTimeString();
  $("date").textContent = now.toLocaleDateString(
    undefined,{weekday:"long",year:"numeric",
    month:"long",day:"numeric"}
  );
}
setInterval(updateClock,1000);
updateClock();

function saveEntry(){
  const title = $("title").value.trim();
  const content = $("content").value.trim();

  if(!title || !content){
    $("status").textContent = "ERROR: ENTER A TITLE AND ENTRY.";
    return;
  }

  logs.unshift({
    id: Date.now(),
    title,
    content,
    mood: $("mood").value,
    date: new Date().toLocaleString()
  });

  try {
    localStorage.setItem("neuralLogs",JSON.stringify(logs));
  } catch {
    logs.shift();
    $("status").textContent =
      "Storage unavailable or full. Export your backup.";
    return;
  }

  $("title").value = "";
  $("content").value = "";
  $("status").textContent = "ENTRY SAVED SUCCESSFULLY.";
  renderEntries();
}

function renderEntries(){
  const query = $("search").value.toLowerCase();
  const filtered = logs.filter(log =>
    (log.title+" "+log.content+" "+log.mood)
      .toLowerCase().includes(query)
  );

  $("count").textContent = filtered.length + " ENTRIES";
  $("entries").replaceChildren();

  filtered.forEach(log => {
    const article = document.createElement("article");
    article.className = "entry";

    const heading = document.createElement("h3");
    heading.textContent = log.title;

    const meta = document.createElement("small");
    meta.textContent = log.date+" | "+log.mood;

    const text = document.createElement("p");
    text.style.whiteSpace = "pre-wrap";
    text.textContent = log.content;

    const del = document.createElement("button");
    del.textContent = "DELETE";
    del.onclick = () => {
      if(confirm("Delete this entry?")){
        logs = logs.filter(item => item.id !== log.id);
        localStorage.setItem("neuralLogs",JSON.stringify(logs));
        renderEntries();
      }
    };

    article.append(heading,meta,text,del);
    $("entries").append(article);
  });
}

function exportData(){
  const file = new Blob(
    [JSON.stringify(logs,null,2)],
    {type:"application/json"}
  );
  const url = URL.createObjectURL(file);
  const a = document.createElement("a");
  a.href = url;
  a.download = "neural-journal-backup.json";
  a.click();
  URL.revokeObjectURL(url);
}

renderEntries();
</script>
</body>
</html>
