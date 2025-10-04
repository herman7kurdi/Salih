```html name=index.html
<!DOCTYPE html>
<html lang="ku" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نالى</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f5f5f5;
      padding: 10px;
      color: #333;
      margin: 0;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .container {
      width: 95%;
      max-width: 600px;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
      overflow-y: auto;
      max-height: 90vh;
    }
    input {
      width: 100%;
      padding: 12px;
      margin: 8px 0;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
      box-sizing: border-box;
    }
    button {
      padding: 12px;
      margin: 8px 0;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-weight: bold;
      font-size: 17px;
      width: 100%;
    }
    .add {
      background: #28a745;
      color: white;
    }
    .calc {
      background: #007bff;
      color: white;
      font-size: 18px;
      padding: 14px;
    }
    .del {
      background: #dc3545;
      color: white;
      font-size: 14px;
      padding: 6px 12px;
    }
    .room {
      background: #f9f9f9;
      padding: 15px;
      margin: 15px 0;
      border-radius: 10px;
      border: 1px solid #eee;
    }
    .result {
      margin-top: 20px;
      padding: 15px;
      background: #e9f7ef;
      border-radius: 10px;
      font-weight: bold;
      white-space: pre-line;
      font-size: 16px;
      line-height: 1.6;
    }
    .header {
      text-align: center;
      margin-bottom: 20px;
      padding-bottom: 15px;
      border-bottom: 2px solid #eee;
      color: #555;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h2>نالى</h2>
    </div>
    <div id="rooms"></div>
    <button class="add" onclick="addRoom()">+ زیادکردنی ژووری نوێ</button>
    <button class="calc" onclick="calc()">کۆی گشتی (د.ع)</button>
    <div class="result" id="result">ئەنجام لێرە دەردەکەوێت</div>
  </div>
  <script>
    let roomId = 0;
    function addRoom() {
      roomId++;
      let room = document.createElement('div');
      room.className = 'room';
      room.dataset.rid = roomId;
      room.innerHTML = `
        <div>ژووری</div>
        <input type="number" id="l${roomId}" placeholder="دراژی (م)" step="0.01" min="0">
        <input type="number" id="w${roomId}" placeholder="پانی (م)" step="0.01" min="0">
        <input type="number" id="p${roomId}" placeholder="نرخی مۆنکێت (د.ع)" step="1" min="0">
        <input type="number" id="eq${roomId}" placeholder="ژمارەی مەحفەر" value="0" min="0">
        <input type="number" id="ep${roomId}" placeholder="نرخی مەحفەر (د.ع)" value="0" step="1" min="0">
        <button class="del" onclick="removeRoom(this)">لابردن</button>
      `;
      document.getElementById('rooms').appendChild(room);
      updateNumbers();
    }
    function removeRoom(btn) {
      btn.parentElement.remove();
      updateNumbers();
    }
    function updateNumbers() {
      let rooms = document.querySelectorAll('.room');
      for (let i = 0; i < rooms.length; i++) {
        rooms[i].children[0].innerText = 'ژووری ' + (i + 1);
      }
    }
    function calc() {
      let rooms = document.querySelectorAll('.room');
      if (rooms.length === 0) {
        document.getElementById('result').innerText = 'هیچ ژوورێک نییە!';
        return;
      }
      let totalAll = 0;
      let msg = '';
      let ok = true;
      rooms.forEach((room, idx) => {
        const rid = room.dataset.rid;
        let l = parseFloat(document.getElementById('l' + rid)?.value) || 0;
        let w = parseFloat(document.getElementById('w' + rid)?.value) || 0;
        let p = parseFloat(document.getElementById('p' + rid)?.value) || 0;
        let eq = parseFloat(document.getElementById('eq' + rid)?.value) || 0;
        let ep = parseFloat(document.getElementById('ep' + rid)?.value) || 0;
        if (l <= 0 || w <= 0 || p <= 0) {
          document.getElementById('result').innerText = 'زانیاری ژووری ' + (idx + 1) + ' ناتەواوە!';
          ok = false;
          return;
        }
        let area = l * w;
        let monket = area * p;
        let mehfer = eq * ep;
        let total = monket + mehfer;
        totalAll += total;
        msg += 'ژووری ' + (idx + 1) + ':\n';
        msg += ' دراژی: ' + l + ' م × پانی: ' + w + ' م\n';
        msg += ' ڕووبەر: ' + area.toFixed(2) + ' م²\n';
        msg += ' مۆنکێت: ' + monket.toLocaleString() + ' د.ع\n';
        if (mehfer > 0) {
          msg += ' مەحفەر: ' + mehfer.toLocaleString() + ' د.ع\n';
        }
        msg += ' کۆی ئەم ژوورە: ' + total.toLocaleString() + ' د.ع\n\n';
      });
      if (ok) {
        msg += 'کۆی گشتی: ' + totalAll.toLocaleString() + ' د.ع';
        document.getElementById('result').innerText = msg;
      }
    }
    window.onload = function() { addRoom(); };
  </script>
</body>
</html>
```
