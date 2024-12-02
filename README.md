<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>تطبيق إنشاء ملصقات</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Amiri&family=Cairo&family=Changa&family=El+Messiri&family=Lemonada&family=Markazi+Text&family=Noto+Naskh+Arabic&family=Tajawal&family=Reem+Kufi&family=Scheherazade+New&display=swap');

    body {
      font-family: 'Cairo', sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
      background-color: #f4f4f4;
    }

    h1 {
      margin: 20px 0;
      color: #333;
      font-size: 1.8rem;
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
    }

    .canvas-container {
      margin: 15px 0;
      border: 2px solid #ccc;
      background-color: #fff;
      border-radius: 10px;
      overflow: hidden;
    }

    canvas {
      display: block;
      max-width: 100%;
      height: auto;
    }

    .controls {
      display: flex;
      flex-direction: column;
      align-items: stretch;
      gap: 10px;
      width: 100%;
      max-width: 400px;
    }

    input, button, select {
      padding: 10px;
      margin: 0;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1rem;
      width: 100%;
      box-sizing: border-box;
    }

    button {
      background-color: #007BFF;
      color: #fff;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #0056b3;
    }

    @media (max-width: 600px) {
      h1 {
        font-size: 1.5rem;
      }

      .canvas-container {
        width: 90%;
      }

      .controls {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>برنامج إنشاء الملصقات</h1>
    <div class="canvas-container">
      <canvas id="stickerCanvas" width="300" height="300"></canvas>
    </div>
    <div class="controls">
      <label>إضافة نص:</label>
      <input type="text" id="stickerText" placeholder="أدخل النص هنا">
      <label>حجم الخط:</label>
      <input type="number" id="fontSize" value="20" min="10" max="100">
      <label>لون النص:</label>
      <input type="color" id="textColor" value="#000000">
      <label>اختر الخط:</label>
      <select id="fontFamily">
        <option value="Cairo">Cairo</option>
        <option value="Amiri">Amiri</option>
        <option value="Changa">Changa</option>
        <option value="El Messiri">El Messiri</option>
        <option value="Lemonada">Lemonada</option>
        <option value="Markazi Text">Markazi Text</option>
        <option value="Noto Naskh Arabic">Noto Naskh Arabic</option>
        <option value="Tajawal">Tajawal</option>
        <option value="Reem Kufi">Reem Kufi</option>
        <option value="Scheherazade New">Scheherazade New</option>
      </select>
      <button id="addText">إضافة النص</button>
      <label>إضافة صورة:</label>
      <input type="file" id="uploadImage" accept="image/*">
      <button id="downloadSticker">تحميل الملصق</button>
    </div>
  </div>

  <script>
    const canvas = document.getElementById('stickerCanvas');
    const ctx = canvas.getContext('2d');
    const stickerText = document.getElementById('stickerText');
    const fontSize = document.getElementById('fontSize');
    const textColor = document.getElementById('textColor');
    const fontFamily = document.getElementById('fontFamily');
    const addTextBtn = document.getElementById('addText');
    const uploadImage = document.getElementById('uploadImage');
    const downloadSticker = document.getElementById('downloadSticker');

    // إعداد الخلفية
    ctx.fillStyle = '#ffffff';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    // إضافة نص إلى اللوحة
    addTextBtn.addEventListener('click', () => {
      const text = stickerText.value;
      const size = fontSize.value;
      const color = textColor.value;
      const font = fontFamily.value;

      ctx.font = `${size}px '${font}'`;
      ctx.fillStyle = color;
      ctx.textAlign = 'center';
      ctx.fillText(text, canvas.width / 2, canvas.height / 2);
    });

    // تحميل صورة إلى اللوحة
    uploadImage.addEventListener('change', (event) => {
      const file = event.target.files[0];
      if (file) {
        const img = new Image();
        img.onload = () => {
          ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        };
        img.src = URL.createObjectURL(file);
      }
    });

    // تحميل الملصق
    downloadSticker.addEventListener('click', () => {
      const link = document.createElement('a');
      link.download = 'sticker.png';
      link.href = canvas.toDataURL();
      link.click();
    });
  </script>
</body>
</html>
