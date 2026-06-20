# index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>BloodSafe IoT</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;900&display=swap');
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Inter', sans-serif; background: #0A1628; color: #fff; min-height: 100vh; padding: 0 0 40px 0; }
    .header { background: #0d1e3a; padding: 18px 20px; display: flex; align-items: center; justify-content: space-between; border-bottom: 2px solid #E53935; }
    .header-left { display: flex; align-items: center; gap: 10px; }
    .header-icon { font-size: 28px; }
    .header-title { font-size: 20px; font-weight: 900; color: #fff; }
    .header-sub { font-size: 11px; color: #90A4AE; margin-top: 2px; }
    .live-badge { background: #E53935; color: #fff; font-size: 11px; font-weight: 700; padding: 4px 10px; border-radius: 20px; animation: pulse 1.5s infinite; }
    @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: 0.5; } }
    .last-updated { text-align: center; font-size: 11px; color: #90A4AE; padding: 10px; background: #0d1e3a; }
    .alert-banner { margin: 16px; padding: 14px 18px; border-radius: 12px; font-size: 14px; font-weight: 700; display: none; align-items: center; gap: 10px; }
    .alert-banner.show { display: flex; }
    .alert-banner.critical { background: #E53935; color: #fff; }
    .alert-banner.warning { background: #FFC107; color: #000; }
    .alert-banner.normal { background: #2e7d32; color: #fff; }
    .alert-icon { font-size: 22px; }
    .alert-title { font-size: 15px; font-weight: 900; }
    .alert-desc { font-size: 12px; font-weight: 400; margin-top: 2px; }
    .cards { padding: 0 16px; display: flex; flex-direction: column; gap: 14px; }
    .card { background: #1A2940; border-radius: 16px; padding: 20px; border-left: 5px solid #1A2940; transition: border-color 0.4s; }
    .card.green { border-left-color: #4CAF50; }
    .card.yellow { border-left-color: #FFC107; }
    .card.red { border-left-color: #E53935; }
    .card-label { font-size: 12px; color: #90A4AE; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; }
    .card-value { font-size: 48px; font-weight: 900; line-height: 1; margin-bottom: 8px; }
    .card-value.green { color: #4CAF50; }
    .card-value.yellow { color: #FFC107; }
    .card-value.red { color: #E53935; }
    .card-status { display: inline-block; padding: 4px 12px; border-radius: 20px; font-size: 12px; font-weight: 700; }
    .card-status.green { background: #1b5e20; color: #4CAF50; }
    .card-status.yellow { background: #4d3800; color: #FFC107; }
    .card-status.red { background: #4a0000; color: #E53935; }
    .status-box { margin: 14px 16px 0; background: #1A2940; border-radius: 16px; padding: 18px 20px; }
    .status-box-title { font-size: 12px; color: #90A4AE; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 14px; }
    .status-rows { display: flex; flex-direction: column; gap: 10px; }
    .status-row { display: flex; align-items: center; justify-content: space-between; font-size: 14px; }
    .status-row-label { color: #90A4AE; }
    .status-row-value { font-weight: 700; color: #fff; }
    .refresh-btn { display: block; margin: 16px 16px 0; background: #E53935; color: #fff; border: none; border-radius: 12px; padding: 16px; font-size: 15px; font-weight: 700; width: calc(100% - 32px); cursor: pointer; font-family: 'Inter', sans-serif; }
    .inventory { margin: 14px 16px 0; background: #1A2940; border-radius: 16px; padding: 18px 20px; }
    .inventory-title { font-size: 12px; color: #90A4AE; font-weight: 600; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 14px; }
    .blood-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .blood-item { background: #0d1e3a; border-radius: 10px; padding: 12px; display: flex; justify-content: space-between; align-items: center; }
    .blood-type { font-size: 16px; font-weight: 900; color: #fff; }
    .blood-units { font-size: 12px; color: #90A4AE; margin-top: 2px; }
    .blood-dot { width: 12px; height: 12px; border-radius: 50%; }
    .loading { text-align: center; padding: 40px 20px; color: #90A4AE; font-size: 14px; }
    .spinner { width: 36px; height: 36px; border: 3px solid #1A2940; border-top: 3px solid #E53935; border-radius: 50%; animation: spin 0.8s linear infinite; margin: 0 auto 14px; }
    @keyframes spin { to { transform: rotate(360deg); } }
  </style>
</head>
<body>
  <div class="header">
    <div class="header-left">
      <div class="header-icon">🩸</div>
      <div>
        <div class="header-title">BloodSafe IoT</div>
        <div class="header-sub">Suwa Setha Hospital — Blood Bank</div>
      </div>
    </div>
    <div class="live-badge">● LIVE</div>
  </div>
  <div class="last-updated" id="lastUpdated">Connecting to cloud...</div>
  <div class="alert-banner" id="alertBanner">
    <div class="alert-icon" id="alertIcon">⚠️</div>
    <div>
      <div class="alert-title" id="alertTitle">Alert</div>
      <div class="alert-desc" id="alertDesc">Checking system...</div>
    </div>
  </div>
  <div class="loading" id="loadingBox">
    <div class="spinner"></div>
    Reading live data from cloud...
  </div>
  <div id="mainContent" style="display:none;">
    <div class="cards">
      <div class="card" id="tempCard">
        <div class="card-label">🌡️ Blood Storage Temperature</div>
        <div class="card-value" id="tempValue">--</div>
        <div class="card-status" id="tempStatus">Loading...</div>
      </div>
      <div class="card" id="stockCard">
        <div class="card-label">🩸 Blood Stock Level</div>
        <div class="card-value" id="stockValue">--</div>
        <div class="card-status" id="stockStatus">Loading...</div>
      </div>
    </div>
    <div class="status-box">
      <div class="status-box-title">📊 System Overview</div>
      <div class="status-rows">
        <div class="status-row">
          <span class="status-row-label">Overall Status</span>
          <span class="status-row-value" id="overallStatus">—</span>
        </div>
        <div class="status-row">
          <span class="status-row-label">Safe Temp Range</span>
          <span class="status-row-value">2°C – 6°C</span>
        </div>
        <div class="status-row">
          <span class="status-row-label">Critical Temp Above</span>
          <span class="status-row-value" style="color:#E53935;">8°C</span>
        </div>
        <div class="status-row">
          <span class="status-row-label">Low Stock Below</span>
          <span class="status-row-value" style="color:#FFC107;">30 Units</span>
        </div>
      </div>
    </div>
    <div class="inventory">
      <div class="inventory-title">🏥 Blood Type Inventory</div>
      <div class="blood-grid">
        <div class="blood-item"><div><div class="blood-type">A+</div><div class="blood-units">20 Units</div></div><div class="blood-dot" style="background:#4CAF50;"></div></div>
        <div class="blood-item"><div><div class="blood-type">A-</div><div class="blood-units">8 Units</div></div><div class="blood-dot" style="background:#FFC107;"></div></div>
        <div class="blood-item"><div><div class="blood-type">B+</div><div class="blood-units">15 Units</div></div><div class="blood-dot" style="background:#4CAF50;"></div></div>
        <div class="blood-item"><div><div class="blood-type">B-</div><div class="blood-units">5 Units</div></div><div class="blood-dot" style="background:#E53935;"></div></div>
        <div class="blood-item"><div><div class="blood-type">O+</div><div class="blood-units">25 Units</div></div><div class="blood-dot" style="background:#4CAF50;"></div></div>
        <div class="blood-item"><div><div class="blood-type">O-</div><div class="blood-units">3 Units</div></div><div class="blood-dot" style="background:#E53935;"></div></div>
        <div class="blood-item"><div><div class="blood-type">AB+</div><div class="blood-units">12 Units</div></div><div class="blood-dot" style="background:#4CAF50;"></div></div>
        <div class="blood-item"><div><div class="blood-type">AB-</div><div class="blood-units">7 Units</div></div><div class="blood-dot" style="background:#FFC107;"></div></div>
      </div>
    </div>
    <button class="refresh-btn" onclick="fetchData()">🔄 Refresh Live Data</button>
  </div>
  <script>
    const API_URL = "https://script.google.com/macros/s/AKfycbzKBglwCP2KyF6ed8HpbrSVC3WMHDwuQYZDdEK_ILK6N632VrPEnnEIFj-MUOy0fa9Z/exec";

    async function fetchData() {
      try {
        const res = await fetch(API_URL);
        const data = await res.json();
        const temp = parseFloat(data.temperature) || 0;
        const stock = parseInt(data.bloodStock) || 0;
        updateDisplay(temp, stock);
        const now = new Date();
        document.getElementById("lastUpdated").textContent = "Last updated: " + now.toLocaleTimeString();
        document.getElementById("loadingBox").style.display = "none";
        document.getElementById("mainContent").style.display = "block";
      } catch (e) {
        document.getElementById("loadingBox").innerHTML = "<div style='color:#E53935;font-size:14px;'>⚠️ Could not load data.<br><br><button onclick='fetchData()' style='background:#E53935;color:#fff;border:none;padding:10px 20px;border-radius:8px;font-size:14px;cursor:pointer;'>Try Again</button></div>";
      }
    }

    function updateDisplay(temp, stock) {
      const tempValEl = document.getElementById("tempValue");
      const tempStatusEl = document.getElementById("tempStatus");
      const tempCard = document.getElementById("tempCard");
      tempValEl.textContent = temp.toFixed(1) + "°C";
      if (temp > 8) { setCard(tempCard, tempValEl, tempStatusEl, "red", "🔴 CRITICAL - TOO HIGH"); }
      else if (temp < 2) { setCard(tempCard, tempValEl, tempStatusEl, "yellow", "🟡 WARNING - TOO LOW"); }
      else { setCard(tempCard, tempValEl, tempStatusEl, "green", "🟢 NORMAL"); }

      const stockValEl = document.getElementById("stockValue");
      const stockStatusEl = document.getElementById("stockStatus");
      const stockCard = document.getElementById("stockCard");
      stockValEl.textContent = stock + " Units";
      if (stock < 15) { setCard(stockCard, stockValEl, stockStatusEl, "red", "🔴 CRITICALLY LOW"); }
      else if (stock < 30) { setCard(stockCard, stockValEl, stockStatusEl, "yellow", "🟡 STOCK LOW"); }
      else { setCard(stockCard, stockValEl, stockStatusEl, "green", "🟢 SUFFICIENT"); }

      const banner = document.getElementById("alertBanner");
      banner.className = "alert-banner show";
      if (temp > 8) {
        banner.classList.add("critical");
        document.getElementById("alertIcon").textContent = "🚨";
        document.getElementById("alertTitle").textContent = "CRITICAL TEMPERATURE ALERT";
        document.getElementById("alertDesc").textContent = "Temperature " + temp.toFixed(1) + "°C exceeds safe limit! Check storage immediately.";
      } else if (stock < 30) {
        banner.classList.add("warning");
        document.getElementById("alertIcon").textContent = "⚠️";
        document.getElementById("alertTitle").textContent = "LOW BLOOD STOCK WARNING";
        document.getElementById("alertDesc").textContent = "Only " + stock + " units remaining. Restock required soon.";
      } else {
        banner.classList.add("normal");
        document.getElementById("alertIcon").textContent = "✅";
        document.getElementById("alertTitle").textContent = "ALL SYSTEMS NORMAL";
        document.getElementById("alertDesc").textContent = "Temperature and blood stock are within safe limits.";
      }

      const overall = document.getElementById("overallStatus");
      if (temp > 8) { overall.textContent = "🔴 CRITICAL ALERT"; overall.style.color = "#E53935"; }
      else if (stock < 30) { overall.textContent = "🟡 WARNING"; overall.style.color = "#FFC107"; }
      else { overall.textContent = "🟢 ALL NORMAL"; overall.style.color = "#4CAF50"; }
    }

    function setCard(card, valEl, statusEl, color, text) {
      card.className = "card " + color;
      valEl.className = "card-value " + color;
      statusEl.className = "card-status " + color;
      statusEl.textContent = text;
    }

    fetchData();
    setInterval(fetchData, 30000);
  </script>
</body>
</html>
