<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Love Dashboard 📈❤️</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', sans-serif; }
        body { background: #111827; color: white; display: flex; justify-content: center; align-items: center; min-height: 100vh; }
        .dashboard { background: #1f2937; padding: 2.5rem; border-radius: 15px; width: 90%; max-width: 500px; text-align: center; box-shadow: 0 10px 25px rgba(0,0,0,0.5); border: 2px solid #374151; }
        h1 { color: #f43f5e; margin-bottom: 5px; font-size: 1.8rem; }
        .status-badge { display: inline-block; padding: 6px 15px; border-radius: 20px; font-size: 0.85rem; font-weight: bold; background: #374151; color: #9ca3af; margin-bottom: 1.5rem; }
        .status-active { background: #065f46; color: #34d399; }
        .log-box { background: #111827; border-radius: 10px; padding: 1.5rem; min-height: 150px; display: flex; flex-direction: column; justify-content: center; align-items: center; border: 1px solid #374151; margin-bottom: 1.5rem;}
        .waiting-text { color: #6b7280; font-style: italic; }
        .alert-text { color: #34d399; font-size: 1.3rem; font-weight: bold; animation: pop 0.5s ease-out; }
        .test-btn { background: #374151; color: #e5e7eb; padding: 8px 15px; border: none; border-radius: 8px; cursor: pointer; font-size: 0.9rem; font-weight: 500; }
        .test-btn:hover { background: #4b5563; }
        .timestamp { color: #9ca3af; font-size: 0.8rem; margin-top: 10px; }
    </style>
</head>
<body>

    <div class="dashboard">
        <h1>Proposal Monitoring System</h1>
        <div class="status-badge" id="statusBadge">Connecting...</div>
        
        <div class="log-box" id="logBox">
            <p class="waiting-text" id="waitingMsg">Waiting for her to make a decision... 🕒</p>
        </div>

        <button class="test-btn" onclick="sendSelfTest()">Send Test Signal ⚡</button>
    </div>

    <audio id="notifSound" src="https://mixkit.co" preload="auto"></audio>

    <script>
        // THIS MATCHES THE CRUSH SITE PIPELINE EXACTLY
        const MY_SECRET_CHANNEL = "date_proposal_secure_pipeline_2026"; 

        const badge = document.getElementById('statusBadge');
        const logBox = document.getElementById('logBox');
        const audio = document.getElementById('notifSound');

        const eventSource = new EventSource(`https://ntfy.sh{MY_SECRET_CHANNEL}/sse`);

        eventSource.onopen = () => {
            badge.textContent = "🟢 LIVE & LISTENING";
            badge.className = "status-badge status-active";
        };

        eventSource.onmessage = (event) => {
            const data = JSON.parse(event.data);
            if (data.event === "message") {
                audio.play().catch(e => console.log("Audio waiting for user activity."));
                const time = new Date().toLocaleTimeString();
                logBox.innerHTML = `
                    <p class="alert-text">🎉 UPDATE RECEIVED!</p>
                    <p style="margin-top: 10px; font-size: 1.1rem; color: #e5e7eb;">${data.message}</p>
                    <p class="timestamp">Sync Time: ${time}</p>
                `;
            }
        };

        eventSource.onerror = () => {
            badge.textContent = "🔴 SYSTEM RECONNECTING...";
            badge.className = "status-badge";
        };

        function sendSelfTest() {
            fetch(`https://ntfy.sh{MY_SECRET_CHANNEL}`, {
                method: 'POST',
                body: "Connection test successful! The pipeline works perfectly."
            });
        }
    </script>
</body>
</html>
