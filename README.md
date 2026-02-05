<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pixel Art Studio</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #1a1a1a;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: #2d2d2d;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
            max-width: 900px;
            width: 100%;
            border: 2px solid #404040;
        }

        h1 {
            text-align: center;
            color: #ffffff;
            margin-bottom: 25px;
            font-size: 2em;
        }

        .tools {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
            align-items: center;
        }

        .tool-group {
            display: flex;
            gap: 10px;
            align-items: center;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            background: #667eea;
            color: white;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.3s;
            font-weight: 600;
        }

        button:hover {
            background: #5568d3;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
        }

        button.active {
            background: #764ba2;
        }

        .color-picker-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        input[type="color"] {
            width: 50px;
            height: 50px;
            border: 3px solid #667eea;
            border-radius: 8px;
            cursor: pointer;
        }

        .grid-size {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .grid-preset {
            padding: 8px 12px;
            font-size: 12px;
        }

        .grid-preset.active {
            background: #764ba2;
            box-shadow: 0 0 10px rgba(118, 75, 162, 0.5);
        }

        input[type="number"] {
            width: 60px;
            padding: 8px;
            border: 2px solid #667eea;
            border-radius: 8px;
            font-size: 14px;
            background: #3a3a3a;
            color: #ffffff;
        }

        .canvas-container {
            display: flex;
            justify-content: center;
            margin: 20px 0;
            background: #3a3a3a;
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #4a4a4a;
            overflow: auto;
            max-height: 600px;
        }

        #pixelCanvas {
            border: 3px solid #667eea;
            cursor: crosshair;
            image-rendering: pixelated;
            transition: transform 0.2s;
            transform-origin: center center;
        }

        .palette {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(40px, 1fr));
            gap: 8px;
            margin-bottom: 20px;
        }

        .palette-color {
            width: 40px;
            height: 40px;
            border-radius: 8px;
            cursor: pointer;
            border: 3px solid transparent;
            transition: all 0.2s;
        }

        .palette-color:hover {
            transform: scale(1.1);
        }

        .palette-color.selected {
            border-color: #667eea;
            box-shadow: 0 0 10px rgba(102, 126, 234, 0.8);
        }

        .export-buttons {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 20px;
        }

        .info {
            text-align: center;
            color: #cccccc;
            margin-top: 15px;
            font-size: 14px;
        }

        .color-modifier {
            background: #3a3a3a;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 20px;
            border: 1px solid #4a4a4a;
        }

        .color-modifier h3 {
            color: #ffffff;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        .slider-group {
            margin-bottom: 15px;
        }

        .slider-group label {
            display: block;
            margin-bottom: 5px;
            color: #cccccc;
            font-weight: 600;
            font-size: 14px;
        }

        .slider-container {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        input[type="range"] {
            flex: 1;
            height: 8px;
            border-radius: 5px;
            background: #4a4a4a;
            outline: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #667eea;
            cursor: pointer;
        }

        input[type="range"]::-moz-range-thumb {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: #667eea;
            cursor: pointer;
            border: none;
        }

        .slider-value {
            min-width: 45px;
            text-align: center;
            font-weight: 600;
            color: #667eea;
        }

        .color-preview {
            display: flex;
            gap: 15px;
            align-items: center;
            margin-top: 15px;
        }

        .color-box {
            width: 80px;
            height: 80px;
            border-radius: 8px;
            border: 3px solid #667eea;
        }

        .color-preview-label {
            font-size: 12px;
            color: #cccccc;
            text-align: center;
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎨 Pixel Art Studio</h1>
        
        <div class="tools">
            <div class="tool-group">
                <button id="penTool" class="active">✏️ Crayon</button>
                <button id="eraserTool">🧹 Gomme</button>
                <button id="fillTool">🪣 Remplir</button>
                <button id="eyedropperTool">💉 Pipette</button>
            </div>
            
            <div class="color-picker-container">
                <label>Couleur:</label>
                <input type="color" id="colorPicker" value="#000000">
            </div>
            
            <div class="grid-size">
                <label>Grille:</label>
                <button class="grid-preset" data-size="16">16x16</button>
                <button class="grid-preset" data-size="32">32x32</button>
                <button class="grid-preset" data-size="48">48x48</button>
                <button class="grid-preset" data-size="64">64x64</button>
                <input type="number" id="gridSize" min="8" max="128" value="32">
                <button id="applyGrid">Appliquer</button>
            </div>
            
            <button id="clearCanvas">🗑️ Effacer tout</button>
            
            <div class="tool-group">
                <button id="zoomOut">🔍−</button>
                <span id="zoomLevel" style="color: #cccccc; font-weight: 600;">100%</span>
                <button id="zoomIn">🔍+</button>
                <button id="zoomReset">↺ Reset</button>
            </div>
        </div>

        <div class="palette">
            <div class="palette-color selected" style="background-color: #000000;" data-color="#000000"></div>
            <div class="palette-color" style="background-color: #FFFFFF;" data-color="#FFFFFF"></div>
            <div class="palette-color" style="background-color: #FF0000;" data-color="#FF0000"></div>
            <div class="palette-color" style="background-color: #00FF00;" data-color="#00FF00"></div>
            <div class="palette-color" style="background-color: #0000FF;" data-color="#0000FF"></div>
            <div class="palette-color" style="background-color: #FFFF00;" data-color="#FFFF00"></div>
            <div class="palette-color" style="background-color: #FF00FF;" data-color="#FF00FF"></div>
            <div class="palette-color" style="background-color: #00FFFF;" data-color="#00FFFF"></div>
            <div class="palette-color" style="background-color: #FFA500;" data-color="#FFA500"></div>
            <div class="palette-color" style="background-color: #800080;" data-color="#800080"></div>
            <div class="palette-color" style="background-color: #FFC0CB;" data-color="#FFC0CB"></div>
            <div class="palette-color" style="background-color: #8B4513;" data-color="#8B4513"></div>
            <div class="palette-color" style="background-color: #808080;" data-color="#808080"></div>
            <div class="palette-color" style="background-color: #A52A2A;" data-color="#A52A2A"></div>
            <div class="palette-color" style="background-color: #90EE90;" data-color="#90EE90"></div>
            <div class="palette-color" style="background-color: #87CEEB;" data-color="#87CEEB"></div>
        </div>

        <div class="color-modifier">
            <h3>🎨 Modificateur de couleur</h3>
            
            <div class="slider-group">
                <label>Teinte (Hue)</label>
                <div class="slider-container">
                    <input type="range" id="hueSlider" min="0" max="360" value="0">
                    <span class="slider-value" id="hueValue">0°</span>
                </div>
            </div>
            
            <div class="slider-group">
                <label>Saturation</label>
                <div class="slider-container">
                    <input type="range" id="satSlider" min="0" max="100" value="100">
                    <span class="slider-value" id="satValue">100%</span>
                </div>
            </div>
            
            <div class="slider-group">
                <label>Luminosité</label>
                <div class="slider-container">
                    <input type="range" id="lightSlider" min="0" max="100" value="50">
                    <span class="slider-value" id="lightValue">50%</span>
                </div>
            </div>

            <div class="color-preview">
                <div>
                    <div class="color-box" id="originalColor"></div>
                    <div class="color-preview-label">Couleur originale</div>
                </div>
                <div>
                    <div class="color-box" id="modifiedColor"></div>
                    <div class="color-preview-label">Couleur modifiée</div>
                </div>
                <button id="applyModifiedColor">✓ Utiliser cette couleur</button>
            </div>
        </div>

        <div class="canvas-container">
            <canvas id="pixelCanvas" width="512" height="512"></canvas>
        </div>

        <div class="export-buttons">
            <button id="exportPNG">💾 Exporter PNG</button>
            <button id="exportScaled">📐 Exporter PNG (x8)</button>
        </div>

        <div class="info">
            💡 Cliquez pour dessiner | Maintenez Shift pour ligne droite
        </div>
    </div>

    <script>
        const canvas = document.getElementById('pixelCanvas');
        const ctx = canvas.getContext('2d');
        const colorPicker = document.getElementById('colorPicker');
        const gridSizeInput = document.getElementById('gridSize');
        
        let gridSize = 32;
        let pixelSize = canvas.width / gridSize;
        let currentColor = '#000000';
        let currentTool = 'pen';
        let isDrawing = false;
        let lastX = -1;
        let lastY = -1;
        let zoomLevel = 1;
        
        // Initialize grid
        let pixelGrid = [];
        
        function initGrid() {
            pixelGrid = Array(gridSize).fill(null).map(() => 
                Array(gridSize).fill('#FFFFFF')
            );
            drawGrid();
        }
        
        function drawGrid() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            for (let y = 0; y < gridSize; y++) {
                for (let x = 0; x < gridSize; x++) {
                    ctx.fillStyle = pixelGrid[y][x];
                    ctx.fillRect(
                        x * pixelSize,
                        y * pixelSize,
                        pixelSize,
                        pixelSize
                    );
                }
            }
            
            // Draw grid lines
            ctx.strokeStyle = '#555555';
            ctx.lineWidth = 1.5;
            for (let i = 0; i <= gridSize; i++) {
                ctx.beginPath();
                ctx.moveTo(i * pixelSize, 0);
                ctx.lineTo(i * pixelSize, canvas.height);
                ctx.stroke();
                
                ctx.beginPath();
                ctx.moveTo(0, i * pixelSize);
                ctx.lineTo(canvas.width, i * pixelSize);
                ctx.stroke();
            }
        }
        
        function getGridPosition(e) {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            const x = Math.floor((e.clientX - rect.left) * scaleX / pixelSize);
            const y = Math.floor((e.clientY - rect.top) * scaleY / pixelSize);
            return { x, y };
        }
        
        function setPixel(x, y, color) {
            if (x >= 0 && x < gridSize && y >= 0 && y < gridSize) {
                pixelGrid[y][x] = color;
                drawGrid();
            }
        }
        
        function floodFill(startX, startY, targetColor, fillColor) {
            if (targetColor === fillColor) return;
            
            const stack = [[startX, startY]];
            
            while (stack.length > 0) {
                const [x, y] = stack.pop();
                
                if (x < 0 || x >= gridSize || y < 0 || y >= gridSize) continue;
                if (pixelGrid[y][x] !== targetColor) continue;
                
                pixelGrid[y][x] = fillColor;
                
                stack.push([x + 1, y]);
                stack.push([x - 1, y]);
                stack.push([x, y + 1]);
                stack.push([x, y - 1]);
            }
            
            drawGrid();
        }
        
        function drawLine(x0, y0, x1, y1, color) {
            const dx = Math.abs(x1 - x0);
            const dy = Math.abs(y1 - y0);
            const sx = x0 < x1 ? 1 : -1;
            const sy = y0 < y1 ? 1 : -1;
            let err = dx - dy;
            
            while (true) {
                setPixel(x0, y0, color);
                
                if (x0 === x1 && y0 === y1) break;
                const e2 = 2 * err;
                if (e2 > -dy) {
                    err -= dy;
                    x0 += sx;
                }
                if (e2 < dx) {
                    err += dx;
                    y0 += sy;
                }
            }
        }
        
        canvas.addEventListener('mousedown', (e) => {
            isDrawing = true;
            const { x, y } = getGridPosition(e);
            
            if (currentTool === 'pen') {
                setPixel(x, y, currentColor);
                lastX = x;
                lastY = y;
            } else if (currentTool === 'eraser') {
                setPixel(x, y, '#FFFFFF');
                lastX = x;
                lastY = y;
            } else if (currentTool === 'fill') {
                const targetColor = pixelGrid[y][x];
                floodFill(x, y, targetColor, currentColor);
            } else if (currentTool === 'eyedropper') {
                currentColor = pixelGrid[y][x];
                colorPicker.value = currentColor;
                document.querySelectorAll('.palette-color').forEach(c => {
                    c.classList.toggle('selected', c.dataset.color === currentColor);
                });
            }
        });
        
        canvas.addEventListener('mousemove', (e) => {
            if (!isDrawing) return;
            if (currentTool !== 'pen' && currentTool !== 'eraser') return;
            
            const { x, y } = getGridPosition(e);
            const color = currentTool === 'pen' ? currentColor : '#FFFFFF';
            
            if (e.shiftKey && lastX !== -1 && lastY !== -1) {
                drawLine(lastX, lastY, x, y, color);
            } else {
                setPixel(x, y, color);
            }
            
            lastX = x;
            lastY = y;
        });
        
        canvas.addEventListener('mouseup', () => {
            isDrawing = false;
            lastX = -1;
            lastY = -1;
        });
        
        canvas.addEventListener('mouseleave', () => {
            isDrawing = false;
            lastX = -1;
            lastY = -1;
        });
        
        // Tool buttons
        document.getElementById('penTool').addEventListener('click', () => {
            currentTool = 'pen';
            document.querySelectorAll('.tools button').forEach(b => b.classList.remove('active'));
            document.getElementById('penTool').classList.add('active');
        });
        
        document.getElementById('eraserTool').addEventListener('click', () => {
            currentTool = 'eraser';
            document.querySelectorAll('.tools button').forEach(b => b.classList.remove('active'));
            document.getElementById('eraserTool').classList.add('active');
        });
        
        document.getElementById('fillTool').addEventListener('click', () => {
            currentTool = 'fill';
            document.querySelectorAll('.tools button').forEach(b => b.classList.remove('active'));
            document.getElementById('fillTool').classList.add('active');
        });
        
        document.getElementById('eyedropperTool').addEventListener('click', () => {
            currentTool = 'eyedropper';
            document.querySelectorAll('.tools button').forEach(b => b.classList.remove('active'));
            document.getElementById('eyedropperTool').classList.add('active');
        });
        
        
        document.getElementById('applyGrid').addEventListener('click', () => {
            const newSize = parseInt(gridSizeInput.value);
            if (newSize >= 8 && newSize <= 128) {
                gridSize = newSize;
                pixelSize = canvas.width / gridSize;
                initGrid();
                updateGridPresetButtons();
            }
        });

        // Grid preset buttons
        document.querySelectorAll('.grid-preset').forEach(btn => {
            btn.addEventListener('click', () => {
                const size = parseInt(btn.dataset.size);
                gridSizeInput.value = size;
                gridSize = size;
                pixelSize = canvas.width / gridSize;
                initGrid();
                updateGridPresetButtons();
            });
        });

        function updateGridPresetButtons() {
            document.querySelectorAll('.grid-preset').forEach(btn => {
                const size = parseInt(btn.dataset.size);
                btn.classList.toggle('active', size === gridSize);
            });
        }

        // Initialize preset buttons state
        updateGridPresetButtons();
        
        document.getElementById('clearCanvas').addEventListener('click', () => {
            if (confirm('Voulez-vous vraiment effacer tout le dessin ?')) {
                initGrid();
            }
        });

        // Zoom functions
        function updateZoom() {
            canvas.style.transform = `scale(${zoomLevel})`;
            document.getElementById('zoomLevel').textContent = Math.round(zoomLevel * 100) + '%';
        }

        document.getElementById('zoomIn').addEventListener('click', () => {
            if (zoomLevel < 3) {
                zoomLevel += 0.25;
                updateZoom();
            }
        });

        document.getElementById('zoomOut').addEventListener('click', () => {
            if (zoomLevel > 0.5) {
                zoomLevel -= 0.25;
                updateZoom();
            }
        });

        document.getElementById('zoomReset').addEventListener('click', () => {
            zoomLevel = 1;
            updateZoom();
        });
        
        document.getElementById('exportPNG').addEventListener('click', () => {
            const link = document.createElement('a');
            link.download = 'pixel-art.png';
            link.href = canvas.toDataURL();
            link.click();
        });
        
        document.getElementById('exportScaled').addEventListener('click', () => {
            const scale = 8;
            const scaledCanvas = document.createElement('canvas');
            scaledCanvas.width = canvas.width * scale;
            scaledCanvas.height = canvas.height * scale;
            const scaledCtx = scaledCanvas.getContext('2d');
            
            scaledCtx.imageSmoothingEnabled = false;
            scaledCtx.scale(scale, scale);
            scaledCtx.drawImage(canvas, 0, 0);
            
            const link = document.createElement('a');
            link.download = 'pixel-art-scaled.png';
            link.href = scaledCanvas.toDataURL();
            link.click();
        });
        
        // Initialize
        initGrid();

        // Color Modifier Functions
        function hexToHSL(hex) {
            let r = parseInt(hex.slice(1, 3), 16) / 255;
            let g = parseInt(hex.slice(3, 5), 16) / 255;
            let b = parseInt(hex.slice(5, 7), 16) / 255;

            let max = Math.max(r, g, b);
            let min = Math.min(r, g, b);
            let h, s, l = (max + min) / 2;

            if (max === min) {
                h = s = 0;
            } else {
                let d = max - min;
                s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
                
                switch (max) {
                    case r: h = ((g - b) / d + (g < b ? 6 : 0)) / 6; break;
                    case g: h = ((b - r) / d + 2) / 6; break;
                    case b: h = ((r - g) / d + 4) / 6; break;
                }
            }

            return {
                h: Math.round(h * 360),
                s: Math.round(s * 100),
                l: Math.round(l * 100)
            };
        }

        function hslToHex(h, s, l) {
            s /= 100;
            l /= 100;

            let c = (1 - Math.abs(2 * l - 1)) * s;
            let x = c * (1 - Math.abs((h / 60) % 2 - 1));
            let m = l - c / 2;
            let r = 0, g = 0, b = 0;

            if (0 <= h && h < 60) {
                r = c; g = x; b = 0;
            } else if (60 <= h && h < 120) {
                r = x; g = c; b = 0;
            } else if (120 <= h && h < 180) {
                r = 0; g = c; b = x;
            } else if (180 <= h && h < 240) {
                r = 0; g = x; b = c;
            } else if (240 <= h && h < 300) {
                r = x; g = 0; b = c;
            } else if (300 <= h && h < 360) {
                r = c; g = 0; b = x;
            }

            r = Math.round((r + m) * 255);
            g = Math.round((g + m) * 255);
            b = Math.round((b + m) * 255);

            return '#' + [r, g, b].map(x => {
                const hex = x.toString(16);
                return hex.length === 1 ? '0' + hex : hex;
            }).join('');
        }

        const hueSlider = document.getElementById('hueSlider');
        const satSlider = document.getElementById('satSlider');
        const lightSlider = document.getElementById('lightSlider');
        const hueValue = document.getElementById('hueValue');
        const satValue = document.getElementById('satValue');
        const lightValue = document.getElementById('lightValue');
        const originalColorBox = document.getElementById('originalColor');
        const modifiedColorBox = document.getElementById('modifiedColor');

        function updateColorModifier() {
            const hsl = hexToHSL(currentColor);
            hueSlider.value = hsl.h;
            satSlider.value = hsl.s;
            lightSlider.value = hsl.l;
            
            hueValue.textContent = hsl.h + '°';
            satValue.textContent = hsl.s + '%';
            lightValue.textContent = hsl.l + '%';
            
            originalColorBox.style.backgroundColor = currentColor;
            updateModifiedColor();
        }

        function updateModifiedColor() {
            const h = parseInt(hueSlider.value);
            const s = parseInt(satSlider.value);
            const l = parseInt(lightSlider.value);
            
            const modifiedHex = hslToHex(h, s, l);
            modifiedColorBox.style.backgroundColor = modifiedHex;
            
            hueValue.textContent = h + '°';
            satValue.textContent = s + '%';
            lightValue.textContent = l + '%';
        }

        hueSlider.addEventListener('input', updateModifiedColor);
        satSlider.addEventListener('input', updateModifiedColor);
        lightSlider.addEventListener('input', updateModifiedColor);

        document.getElementById('applyModifiedColor').addEventListener('click', () => {
            const h = parseInt(hueSlider.value);
            const s = parseInt(satSlider.value);
            const l = parseInt(lightSlider.value);
            
            currentColor = hslToHex(h, s, l);
            colorPicker.value = currentColor;
            
            document.querySelectorAll('.palette-color').forEach(c => {
                c.classList.remove('selected');
            });
            
            updateColorModifier();
        });

        // Update color modifier when color changes
        colorPicker.addEventListener('input', (e) => {
            currentColor = e.target.value;
            document.querySelectorAll('.palette-color').forEach(c => {
                c.classList.remove('selected');
            });
            updateColorModifier();
        });

        document.querySelectorAll('.palette-color').forEach(colorDiv => {
            colorDiv.addEventListener('click', () => {
                currentColor = colorDiv.dataset.color;
                colorPicker.value = currentColor;
                document.querySelectorAll('.palette-color').forEach(c => {
                    c.classList.remove('selected');
                });
                colorDiv.classList.add('selected');
                updateColorModifier();
            });
        });

        // Initialize color modifier
        updateColorModifier();
    </script>
</body>
</html>
