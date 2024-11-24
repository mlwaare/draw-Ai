<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>أداة الرسم التفاعلية</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #222;
            color: #fff;
            text-align: center;
            margin: 0;
            padding: 0;
        }
        canvas {
            border: 2px solid #fff;
            margin-top: 20px;
            cursor: crosshair;
        }
        .toolbar {
            margin: 10px;
        }
        .toolbar button, .toolbar input {
            padding: 10px;
            margin: 5px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .toolbar button {
            background-color: #007BFF;
            color: #fff;
        }
        .toolbar button:hover {
            background-color: #0056b3;
        }
        .toolbar input[type="color"] {
            cursor: pointer;
        }
        .toolbar input[type="range"] {
            width: 150px;
        }
    </style>
</head>
<body>
    <h1>أداة الرسم التفاعلية</h1>
    <div class="toolbar">
        <button id="clear">مسح الشاشة</button>
        <button id="save">حفظ الصورة</button>
        <input type="color" id="colorPicker" value="#ffffff">
        <input type="range" id="brushSize" min="1" max="20" value="5">
    </div>
    <canvas id="drawingCanvas" width="800" height="500"></canvas>

    <script>
        const canvas = document.getElementById('drawingCanvas');
        const ctx = canvas.getContext('2d');
        const clearButton = document.getElementById('clear');
        const saveButton = document.getElementById('save');
        const colorPicker = document.getElementById('colorPicker');
        const brushSize = document.getElementById('brushSize');

        let drawing = false;
        let currentColor = '#ffffff';
        let currentBrushSize = 5;

        // تحديث اللون
        colorPicker.addEventListener('input', (e) => {
            currentColor = e.target.value;
        });

        // تحديث حجم الفرشاة
        brushSize.addEventListener('input', (e) => {
            currentBrushSize = e.target.value;
        });

        // بدء الرسم
        canvas.addEventListener('mousedown', () => drawing = true);
        canvas.addEventListener('mouseup', () => drawing = false);
        canvas.addEventListener('mousemove', draw);

        // مسح الشاشة
        clearButton.addEventListener('click', () => {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        });

        // حفظ الصورة
        saveButton.addEventListener('click', () => {
            const link = document.createElement('a');
            link.download = 'my-drawing.png';
            link.href = canvas.toDataURL();
            link.click();
        });

        // وظيفة الرسم
        function draw(e) {
            if (!drawing) return;
            const rect = canvas.getBoundingClientRect();
            const x = e.clientX - rect.left;
            const y = e.clientY - rect.top;

            ctx.fillStyle = currentColor;
            ctx.beginPath();
            ctx.arc(x, y, currentBrushSize / 2, 0, Math.PI * 2);
            ctx.fill();
        }
    </script>
</body>
</html>
