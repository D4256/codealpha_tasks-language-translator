# codealpha_tasks 
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Language Translator</title>
  <style>
    textarea {
      width: 300px;
      height: 100px;
    }
  </style>
</head>
<body>
  <h2>Language Translator Tool</h2>

  <label for="inputText">Enter Text:</label><br>
  <textarea id="inputText" placeholder="Enter text here..."></textarea><br><br>

  <label for="sourceLang">From:</label>
  <select id="sourceLang">
    <option value="en">English</option>
    <option value="hi">Hindi</option>
    <option value="es">Spanish</option>
    <option value="fr">French</option>
    <option value="de">German</option>
    <option value="zh">Chinese</option>
  </select>

  <label for="targetLang">To:</label>
  <select id="targetLang">
    <option value="hi">Hindi</option>
    <option value="en">English</option>
    <option value="es">Spanish</option>
    <option value="fr">French</option>
    <option value="de">German</option>
    <option value="zh">Chinese</option>
  </select>

  <br><br>
  <button onclick="translateText()">Translate</button>
  <button onclick="copyText()">Copy</button>
  <button onclick="speak()">Speak</button>

  <p><strong>Translated Text:</strong></p>
  <p id="translatedOutput"></p>

  <script>
    let voices = [];

    // Load voices for speech synthesis
    window.speechSynthesis.onvoiceschanged = () => {
      voices = window.speechSynthesis.getVoices();
    };

    async function translateText() {
      const text = document.getElementById("inputText").value.trim();
      const source = document.getElementById("sourceLang").value;
      const target = document.getElementById("targetLang").value;

      if (!text) {
        alert("Please enter some text to translate.");
        return;
      }

      if (source === target) {
        alert("Source and target languages must be different.");
        return;
      }

      try {
        const response = await fetch https:api.mymemory.translated.net/get?q=${text}&langpair=${translateFrom}|${translateTo};
{
          method: "POST",
          body: JSON.stringify({
            q: text,
            source: source,
            target: target,
            format: "text"
          }),
          headers: { "Content-Type": "application/json" }
        });

        const data = await response.json();
        document.getElementById("translatedOutput").innerText = data.translatedText;
      } catch (error) {
        console.error("Translation Error:", error);
        alert("Failed to translate. Please check your network or try again later.");
      }
    }

    function copyText() {
      const text = document.getElementById("translatedOutput").innerText;
      if (!text) {
        alert("Nothing to copy.");
        return;
      }

      navigator.clipboard.writeText(text).then(() => {
        alert("Copied to clipboard!");
      });
    }

    function speak() {
      const text = document.getElementById("translatedOutput").innerText;
      const target = document.getElementById("targetLang").value;

      if (!text) {
        alert("Please translate something first!");
        return;
      }

      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = target;

      const matchingVoice = voices.find(v => v.lang.startsWith(target));
      if (matchingVoice) {
        utterance.voice = matchingVoice;
      }

      window.speechSynthesis.speak(utterance);
    }
  </script>
</body>
</html>
