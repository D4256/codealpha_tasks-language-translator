# codealpha_tasks 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Microsoft Translator - Free Version</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      text-align: center;
      padding: 20px;
    }
    .container {
      background: #fff;
      padding: 20px;
      margin: auto;
      width: 90%;
      max-width: 500px;
      box-shadow: 0 0 10px #aaa;
      border-radius: 10px;
    }
    textarea {
      width: 100%;
      height: 100px;
      margin: 10px 0;
      font-size: 16px;
    }
    select, button {
      margin: 5px;
      padding: 8px 12px;
      font-size: 14px;
    }
    #translatedOutput {
      background-color: #f0f0f0;
      padding: 10px;
      margin-top: 10px;
      border-radius: 6px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>🌐 Microsoft Translator Tool (Free API)</h2>

    <textarea id="inputText" placeholder="Enter text here..."></textarea><br>

    <label for="targetLang">To:</label>
    <select id="targetLang">
      <option value="hi">Hindi</option>
      <option value="en">English</option>
      <option value="fr">French</option>
      <option value="es">Spanish</option>
      <option value="de">German</option>
    </select>

    <button onclick="translateText()">Translate</button>

    <h3>Translated Text:</h3>
    <p id="translatedOutput"></p>

    <button onclick="copyText()">📋 Copy</button>
    <button onclick="speak()">🔊 Speak</button>
  </div>

  <script>
    const subscriptionKey = "YOUR_API_KEY";  // Replace with your key
    const endpoint = "https://api.cognitive.microsofttranslator.com";  // Default endpoint
    const region = "YOUR_REGION";  // e.g., centralindia

    async function translateText() {
      const text = document.getElementById("inputText").value;
      const target = document.getElementById("targetLang").value;

      if (!text.trim()) {
        alert("Please enter some text.");
        return;
      }

      try {
        const response = await fetch(`${endpoint}/translate?api-version=3.0&to=${target}`, {
          method: "POST",
          headers: {
            "Ocp-Apim-Subscription-Key": subscriptionKey,
            "Ocp-Apim-Subscription-Region": region,
            "Content-type": "application/json"
          },
          body: JSON.stringify([{ Text: text }])
        });

        const result = await response.json();
        document.getElementById("translatedOutput").innerText = result[0].translations[0].text;
      } catch (error) {
        console.error("Error:", error);
        document.getElementById("translatedOutput").innerText = "Translation failed.";
      }
    }

    function copyText() {
      const text = document.getElementById("translatedOutput").innerText;
      if (text) {
        navigator.clipboard.writeText(text).then(() => {
          alert("Copied to clipboard!");
        });
      }
    }

    function speak() {
      const text = document.getElementById("translatedOutput").innerText;
      if (text) {
        const utterance = new SpeechSynthesisUtterance(text);
        window.speechSynthesis.speak(utterance);
      }
    }
  </script>
</body>
</html>
