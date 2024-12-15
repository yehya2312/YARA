project/
|-- index.html        (واجهة المستخدم الرئيسية)
|-- styles.css        (ملف تنسيقات CSS)
|-- scripts.js        (وظائف JavaScript)
|-- videos/           (مجلد فيديوهات لغة الإشارة)
|-- assets/           (أيقونات أو صور إضافية)
|-- README.md
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Yara Translator</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1>Yara Sign Language Translator</h1>
  <select id="language-select">
    <option value="arabic">Arabic</option>
    <option value="english">English</option>
    <option value="french">French</option>
    <option value="chinese">Chinese</option>
    <option value="russian">Russian</option>
  </select>
  <textarea id="text-input" placeholder="Enter text here"></textarea>
  <button id="translate-btn">Translate</button>
  <button id="audio-btn">Translate Audio</button>
  <div id="video-output"></div>
  <script src="scripts.js"></script>
</body>
</html>
document.getElementById('translate-btn').addEventListener('click', () => {
  const text = document.getElementById('text-input').value;
  const language = document.getElementById('language-select').value;

  const videoLibrary = {
    arabic: { eat: 'videos/arabic/eat.mp4', drink: 'videos/arabic/drink.mp4' },
    english: { eat: 'videos/english/eat.mp4', drink: 'videos/english/drink.mp4' },
  };

  const videoPath = videoLibrary[language]?.[text.toLowerCase()];
  if (videoPath) {
    document.getElementById('video-output').innerHTML = `<video src="${videoPath}" controls autoplay></video>`;
  } else {
    alert('Video not available for this text.');
  }
});

// Implementing Web Speech API for audio input
document.getElementById('audio-btn').addEventListener('click', () => {
  const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
  recognition.lang = document.getElementById('language-select').value === 'arabic' ? 'ar-SA' : 'en-US';

  recognition.onresult = (event) => {
    const text = event.results[0][0].transcript;
    document.getElementById('text-input').value = text;
    alert(`Recognized text: ${text}`);
  };

  recognition.onerror = (event) => alert('Error recognizing speech: ' + event.error);

  recognition.start();
});
