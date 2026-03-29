



<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Staff Application</title>
  <style>
    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #05060a;
      color: #f5f5f5;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
    }
    .card {
      background: rgba(15, 16, 25, 0.9);
      border-radius: 18px;
      padding: 24px 28px;
      width: 100%;
      max-width: 480px;
      box-shadow: 0 18px 45px rgba(0, 0, 0, 0.6);
      backdrop-filter: blur(18px);
      border: 1px solid rgba(255, 255, 255, 0.06);
    }
    h1 {
      font-size: 1.4rem;
      margin: 0 0 4px;
    }
    p.subtitle {
      margin: 0 0 18px;
      font-size: 0.9rem;
      color: #b0b3c0;
    }
    label {
      display: block;
      font-size: 0.85rem;
      margin-bottom: 4px;
      color: #d7d9e4;
    }
    input, textarea {
      width: 100%;
      padding: 9px 11px;
      border-radius: 10px;
      border: 1px solid rgba(255, 255, 255, 0.08);
      background: rgba(10, 11, 18, 0.9);
      color: #f5f5f5;
      font-size: 0.9rem;
      outline: none;
      box-sizing: border-box;
    }
    input:focus, textarea:focus {
      border-color: #5865f2;
      box-shadow: 0 0 0 1px rgba(88, 101, 242, 0.4);
    }
    textarea {
      resize: vertical;
      min-height: 80px;
    }
    .field {
      margin-bottom: 14px;
    }
    button {
      width: 100%;
      padding: 10px 14px;
      border-radius: 999px;
      border: none;
      background: linear-gradient(135deg, #5865f2, #3b82f6);
      color: #fff;
      font-weight: 600;
      font-size: 0.95rem;
      cursor: pointer;
      margin-top: 6px;
      transition: transform 0.08s ease, box-shadow 0.08s ease, opacity 0.08s ease;
      box-shadow: 0 10px 25px rgba(88, 101, 242, 0.45);
    }
    button:hover {
      transform: translateY(-1px);
      box-shadow: 0 14px 30px rgba(88, 101, 242, 0.6);
    }
    button:active {
      transform: translateY(0);
      box-shadow: 0 8px 18px rgba(88, 101, 242, 0.4);
      opacity: 0.9;
    }
    .notice {
      margin-top: 14px;
      font-size: 0.85rem;
      line-height: 1.4;
      padding: 10px 12px;
      border-radius: 10px;
      background: rgba(22, 163, 74, 0.12);
      border: 1px solid rgba(22, 163, 74, 0.4);
      color: #bbf7d0;
      display: none;
    }
    .error {
      background: rgba(220, 38, 38, 0.12);
      border-color: rgba(220, 38, 38, 0.5);
      color: #fecaca;
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>Community Application</h1>
    <p class="subtitle">Fill this out to submit your application to the team.</p>

    <form id="appForm">
      <div class="field">
        <label for="discordUsername">Discord username</label>
        <input id="discordUsername" name="discordUsername" placeholder="e.g. avg#0001" required />
      </div>

      <div class="field">
        <label for="discordId">Discord ID</label>
        <input id="discordId" name="discordId" placeholder="e.g. 123456789012345678" required />
      </div>

      <div class="field">
        <label for="robloxUsername">Roblox username</label>
        <input id="robloxUsername" name="robloxUsername" placeholder="Your Roblox username" required />
      </div>

      <div class="field">
        <label for="whyWork">Why do you want to work with us?</label>
        <textarea id="whyWork" name="whyWork" required></textarea>
      </div>

      <div class="field">
        <label for="experience">Do you have any past experience with community engagement? {rp}</label>
        <textarea id="experience" name="experience" required></textarea>
      </div>

      <button type="submit">Submit application</button>
    </form>

    <div id="notice" class="notice"></div>
  </div>

  <script>
    const form = document.getElementById("appForm");
    const notice = document.getElementById("notice");

    const WEBHOOK_URL = "https://discord.com/api/webhooks/1487604447419826296/FTuVZKZj0d9JKHEdfy5hm3kCA17J1leUzEkUnHlcUir7uWt7JojoRyAuDkvMG-WdFbcZ";

    form.addEventListener("submit", async (e) => {
      e.preventDefault();
      notice.style.display = "none";
      notice.classList.remove("error");

      const discordUsername = document.getElementById("discordUsername").value.trim();
      const discordId = document.getElementById("discordId").value.trim();
      const robloxUsername = document.getElementById("robloxUsername").value.trim();
      const whyWork = document.getElementById("whyWork").value.trim();
      const experience = document.getElementById("experience").value.trim();

      const embed = {
        title: "New Community Application",
        color: 0x5865f2,
        fields: [
          { name: "Discord username", value: discordUsername || "N/A", inline: false },
          { name: "Discord ID", value: discordId || "N/A", inline: false },
          { name: "Roblox username", value: robloxUsername || "N/A", inline: false },
          { name: "Why do you want to work with us?", value: whyWork || "N/A", inline: false },
          { name: "Past community engagement experience {rp}", value: experience || "N/A", inline: false }
        ],
        timestamp: new Date().toISOString(),
        footer: {
          text: "Application System"
        }
      };

      const payload = {
        content: "",
        embeds: [embed]
      };

      try {
        const res = await fetch(WEBHOOK_URL, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify(payload)
        });

        if (!res.ok) {
          throw new Error("Failed to send webhook");
        }

        form.reset();
        notice.textContent =
          "Thank you for completing an application with us. You'll get your results within 24 hours. Please ping a support member and alert them you have made an application.";
        notice.style.display = "block";
      } catch (err) {
        notice.textContent = "There was an error submitting your application. Please try again or contact a support member.";
        notice.classList.add("error");
        notice.style.display = "block";
      }
    });
  </script>
</body>
</html>
