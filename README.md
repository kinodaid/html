<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>بوت عرض الصور</title>
<style>
  body {
    font-family: Arial, sans-serif;
    max-width: 700px;
    margin: 20px auto;
    padding: 10px;
    background: #f9f9f9;
    text-align: center;
  }
  button {
    margin: 8px;
    padding: 12px 24px;
    font-size: 18px;
    cursor: pointer;
    border: none;
    background-color: #007bff;
    color: white;
    border-radius: 6px;
    transition: background-color 0.3s;
  }
  button:hover {
    background-color: #0056b3;
  }
  #images-container img {
    max-width: 100%;
    margin: 12px 0;
    border-radius: 10px;
  }
  #message {
    color: red;
    margin-top: 15px;
    font-weight: bold;
  }
</style>
</head>
<body>

<h1>بوت عرض الصور</h1>
<p>اضغط على الزر لتحميل 20 صورة عشوائية</p>

<button onclick="loadImages('sfw')">🎴 صور SFW</button>
<button onclick="loadImages('nsfw')">🍑 صور Hentai</button>

<div id="message"></div>
<div id="images-container"></div>

<script>
async function loadImages(kind) {
  const container = document.getElementById('images-container');
  const message = document.getElementById('message');
  container.innerHTML = "";
  message.textContent = "... جاري تحميل الصور ...";

  const urls = new Set();
  let tries = 0;
  const maxImages = 20;
  const maxTries = 100;

  while (urls.size < maxImages && tries < maxTries) {
    tries++;
    try {
      const res = await fetch(`https://api.waifu.pics/${kind}/waifu`);
      const data = await res.json();
      if (data.url && !urls.has(data.url)) {
        urls.add(data.url);
      }
    } catch (e) {
      // تجاهل الأخطاء واستمر
    }
  }

  if (urls.size === 0) {
    message.textContent = "❌ لم نتمكن من تحميل الصور. حاول مرة أخرى.";
    return;
  }

  message.textContent = "";
  urls.forEach(url => {
    const img = document.createElement('img');
    img.src = url;
    img.alt = kind + " image";
    container.appendChild(img);
  });
}
</script>

</body>
</html>
