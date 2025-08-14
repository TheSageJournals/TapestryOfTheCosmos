<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tapestry of the Cosmos</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: url('https://source.unsplash.com/random/1920x1080?cosmos') no-repeat center/cover;
            color: #fff;
            margin: 0;
            padding: 10px;
            overflow-x: hidden;
            touch-action: manipulation;
        }
        .container {
            max-width: 100%;
            margin: auto;
            background: rgba(0, 0, 0, 0.8);
            border-radius: 10px;
            padding: 15px;
        }
        h1 {
            text-align: center;
            color: #00FFFF;
            font-size: 1.5rem;
        }
        .tab-buttons {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 15px;
        }
        .tab-button {
            padding: 8px 15px;
            margin: 5px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.9rem;
            touch-action: manipulation;
        }
        .tab-button.active {
            background: #00FFFF;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        canvas {
            border: 1px solid #fff;
            background: #fff;
            margin: 10px auto;
            display: block;
            max-width: 90vw;
            max-height: 90vw;
        }
        .plotly-container {
            max-width: 90vw;
            max-height: 90vw;
            margin: 10px auto;
        }
        .controls {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            margin-bottom: 15px;
        }
        .input-group {
            margin: 5px;
            font-size: 0.9rem;
        }
        .color-box {
            display: inline-block;
            width: 12px;
            height: 12px;
            margin: 2px;
            vertical-align: middle;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(72, 8px);
            gap: 1px;
            max-width: 100%;
            overflow-x: auto;
        }
        .row-container {
            display: flex;
            align-items: center;
            margin-bottom: 2px;
        }
        .row-label {
            width: 40px;
            font-weight: bold;
            color: #fff;
            font-size: 0.8rem;
        }
        .tooltip {
            position: absolute;
            background: rgba(0, 0, 0, 0.8);
            color: #fff;
            padding: 5px;
            border-radius: 3px;
            pointer-events: none;
            font-size: 0.8rem;
            z-index: 100;
        }
        #loading {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        .loading-bar {
            width: 150px;
            height: 15px;
            background: #ddd;
            border-radius: 10px;
            overflow: hidden;
        }
        .loading-progress {
            width: 0;
            height: 100%;
            background: #00FFFF;
            animation: loading 2s infinite;
        }
        @keyframes loading {
            0% { width: 0; }
            50% { width: 100%; }
            100% { width: 0; }
        }
        .legend {
            margin: 10px 0;
            font-size: 0.9rem;
        }
        .legend ul {
            list-style: none;
            padding: 0;
        }
        @media (max-width: 414px) {
            canvas, .plotly-container {
                width: 100% !important;
                height: auto !important;
            }
            .grid {
                grid-template-columns: repeat(72, 6px);
            }
            .tab-button {
                padding: 6px 12px;
                font-size: 0.8rem;
            }
            .input-group {
                flex: 1 1 100%;
            }
            .btn-group {
                flex-direction: column;
                gap: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Tapestry of the Cosmos</h1>
        <div id="warning" class="alert alert-warning text-center">
            <p><strong>Warning:</strong> This app may blow your device but will certainly blow your mind! Use at self risk.</p>
            <div class="btn-group mt-2">
                <button class="btn btn-primary" onclick="proceed(true)">Yes</button>
                <button class="btn btn-danger" onclick="proceed(false)">No</button>
            </div>
        </div>
        <div id="main" style="display: none;">
            <div class="tab-buttons">
                <button class="tab-button active" onclick="openTab('gridVisualizer')">Grid Visualizer</button>
                <button class="tab-button" onclick="openTab('primeSieve')">Prime Sieve</button>
                <button class="tab-button" onclick="openTab('fibonacciSong')">Fibonacci Song</button>
                <button class="tab-button" onclick="openTab('cymatics')">Cymatics</button>
                <button class="tab-button" onclick="openTab('cymaticSymphony')">Cymatic Symphony</button>
                <button class="tab-button" onclick="openTab('about')">About</button>
            </div>
            <div id="gridVisualizer" class="tab-content active">
                <div class="controls">
                    <div class="input-group">
                        <label>Grid Size:</label>
                        <select id="gridSize" class="form-select" onchange="updateGrid()">
                            <option value="9x24">9x24</option>
                            <option value="72x72" selected>72x72</option>
                        </select>
                    </div>
                    <div class="input-group">
                        <label>Sectors:</label>
                        <div id="sectorCheckboxes"></div>
                    </div>
                    <div class="input-group">
                        <label>Digital Roots:</label>
                        <div id="rootCheckboxes"></div>
                    </div>
                    <div class="input-group">
                        <label>Column Range:</label>
                        <input type="number" id="startCol" class="form-control" placeholder="Start" min="1" value="1">
                        <input type="number" id="endCol" class="form-control" placeholder="End" min="1">
                    </div>
                    <div class="input-group">
                        <label>Show Primes:</label>
                        <input type="checkbox" id="showPrimes" onchange="cachePoints()">
                    </div>
                    <button class="btn btn-primary" onclick="updateGrid()">Update</button>
                    <button class="btn btn-primary" onclick="exportCanvas()">Export Canvas</button>
                </div>
                <div class="legend">
                    <strong>Digital Roots:</strong>
                    <ul>
                        <li><span class="color-box" style="background: #00FFFF"></span> 1: Cyan</li>
                        <li><span class="color-box" style="background: #00FF00"></span> 2: Green</li>
                        <li><span class="color-box" style="background: #FFA500"></span> 3: Orange</li>
                        <li><span class="color-box" style="background: #FF69B4"></span> 4: Pink</li>
                        <li><span class="color-box" style="background: #228B22"></span> 5: Forest Green</li>
                        <li><span class="color-box" style="background: #FFFF00"></span> 6: Yellow</li>
                        <li><span class="color-box" style="background: #800080"></span> 7: Purple</li>
                        <li><span class="color-box" style="background: #808080"></span> 8: Gray</li>
                        <li><span class="color-box" style="background: #FF0000"></span> 9: Red</li>
                    </ul>
                    <p id="rootCounts"></p>
                </div>
                <canvas id="gridCanvas" width="720" height="720"></canvas>
                <div id="plotlyInteractive" class="plotly-container"></div>
                <div id="plotlyToroidal" class="plotly-container"></div>
                <div id="plotlyGlobe" class="plotly-container"></div>
                <div id="plotlyMobius" class="plotly-container"></div>
                <div id="plotlyCylinder" class="plotly-container"></div>
                <div id="plotlyTrends" class="plotly-container"></div>
                <div id="plotlyPolar" class="plotly-container"></div>
            </div>
            <div id="primeSieve" class="tab-content">
                <div class="controls">
                    <div class="input-group">
                        <label for="numberInput">Number:</label>
                        <input type="number" id="numberInput" class="form-control" placeholder="Enter a number" min="0">
                    </div>
                    <div class="input-group">
                        <label for="startInput">Start:</label>
                        <input type="number" id="startInput" class="form-control" placeholder="Enter start" min="0">
                    </div>
                    <div class="input-group">
                        <label for="endInput">End:</label>
                        <input type="number" id="endInput" class="form-control" placeholder="Enter end" min="0">
                    </div>
                    <button class="btn btn-primary" onclick="checkPrime()">Check Prime</button>
                    <button class="btn btn-primary" onclick="findPrimes()">Find Primes</button>
                    <button class="btn btn-primary" onclick="findNextPrime()">Next Prime</button>
                    <button class="btn btn-primary" id="mersenneButton" onclick="checkMersenne()" disabled>Check Mersenne</button>
                    <button class="btn btn-danger" onclick="panicStop()">Panic Stop</button>
                </div>
                <div id="primeOutput" class="alert alert-info"></div>
                <div id="plotPrime" class="plotly-container"></div>
                <div id="plotDigitalRoot" class="plotly-container"></div>
                <div id="plotBinary" class="plotly-container"></div>
            </div>
            <div id="fibonacciSong" class="tab-content">
                <div class="controls">
                    <label for="patternSelect">Playback Pattern:</label>
                    <select id="patternSelect" class="form-select">
                        <option value="row-wise">Left-to-Right, Downward</option>
                        <option value="column-wise">Top-to-Bottom, Left-to-Right</option>
                        <option value="both">Both Overlap</option>
                    </select>
                    <button class="btn btn-primary" onclick="playSong()">Play Song</button>
                    <button class="btn btn-primary" onclick="stopSong()">Stop Song</button>
                </div>
                <div class="legend">
                    <strong>Digital Roots & Notes:</strong>
                    <ul>
                        <li><span class="color-box" style="background: #00FFFF"></span> 1: Cyan (432 Hz)</li>
                        <li><span class="color-box" style="background: #00FF00"></span> 2: Green (432 Hz)</li>
                        <li><span class="color-box" style="background: #FFA500"></span> 3: Orange (720 Hz)</li>
                        <li><span class="color-box" style="background: #FF69B4"></span> 4: Pink (432 Hz)</li>
                        <li><span class="color-box" style="background: #228B22"></span> 5: Forest Green (432 Hz)</li>
                        <li><span class="color-box" style="background: #FFFF00"></span> 6: Yellow (720 Hz)</li>
                        <li><span class="color-box" style="background: #800080"></span> 7: Purple (432 Hz)</li>
                        <li><span class="color-box" style="background: #808080"></span> 8: Gray (432 Hz)</li>
                        <li><span class="color-box" style="background: #FF0000"></span> 9: Red (1152 Hz)</li>
                    </ul>
                    <p id="songRootCounts"></p>
                </div>
                <div id="songGrid"></div>
            </div>
            <div id="cymatics" class="tab-content">
                <div class="controls">
                    <div class="input-group">
                        <label for="freq1">Freq 1 (Hz):</label>
                        <input type="number" id="freq1" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq2">Freq 2 (Hz):</label>
                        <input type="number" id="freq2" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq3">Freq 3 (Hz):</label>
                        <input type="number" id="freq3" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq4">Freq 4 (Hz):</label>
                        <input type="number" id="freq4" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq5">Freq 5 (Hz):</label>
                        <input type="number" id="freq5" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq6">Freq 6 (Hz):</label>
                        <input type="number" id="freq6" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq7">Freq 7 (Hz):</label>
                        <input type="number" id="freq7" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq8">Freq 8 (Hz):</label>
                        <input type="number" id="freq8" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <div class="input-group">
                        <label for="freq9">Freq 9 (Hz):</label>
                        <input type="number" id="freq9" class="form-control" placeholder="Enter frequency" min="1">
                    </div>
                    <button class="btn btn-primary" onclick="updateCymatics()">Update Pattern</button>
                    <button class="btn btn-primary" onclick="playCymaticNotes()">Play Frequencies</button>
                    <button class="btn btn-primary" onclick="toggleCymaticsMode()">Toggle 2D/3D</button>
                    <button class="btn btn-primary" onclick="clearCymaticsInputs()">Clear All</button>
                </div>
                <div id="cymaticsStatus" class="alert alert-info"></div>
                <canvas id="cymaticsCanvas" width="720" height="720"></canvas>
                <div id="cymaticsPlotly" class="plotly-container"></div>
            </div>
            <div id="cymaticSymphony" class="tab-content">
                <div class="controls">
                    <label for="symphonyPattern">Playback Pattern:</label>
                    <select id="symphonyPattern" class="form-select">
                        <option value="row-wise">Left-to-Right, Downward</option>
                        <option value="column-wise">Top-to-Bottom, Left-to-Right</option>
                        <option value="both">Both Overlap</option>
                    </select>
                    <button class="btn btn-primary" onclick="playSymphony()">Play Symphony</button>
                    <button class="btn btn-primary" onclick="stopSymphony()">Stop Symphony</button>
                    <button class="btn btn-primary" onclick="toggleSymphonyMode()">Toggle 2D/3D</button>
                </div>
                <div class="legend">
                    <strong>Digital Roots & Notes:</strong>
                    <ul>
                        <li><span class="color-box" style="background: #00FFFF"></span> 1: Cyan (130.81 Hz)</li>
                        <li><span class="color-box" style="background: #00FF00"></span> 2: Green (138.59 Hz)</li>
                        <li><span class="color-box" style="background: #FFA500"></span> 3: Orange (146.83 Hz)</li>
                        <li><span class="color-box" style="background: #FF69B4"></span> 4: Pink (155.56 Hz)</li>
                        <li><span class="color-box" style="background: #228B22"></span> 5: Forest Green (164.81 Hz)</li>
                        <li><span class="color-box" style="background: #FFFF00"></span> 6: Yellow (174.61 Hz)</li>
                        <li><span class="color-box" style="background: #800080"></span> 7: Purple (184.99 Hz)</li>
                        <li><span class="color-box" style="background: #808080"></span> 8: Gray (196.00 Hz)</li>
                        <li><span class="color-box" style="background: #FF0000"></span> 9: Red (207.65 Hz)</li>
                    </ul>
                    <p id="symphonyRootCounts"></p>
                </div>
                <canvas id="symphonyCanvas" width="720" height="720"></canvas>
                <div id="symphonyPlotly" class="plotly-container"></div>
                <div id="symphonyGrid"></div>
            </div>
            <div id="about" class="tab-content">
                <h2>Unifying Theory of Intelligent Design</h2>
                <p>The Tapestry of the Cosmos reveals an underlying order in reality, connecting mathematics, music, and physical phenomena. By exploring Fibonacci-derived digital roots, prime numbers, musical scales, and cymatic patterns, this app demonstrates a harmonious structure suggestive of intelligent design.</p>
                <p><strong>Fibonacci and Digital Roots:</strong> The 9x24 and 72x72 grids, derived from Fibonacci numbers, produce cyclic digital root patterns (mod 9) that mirror cosmic cycles.</p>
                <p><strong>Primes:</strong> Prime numbers’ digital roots reveal selective patterns (e.g., excluding 6), hinting at a structured numerical order.</p>
                <p><strong>Music and Cymatics:</strong> Mapping digital roots to musical notes and visualizing frequencies as cymatic patterns connects numbers to sensory experiences, reflecting universal harmonics.</p>
                <p><strong>Geometric Forms:</strong> Visualizations like Möbius strips and toroidal plots suggest that mathematical patterns manifest in topological structures, supporting a designed reality.</p>
            </div>
            <div id="loading">
                <div class="loading-bar">
                    <div class="loading-progress"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Constants
        const COLOR_MAP = {
            1: '#00FFFF', // Cyan
            2: '#00FF00', // Green
            3: '#FFA500', // Orange
            4: '#FF69B4', // Pink
            5: '#228B22', // Forest Green
            6: '#FFFF00', // Yellow
            7: '#800080', // Purple
            8: '#808080', // Gray
            9: '#FF0000'  // Red
        };
        const SONG_FREQUENCIES = {
            1: 432, 2: 432, 3: 720, 4: 432, 5: 432, 6: 720, 7: 432, 8: 432, 9: 1152
        };
        const SYMPHONY_FREQUENCIES = {
            1: 130.81, 2: 138.59, 3: 146.83, 4: 155.56, 5: 164.81, 6: 174.61, 7: 184.99, 8: 196.00, 9: 207.65
        };
        const state = {
            zoom: 1,
            offsetX: 0,
            offsetY: 0,
            isDragging: false,
            lastX: 0,
            lastY: 0,
            points: [],
            abort: false
        };
        let audioContext = null;
        let isPlaying = false;
        let lastPixelData = [];

        // Fibonacci and Grid Generation
        function generateFibonacci(n) {
            const fib = [1n, 1n]; // Start with [1, 1] to exclude 0
            for (let i = 2; i < n; i++) {
                fib[i] = fib[i-1] + fib[i-2];
            }
            return fib;
        }

        function digitalRoot(n) {
            const mod = Number(n % 9n);
            return mod === 0 ? 9 : mod;
        }

        const fibNumbers = generateFibonacci(72);
        const fibSequences9x24 = Array.from({length: 9}, (_, k) =>
            Array.from({length: 24}, (_, n) => digitalRoot(BigInt(k + 1) * fibNumbers[n]))
        );
        const fibSequences72x72 = Array.from({length: 72}, (_, k) =>
            Array.from({length: 72}, (_, n) => digitalRoot(BigInt(k + 1) * fibNumbers[n]))
        );

        // Grid Visualizer
        const gridCanvas = document.getElementById('gridCanvas');
        const gridCtx = gridCanvas.getContext('2d');
        function renderCheckboxes() {
            const sectorCheckboxes = document.getElementById('sectorCheckboxes');
            sectorCheckboxes.innerHTML = '';
            for (let i = 0; i < 8; i++) {
                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.checked = true;
                checkbox.onchange = () => cachePoints();
                checkbox.id = `sector${i + 1}`;
                const label = document.createElement('label');
                label.textContent = `S${i + 1}`;
                label.style.color = '#fff';
                label.style.marginRight = '10px';
                sectorCheckboxes.appendChild(checkbox);
                sectorCheckboxes.appendChild(label);
            }
            const rootCheckboxes = document.getElementById('rootCheckboxes');
            rootCheckboxes.innerHTML = '';
            for (let i = 1; i <= 9; i++) {
                const checkbox = document.createElement('input');
                checkbox.type = 'checkbox';
                checkbox.checked = true;
                checkbox.onchange = () => cachePoints();
                checkbox.id = `root${i}`;
                const label = document.createElement('label');
                label.textContent = i;
                label.style.color = COLOR_MAP[i];
                label.style.marginRight = '10px';
                rootCheckboxes.appendChild(checkbox);
                rootCheckboxes.appendChild(label);
            }
        }

        function cachePoints() {
            const gridSize = document.getElementById('gridSize').value;
            const fibSequences = gridSize === '9x24' ? fibSequences9x24 : fibSequences72x72;
            const maxCols = gridSize === '9x24' ? 24 : 72;
            const maxRows = gridSize === '9x24' ? 9 : 72;
            let startCol = parseInt(document.getElementById('startCol').value || 1);
            let endCol = parseInt(document.getElementById('endCol').value || maxCols);
            if (isNaN(startCol) || startCol < 1) startCol = 1;
            if (isNaN(endCol) || endCol < startCol) endCol = maxCols;
            const selectedSectors = Array.from({length: 8}, (_, i) => document.getElementById(`sector${i+1}`).checked);
            const selectedRoots = Array.from({length: 9}, (_, i) => document.getElementById(`root${i+1}`).checked);
            const showPrimes = document.getElementById('showPrimes').checked;
            const primes = showPrimes ? goldenDigitalRootSieve(1, Math.max(endCol, maxCols)) : [];
            state.points = [];
            for (let k = 0; k < maxRows; k++) {
                if (gridSize === '72x72' && !selectedSectors[Math.floor(k / 9)]) continue;
                for (let n = startCol - 1; n < endCol; n++) {
                    if (n >= maxCols) break;
                    const value = fibSequences[k][n];
                    if (selectedRoots[value - 1] && (!showPrimes || primes.includes(k + 1))) {
                        state.points.push({ sequence: k + 1, column: n + 1, value, sector: Math.floor(k / 9) + 1 });
                    }
                }
            }
            const counts = state.points.reduce((acc, p) => {
                acc[p.value] = (acc[p.value] || 0) + 1;
                return acc;
            }, {});
            document.getElementById('rootCounts').textContent = `Counts: ${Object.entries(counts).map(([k, v]) => `${k}: ${v}`).join(', ')}`;
            draw2DGrid();
            plot3D();
        }

        function draw2DGrid() {
            gridCtx.clearRect(0, 0, gridCanvas.width, gridCanvas.height);
            const gridSize = document.getElementById('gridSize').value;
            const maxCols = gridSize === '9x24' ? 24 : 72;
            const cellSize = gridSize === '9x24' ? 720 / 24 * state.zoom : 720 / 72 * state.zoom;
            gridCtx.save();
            gridCtx.translate(state.offsetX, state.offsetY);
            state.points.forEach(point => {
                const x = (point.column - 1) * cellSize;
                const y = (point.sequence - 1) * cellSize;
                gridCtx.fillStyle = COLOR_MAP[point.value];
                gridCtx.fillRect(x, y, cellSize, cellSize);
            });
            gridCtx.restore();
        }

        function setupCanvasEvents() {
            let tooltip = null;
            gridCanvas.addEventListener('mousedown', e => {
                state.isDragging = true;
                state.lastX = e.offsetX;
                state.lastY = e.offsetY;
            });
            gridCanvas.addEventListener('mousemove', e => {
                if (state.isDragging) {
                    state.offsetX += e.offsetX - state.lastX;
                    state.offsetY += e.offsetY - state.lastY;
                    state.lastX = e.offsetX;
                    state.lastY = e.offsetY;
                    draw2DGrid();
                }
                const gridSize = document.getElementById('gridSize').value;
                const maxCols = gridSize === '9x24' ? 24 : 72;
                const cellSize = gridSize === '9x24' ? 720 / 24 * state.zoom : 720 / 72 * state.zoom;
                const x = (e.offsetX - state.offsetX) / cellSize;
                const y = (e.offsetY - state.offsetY) / cellSize;
                const col = Math.floor(x) + 1;
                const row = Math.floor(y) + 1;
                const fibSequences = gridSize === '9x24' ? fibSequences9x24 : fibSequences72x72;
                if (row >= 1 && row <= (gridSize === '9x24' ? 9 : 72) && col >= 1 && col <= maxCols) {
                    const value = fibSequences[row - 1][col - 1];
                    if (!tooltip) {
                        tooltip = document.createElement('div');
                        tooltip.className = 'tooltip';
                        document.body.appendChild(tooltip);
                    }
                    tooltip.style.left = `${e.pageX + 10}px`;
                    tooltip.style.top = `${e.pageY + 10}px`;
                    tooltip.textContent = `Row: ${row}, Col: ${col}, Value: ${value}`;
                } else if (tooltip) {
                    tooltip.remove();
                    tooltip = null;
                }
            });
            gridCanvas.addEventListener('mouseup', () => state.isDragging = false);
            gridCanvas.addEventListener('wheel', e => {
                e.preventDefault();
                state.zoom += e.deltaY < 0 ? 0.1 : -0.1;
                state.zoom = Math.max(0.5, Math.min(state.zoom, 3));
                draw2DGrid();
            });
            gridCanvas.addEventListener('touchstart', e => {
                if (e.touches.length === 1) {
                    state.isDragging = true;
                    state.lastX = e.touches[0].clientX - gridCanvas.getBoundingClientRect().left;
                    state.lastY = e.touches[0].clientY - gridCanvas.getBoundingClientRect().top;
                }
            });
            gridCanvas.addEventListener('touchmove', e => {
                if (state.isDragging && e.touches.length === 1) {
                    e.preventDefault();
                    const x = e.touches[0].clientX - gridCanvas.getBoundingClientRect().left;
                    const y = e.touches[0].clientY - gridCanvas.getBoundingClientRect().top;
                    state.offsetX += x - state.lastX;
                    state.offsetY += y - state.lastY;
                    state.lastX = x;
                    state.lastY = y;
                    draw2DGrid();
                }
            });
            gridCanvas.addEventListener('touchend', () => state.isDragging = false);
        }

        function plot3D() {
            const gridSize = document.getElementById('gridSize').value;
            const maxCols = gridSize === '9x24' ? 24 : 72;
            const maxRows = gridSize === '9x24' ? 9 : 72;
            const markerSize = gridSize === '9x24' ? 5 : 3;
            const types = ['Interactive', 'Toroidal', 'Globe', 'Mobius', 'Cylinder', 'Trends', 'Polar'];
            types.forEach(type => {
                const container = `plotly${type}`;
                let data = [];
                const layout = {
                    width: Math.min(800, window.innerWidth * 0.9),
                    height: Math.min(800, window.innerWidth * 0.9),
                    margin: { t: 0 },
                    scene: type !== 'Trends' && type !== 'Polar' ? {
                        xaxis: { title: 'X' },
                        yaxis: { title: 'Y' },
                        zaxis: { title: 'Z' }
                    } : {}
                };
                if (type === 'Interactive') {
                    data = [{
                        type: 'scatter3d',
                        x: state.points.map(p => p.sequence / maxRows),
                        y: state.points.map(p => p.column / maxCols),
                        z: state.points.map(p => p.value),
                        mode: 'markers',
                        marker: { size: markerSize, color: state.points.map(p => COLOR_MAP[p.value]) },
                        text: state.points.map(p => `Row: ${p.sequence}, Col: ${p.column}, Value: ${p.value}`)
                    }];
                } else if (type === 'Toroidal') {
                    const R = gridSize === '72x72' ? 3 : 1.5;
                    const r = 0.5;
                    data = [{
                        type: 'scatter3d',
                        x: state.points.map(p => (R + r * Math.cos(p.column * 2 * Math.PI / maxCols)) * Math.cos(p.sequence * 2 * Math.PI / maxRows)),
                        y: state.points.map(p => (R + r * Math.cos(p.column * 2 * Math.PI / maxCols)) * Math.sin(p.sequence * 2 * Math.PI / maxRows)),
                        z: state.points.map(p => r * Math.sin(p.column * 2 * Math.PI / maxCols)),
                        mode: 'markers',
                        marker: { size: markerSize, color: state.points.map(p => COLOR_MAP[p.value]) },
                        text: state.points.map(p => `Row: ${p.sequence}, Col: ${p.column}, Value: ${p.value}`)
                    }];
                } else if (type === 'Globe') {
                    data = [{
                        type: 'scatter3d',
                        x: state.points.map(p => Math.sin(p.sequence * Math.PI / maxRows) * Math.cos(p.column * 2 * Math.PI / maxCols)),
                        y: state.points.map(p => Math.sin(p.sequence * Math.PI / maxRows) * Math.sin(p.column * 2 * Math.PI / maxCols)),
                        z: state.points.map(p => Math.cos(p.sequence * Math.PI / maxRows)),
                        mode: 'markers',
                        marker: { size: markerSize, color: state.points.map(p => COLOR_MAP[p.value]) },
                        text: state.points.map(p => `Row: ${p.sequence}, Col: ${p.column}, Value: ${p.value}`)
                    }];
                } else if (type === 'Mobius') {
                    const R = gridSize === '72x72' ? 3 : 1.5;
                    data = [{
                        type: 'scatter3d',
                        x: state.points.map(p => (R + (p.value - 5) * 0.2 * Math.cos(p.sequence * Math.PI / maxRows)) * Math.cos(p.sequence * 2 * Math.PI / maxRows)),
                        y: state.points.map(p => (R + (p.value - 5) * 0.2 * Math.cos(p.sequence * Math.PI / maxRows)) * Math.sin(p.sequence * 2 * Math.PI / maxRows)),
                        z: state.points.map(p => (p.value - 5) * 0.2 * Math.sin(p.sequence * Math.PI / maxRows)),
                        mode: 'markers',
                        marker: { size: markerSize, color: state.points.map(p => COLOR_MAP[p.value]) },
                        text: state.points.map(p => `Row: ${p.sequence}, Col: ${p.column}, Value: ${p.value}`)
                    }];
                } else if (type === 'Cylinder') {
                    data = [{
                        type: 'scatter3d',
                        x: state.points.map(p => p.value * 0.2 * Math.cos(p.sequence * 2 * Math.PI / maxRows)),
                        y: state.points.map(p => p.value * 0.2 * Math.sin(p.sequence * 2 * Math.PI / maxRows)),
                        z: state.points.map(p => p.column / maxCols),
                        mode: 'markers',
                        marker: { size: markerSize, color: state.points.map(p => COLOR_MAP[p.value]) },
                        text: state.points.map(p => `Row: ${p.sequence}, Col: ${p.column}, Value: ${p.value}`)
                    }];
                } else if (type === 'Trends') {
                    data = state.points.reduce((acc, p) => {
                        const trace = acc[p.value - 1] || {
                            type: 'scatter3d',
                            x: [], y: [], z: [],
                            mode: 'lines',
                            name: `Root ${p.value}`,
                            line: { color: COLOR_MAP[p.value], width: 2 }
                        };
                        trace.x.push(p.sequence / maxRows);
                        trace.y.push(p.column / maxCols);
                        trace.z.push(p.value);
                        acc[p.value - 1] = trace;
                        return acc;
                    }, []).filter(Boolean);
                } else if (type === 'Polar') {
                    data = state.points.reduce((acc, p) => {
                        const trace = acc[p.value - 1] || {
                            type: 'scatter3d',
                            x: [], y: [], z: [],
                            mode: 'markers',
                            name: `Root ${p.value}`,
                            marker: { size: markerSize, color: COLOR_MAP[p.value] },
                            text: []
                        };
                        trace.x.push(p.value * 0.2 * Math.cos(p.sequence * 2 * Math.PI / maxRows));
                        trace.y.push(p.value * 0.2 * Math.sin(p.sequence * 2 * Math.PI / maxRows));
                        trace.z.push(p.column / maxCols);
                        trace.text.push(`Row: ${p.sequence}, Col: ${p.column}, Value: ${p.value}`);
                        acc[p.value - 1] = trace;
                        return acc;
                    }, []).filter(Boolean);
                }
                Plotly.newPlot(container, data, layout);
            });
        }

        function exportCanvas() {
            gridCtx.save();
            gridCtx.setTransform(1, 0, 0, 1, 0, 0);
            const gridSize = document.getElementById('gridSize').value;
            const maxCols = gridSize === '9x24' ? 24 : 72;
            const maxRows = gridSize === '9x24' ? 9 : 72;
            const cellSize = gridSize === '9x24' ? 720 / 24 : 720 / 72;
            gridCtx.clearRect(0, 0, gridCanvas.width, gridCanvas.height);
            state.points.forEach(point => {
                const x = (point.column - 1) * cellSize;
                const y = (point.sequence - 1) * cellSize;
                gridCtx.fillStyle = COLOR_MAP[point.value];
                gridCtx.fillRect(x, y, cellSize, cellSize);
            });
            const link = document.createElement('a');
            link.href = gridCanvas.toDataURL('image/png');
            link.download = 'tapestry_grid.png';
            link.click();
            gridCtx.restore();
            draw2DGrid();
        }

        function updateGrid() {
            const gridSize = document.getElementById('gridSize').value;
            const maxCols = gridSize === '9x24' ? 24 : 72;
            document.getElementById('endCol').value = maxCols;
            cachePoints();
        }

        // Prime Sieve
        let lastInput = { type: null, number: null, start: null, end: null, primes: null };
        let primeCache = {};
        function isPrime(n) {
            if (state.abort) return false;
            if (n < 2) return false;
            if (n === 2 || n === 3 || n === 5) return true;
            if (n % 2 === 0 || n % 5 === 0) return false;
            for (let i = 3; i <= Math.sqrt(n); i += 2) {
                if (state.abort) return false;
                if (n % i === 0) return false;
            }
            return true;
        }

        function isMersennePrime(n) {
            if (state.abort || !isPrime(n)) return false;
            let p = 0;
            let temp = n + 1;
            while (temp % 2 === 0) {
                temp /= 2;
                p++;
            }
            return temp === 1 && isPrime(p);
        }

        function sieveDigitalRoot(start, end, dr) {
            if (state.abort) return [];
            if ([3, 9].includes(dr)) return start <= 3 && 3 <= end ? [3] : [];
            if (dr === 6) return [];
            const remainder = {1: 1, 2: 2, 4: 4, 5: 5, 7: 7, 8: 8}[dr];
            const primes = [];
            start = Math.max(start, dr !== 2 ? 1 : 2);
            for (let n = start + (remainder - (start % 9) + 9) % 9; n <= end; n += 9) {
                if (state.abort) return [];
                if ([2, 3, 5, 11].includes(n) && digitalRoot(n) === dr) {
                    primes.push(n);
                    continue;
                }
                if (n % 2 === 0 || n % 5 === 0) continue;
                if (digitalRoot(n) === dr && isPrime(n)) primes.push(n);
            }
            return primes;
        }

        function goldenDigitalRootSieve(start, end) {
            if (state.abort) return [];
            const cacheKey = `${start}-${end}`;
            if (primeCache[cacheKey]) return primeCache[cacheKey];
            const primes = new Set();
            for (let dr of [1, 2, 4, 5, 7, 8, 3, 9]) {
                if (state.abort) return [];
                sieveDigitalRoot(start, end, dr).forEach(p => primes.add(p));
            }
            const result = Array.from(primes).sort((a, b) => a - b);
            primeCache[cacheKey] = result;
            return result;
        }

        function checkPrime() {
            state.abort = false;
            showLoading();
            const n = parseInt(document.getElementById('numberInput').value);
            const output = document.getElementById('primeOutput');
            if (isNaN(n) || n < 0) {
                output.textContent = 'Invalid inpu        }
        #tabs {
            display: flex;
            overflow-x: auto;
            background: #222;
        }
        .tab-button {
            flex: 1;
            padding: 10px;
            text-align: center;
            background: #222;
            border: none;
            color: white;
            cursor: pointer;
        }
        .tab-button.active {
            background: #333;
            color: cyan;
        }
        .tab-content {
            display: none;
            padding: 10px;
        }
        .tab-content.active {
            display: block;
        }
        .scroll-section {
            overflow-y: auto;
            max-height: 70vh;
        }
        .controls label {
            display: block;
            margin: 5px 0;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(72, 10px);
            grid-template-rows: repeat(72, 10px);
            gap: 1px;
            overflow: auto;
            max-width: 720px;
            margin: 0 auto;
        }
        .cell {
            width: 10px;
            height: 10px;
        }
        .cyan { background-color: cyan; }
        .bright-green { background-color: lime; }
        .orange { background-color: orange; }
        .bright-pink { background-color: hotpink; }
        .forest-green { background-color: forestgreen; }
        .yellow { background-color: yellow; }
        .purple { background-color: purple; }
        .gray { background-color: gray; }
        .red { background-color: red; }
        .highlight { background-color: white !important; }
        .btn {
            margin: 5px 0;
            padding: 10px;
            width: 100%;
            max-width: 200px;
            border: none;
            background: #333;
            color: white;
            cursor: pointer;
            border-radius: 5px;
        }
        .btn:hover {
            background: #444;
        }
        select {
            margin: 5px 0;
            padding: 10px;
            width: 100%;
            max-width: 200px;
            background: #333;
            color: white;
            border: none;
            border-radius: 5px;
            max-height: 200px;
            overflow-y: auto;
        }
        select:hover {
            background: #444;
        }
        .plotly-graph-div { max-width: 100% !important; }
        .plot-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(360px, 1fr));
            gap: 10px;
        }
        canvas { display: block; max-width: 100%; margin: 0 auto; }
        .plot-error { color: red; text-align: center; margin: 10px 0; }
    </style>
</head>
<body>
    <header>Fibonacci Harmonics Explorer</header>
    <div id="tabs">
        <button class="tab-button active" onclick="switchTab('grid')">Grid</button>
        <button class="tab-button" onclick="switchTab('3d')">3D Views</button>
        <button class="tab-button" onclick="switchTab('music')">Music</button>
        <button class="tab-button" onclick="switchTab('cymatics')">Cymatics</button>
        <button class="tab-button" onclick="switchTab('cosmos')">Cosmos</button>
        <button class="tab-button" onclick="switchTab('settings')">Settings</button>
        <button class="tab-button" onclick="switchTab('about')">About</button>
    </div>
    <div id="grid" class="tab-content active">
        <h2>Mother Grid</h2>
        <div class="controls scroll-section" id="root-controls">
            <h3 class="font-semibold">Toggle Digital Roots</h3>
            <label><input type="checkbox" checked value="1" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 1 (Cyan)</label>
            <label><input type="checkbox" checked value="2" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 2 (Bright Green)</label>
            <label><input type="checkbox" checked value="3" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 3 (Orange)</label>
            <label><input type="checkbox" checked value="4" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 4 (Bright Pink)</label>
            <label><input type="checkbox" checked value="5" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 5 (Forest Green)</label>
            <label><input type="checkbox" checked value="6" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 6 (Yellow)</label>
            <label><input type="checkbox" checked value="7" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 7 (Purple)</label>
            <label><input type="checkbox" checked value="8" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 8 (Gray)</label>
            <label><input type="checkbox" checked value="9" onchange="updateGrid('mother-grid'); setTimeout(() => update3DGraphs(1, 3), 1000);"> 9 (Red)</label>
        </div>
        <div class="grid" id="mother-grid"></div>
        <div id="root-counts" style="width: 100%; max-width: 380px; height: 285px; margin-top: 20px;"></div>
    </div>
    <div id="3d" class="tab-content">
        <h2>3D Harmonic Views</h2>
        <button class="btn" onclick="renderGlobe()">Globe</button>
        <button class="btn" onclick="renderCylinder()">Cylinder</button>
        <button class="btn" onclick="renderSpiral()">Spiral</button>
        <button class="btn" onclick="renderFibonacciSpiral()">Fibonacci Spiral</button>
        <button class="btn" onclick="renderMobius()">Möbius Strip</button>
        <button class="btn" onclick="renderHelicalToroidal()">Helical Toroidal</button>
        <button class="btn" onclick="renderCube()">Cube</button>
        <button class="btn" onclick="render3DPolar()">3D Polar Plot</button>
        <div id="3d-error" class="plot-error" style="display: none;">Failed to render 3D plots. Please check console for details.</div>
        <div class="plot-grid">
            <div id="globe-plot" style="width: 360px; height: 360px;"></div>
            <div id="cylinder-plot" style="width: 360px; height: 360px;"></div>
            <div id="spiral-plot" style="width: 360px; height: 360px;"></div>
            <div id="fibonacci-spiral-plot" style="width: 360px; height: 360px;"></div>
            <div id="toroidal-mobius-plot" style="width: 360px; height: 360px;"></div>
            <div id="helical-toroidal-plot" style="width: 360px; height: 360px;"></div>
            <div id="cube-plot" style="width: 360px; height: 360px;"></div>
            <div id="polar-plot" style="width: 360px; height: 360px;"></div>
            <div id="3d-polar-plot" style="width: 360px; height: 360px;"></div>
        </div>
    </div>
    <div id="music" class="tab-content">
        <h2>Mother Sequence Song</h2>
        <button class="btn" onclick="playSong('full')">▶️ Start to Finish</button>
        <select id="sequence-select" multiple size="10"></select>
        <button class="btn" onclick="playSong('sequences')">▶️ Play Selected Sequences</button>
        <select id="sector-select">
            <option value="0">Select Sector</option>
            <option value="1">Sector 1 (Rows 1–9)</option>
            <option value="2">Sector 2 (Rows 10–18)</option>
            <option value="3">Sector 3 (Rows 19–27)</option>
            <option value="4">Sector 4 (Rows 28–36)</option>
            <option value="5">Sector 5 (Rows 37–45)</option>
            <option value="6">Sector 6 (Rows 46–54)</option>
            <option value="7">Sector 7 (Rows 55–63)</option>
            <option value="8">Sector 8 (Rows 64–72)</option>
        </select>
        <button class="btn" onclick="playSong('sector')">▶️ Play Selected Sector</button>
        <button class="btn" onclick="stopSong()">⏹️ Stop</button>
        <p>Note mapping and frequency tuning logic applied with octave shifts per sector.</p>
        <div class="grid" id="music-grid"></div>
        <div id="frequency-graph" style="width: 100%; max-width: 600px; height: 400px; margin-top: 20px;">
            <canvas id="freq-canvas" width="600" height="400"></canvas>
        </div>
    </div>
    <div id="cymatics" class="tab-content">
        <h2>Cymatics Generator</h2>
        <canvas id="cymaticsCanvas" width="360" height="360"></canvas>
        <button class="btn" onclick="generateCymatics()">Generate Cymatics</button>
        <div id="frequency-3d" style="width: 360px; height: 360px;"></div>
    </div>
    <div id="cosmos" class="tab-content">
        <h2>Cosmological Overlay</h2>
        <button class="btn" onclick="renderCosmicOverlay()">Render CMB Overlay</button>
        <div id="cosmo-overlay" style="width: 360px; height: 360px;"></div>
    </div>
    <div id="settings" class="tab-content">
        <h2>Customize</h2>
        <p>(Coming soon): Set color mappings, note values, harmonic tuning, and sequence skips.</p>
    </div>
    <div id="about" class="tab-content">
        <h2>About This Tool</h2>
        <p>This app visualizes the Fibonacci sequence through digital roots, colors, and sound. It supports harmonic projection in 3D space, cymatic resonance, and overlays to cosmological models like the CMB spectrum and Friedmann equations.</p>
        <p>Explore how the root patterns repeat in toroidal harmonics, experience the audible symmetry, and visualize frequency interactions.</p>
    </div>

    <script>
        // Fibonacci sequence up to 72nd number (excluding 0)
        const fib = [1n];
        for (let i = 1; i < 72; i++) {
            fib[i] = fib[i - 1] + (i >= 2 ? fib[i - 2] : 0n);
        }

        // Create Father grid (72x72)
        const father = Array(72).fill().map((_, i) => 
            fib.map(n => n * BigInt(i + 1))
        );
        console.log('Father[0][0]:', Number(father[0][0]), 'Father[71][71]:', Number(father[71][71]));
        console.log('Father[9][9]:', Number(father[9][9]), 'Father[49][49]:', Number(father[49][49]));
        console.log('Father grid size:', father.length, 'x', father[0].length);

        // Digital root function
        function digitalRoot(n) {
            if (n === 0n) return 0;
            let num = n;
            while (num > 9n) {
                num = num.toString().split('').reduce((sum, d) => sum + parseInt(d), 0);
            }
            return num === 0n ? 9 : Number(num);
        }

        // Create Mother grid with digital roots
        const mother = father.map(row => row.map(n => digitalRoot(n)));
        console.log('Mother[0][0]:', mother[0][0], 'Mother[71][71]:', mother[71][71]);

        // Color scheme for digital roots
        const colors = {
            1: 'cyan',
            2: 'bright-green',
            3: 'orange',
            4: 'bright-pink',
            5: 'forest-green',
            6: 'yellow',
            7: 'purple',
            8: 'gray',
            9: 'red'
        };
        const plotlyColors = {
            1: 'cyan',
            2: 'lime',
            3: 'orange',
            4: 'hotpink',
            5: 'forestgreen',
            6: 'yellow',
            7: 'purple',
            8: 'gray',
            9: 'red'
        };

        // Initialize Mother grid displays
        function updateGrid(gridId = 'mother-grid') {
            const activeRoots = Array.from(document.querySelectorAll('#root-controls input:checked')).map(input => parseInt(input.value));
            const gridContainer = document.getElementById(gridId);
            gridContainer.innerHTML = '';
            const appliedColors = {};
            for (let i = 0; i < 72; i++) {
                for (let j = 0; j < 72; j++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    const root = mother[i][j];
                    const colorClass = activeRoots.includes(root) ? colors[root] : 'gray';
                    cell.classList.add(colorClass);
                    cell.title = `Row: ${i + 1}, Seq: ${j + 1}, Root: ${root}`;
                    cell.dataset.row = i;
                    cell.dataset.seq = j;
                    gridContainer.appendChild(cell);
                    appliedColors[root] = colorClass;
                }
            }
            console.log('Grid colors applied:', appliedColors);
            console.log(`${gridId} width:`, gridContainer.offsetWidth);
            if (gridId === 'mother-grid') {
                update2DGraphs();
            }
        }

        // 3D Visualizations with Scatter3D for 5184 Points
        function update3DGraphs(attempt = 1, maxAttempts = 3) {
            if (!window.Plotly) {
                console.error('Plotly not loaded. Version check failed.');
                document.getElementById('3d-error').style.display = 'block';
                if (attempt < maxAttempts) {
                    console.log(`Retrying 3D plots, attempt ${attempt + 1}/${maxAttempts}`);
                    setTimeout(() => update3DGraphs(attempt + 1, maxAttempts), 3000);
                }
                return;
            }
            console.log('Plotly loaded, version:', Plotly.version);

            const plotIds = ['globe-plot', 'cylinder-plot', 'spiral-plot', 'fibonacci-spiral-plot', 'toroidal-mobius-plot', 'helical-toroidal-plot', 'cube-plot', 'polar-plot', '3d-polar-plot', 'cosmo-overlay'];
            let error = false;
            for (const plotId of plotIds) {
                Plotly.purge(plotId); // Clear previous state
                const container = document.getElementById(plotId);
                if (!container) {
                    console.error(`Plot container ${plotId} not found`);
                    error = true;
                } else {
                    console.log(`Container ${plotId} found, dimensions: ${container.offsetWidth}x${container.offsetHeight}`);
                }
            }
            if (error) {
                document.getElementById('3d-error').style.display = 'block';
                if (attempt < maxAttempts) {
                    console.log(`Retrying 3D plots, attempt ${attempt + 1}/${maxAttempts}`);
                    setTimeout(() => update3DGraphs(attempt + 1, maxAttempts), 3000);
                }
                return;
            } else {
                document.getElementById('3d-error').style.display = 'none';
            }

            const activeRoots = Array.from(document.querySelectorAll('#root-controls input:checked')).map(input => parseInt(input.value));
            const maxVal = Number(father[71][71]) || 1;
            const maxLog = Math.log10(maxVal + 1);
            const phi = (1 + Math.sqrt(5)) / 2; // Golden ratio

            const plotTypes = [
                { id: 'globe', name: 'Globe', desc: 'Spherical harmonic distribution of digital roots' },
                { id: 'cylinder', name: 'Cylinder', desc: 'Cylindrical projection with row-based periodicity' },
                { id: 'spiral', name: 'Spiral', desc: 'Archimedean spiral reflecting Fibonacci growth' },
                { id: 'fibonacci-spiral', name: 'Fibonacci Spiral', desc: 'Logarithmic spiral with golden ratio scaling' },
                { id: 'toroidal-mobius', name: 'Möbius Strip', desc: 'Non-orientable twisted toroidal band' },
                { id: 'helical-toroidal', name: 'Helical Toroidal', desc: 'Helical path on a toroidal surface' },
                { id: 'cube', name: 'Cube', desc: 'Cubic lattice of digital roots' },
                { id: '3d-polar', name: '3D Polar Plot', desc: '3D projection of vector paths by digital root' }
            ];

            plotTypes.forEach(({ id, name, desc }) => {
                const x = [], y = [], z = [], c = [], text = [];
                let pointCount = 0;
                for (let i = 0; i < 72; i++) {
                    for (let j = 0; j < 72; j++) {
                        if (activeRoots.includes(mother[i][j])) {
                            const fatherVal = Number(father[i][j]);
                            const logVal = Math.log10(fatherVal + 1) / maxLog;
                            const theta = (j / 72) * 2 * Math.PI; // Sequence angle
                            const phiAngle = (i / 72) * Math.PI; // Row angle
                            let px, py, pz;
                            if (id === 'globe') {
                                const R = 12 + logVal * 0.5;
                                px = R * Math.sin(phiAngle) * Math.cos(theta);
                                py = R * Math.sin(phiAngle) * Math.sin(theta);
                                pz = R * Math.cos(phiAngle);
                            } else if (id === 'cylinder') {
                                const R = 10 + logVal * 0.5;
                                px = R * Math.cos(theta);
                                py = R * Math.sin(theta);
                                pz = (i / 72) * 24 - 12;
                            } else if (id === 'spiral') {
                                const R = (i / 72) * 12 + logVal * 0.5;
                                px = R * Math.cos(theta * 3);
                                py = R * Math.sin(theta * 3);
                                pz = (i / 72) * 24 - 12;
                            } else if (id === 'fibonacci-spiral') {
                                const R = (i / 72) * 10 * Math.pow(phi, i / 72); // Golden ratio scaling
                                px = R * Math.cos(theta * 3);
                                py = R * Math.sin(theta * 3);
                                pz = (i / 72) * 24 - 12;
                            } else if (id === 'toroidal-mobius') {
                                const R = 10 + logVal * 0.5;
                                const r = 5; // Enhanced for clarity
                                const u = (i / 72) * 2 * Math.PI;
                                const v = (j / 72) * Math.PI;
                                px = (R + r * Math.cos(v + theta)) * Math.cos(u);
                                py = (R + r * Math.cos(v + theta)) * Math.sin(u);
                                pz = r * Math.sin(v + theta);
                            } else if (id === 'helical-toroidal') {
                                const R = 10 + logVal * 0.5;
                                const r = 4;
                                const helixAngle = (j / 72) * 6 * Math.PI;
                                px = (R + r * Math.cos(theta)) * Math.cos(helixAngle);
                                py = (R + r * Math.cos(theta)) * Math.sin(helixAngle);
                                pz = r * Math.sin(theta) + (i / 72) * 6;
                            } else if (id === 'cube') {
                                px = (i / 72) * 20 - 10; // Row-based X
                                py = (j / 72) * 20 - 10; // Sequence-based Y
                                pz = mother[i][j] * 2 - 10; // Root-based Z
                            } else if (id === '3d-polar') {
                                const R = 10; // Fixed radius for cylindrical projection
                                const normalizedValue = Math.min(logVal * 20, 20); // Z-axis for Father value
                                px = R * Math.cos(theta);
                                py = R * Math.sin(theta);
                                pz = normalizedValue - 10; // Center vertically
                            }
                            px = Math.max(Math.min(px, 18), -18);
                            py = Math.max(Math.min(py, 18), -18);
                            pz = Math.max(Math.min(pz, 18), -18);
                            x.push(px);
                            y.push(py);
                            z.push(pz);
                            c.push(plotlyColors[mother[i][j]]);
                            text.push(`Row: ${i + 1}, Seq: ${j + 1}, Root: ${mother[i][j]}, Father: ${fatherVal}, Coord: (${px.toFixed(2)}, ${py.toFixed(2)}, ${pz.toFixed(2)})`);
                            pointCount++;
                        }
                    }
                }
                if (pointCount === 0) {
                    x.push(0); y.push(0); z.push(0); c.push('white');
                    text.push('Fallback: No data points');
                    pointCount = 1;
                }
                console.log(`${id}-plot points:`, pointCount, 'Sample data:', { x: x.slice(0, 5), y: y.slice(0, 5), z: z.slice(0, 5), colors: c.slice(0, 5) });
                Plotly.newPlot(`${id}-plot`, [{
                    type: 'scatter3d',
                    mode: id === '3d-polar' ? 'lines+markers' : 'markers',
                    x: x,
                    y: y,
                    z: z,
                    marker: { size: 2, color: c, opacity: 0.8 },
                    line: id === '3d-polar' ? { color: c, width: 1 } : undefined,
                    text: text,
                    hoverinfo: 'text'
                }], {
                    title: `3D ${name}: ${desc}`,
                    scene: {
                        xaxis: { title: 'X', range: [-18, 18] },
                        yaxis: { title: 'Y', range: [-18, 18] },
                        zaxis: { title: 'Z', range: [-18, 18] },
                        aspectratio: { x: 1, y: 1, z: 1 }
                    },
                    margin: { t: 80, l: 20, r: 20, b: 20 },
                    width: 360,
                    height: 360,
                    paper_bgcolor: '#0a0a0a',
                    plot_bgcolor: '#0a0a0a',
                    font: { color: 'white' }
                }, { responsive: true }).catch(err => {
                    console.error(`Error rendering ${id}-plot:`, err);
                    document.getElementById('3d-error').style.display = 'block';
                    if (attempt < maxAttempts) {
                        console.log(`Retrying ${id}-plot, attempt ${attempt + 1}/${maxAttempts}`);
                        setTimeout(() => update3DGraphs(attempt + 1, maxAttempts), 3000);
                    }
                });
            });

            // 2D Polar Plot: Vector paths for each digital root
            const rootPoints = Array(9).fill().map(() => []);
            for (let i = 0; i < 72; i++) {
                for (let j = 0; j < 72; j++) {
                    if (activeRoots.includes(mother[i][j])) {
                        rootPoints[mother[i][j] - 1].push({ i, j, fatherVal: Number(father[i][j]) });
                    }
                }
            }
            const traces = [];
            rootPoints.forEach((points, rootIndex) => {
                if (points.length === 0) return;
                points.sort((a, b) => a.fatherVal - b.fatherVal); // Sort by increasing Father value
                const r = [], theta = [], text = [];
                points.forEach(({ i, j, fatherVal }) => {
                    const normalizedValue = Math.min(Math.log10(fatherVal + 1) / maxLog * 100, 100);
                    r.push(normalizedValue);
                    theta.push(j * 15 % 360);
                    text.push(`Row: ${i + 1}, Seq: ${j + 1}, Root: ${rootIndex + 1}, Value: ${fatherVal}`);
                });
                traces.push({
                    type: 'scatterpolar',
                    r: r,
                    theta: theta,
                    mode: 'lines+markers',
                    marker: { size: 2, color: plotlyColors[rootIndex + 1], opacity: 0.8 },
                    line: { color: plotlyColors[rootIndex + 1], width: 1 },
                    text: text,
                    hoverinfo: 'text',
                    name: `Root ${rootIndex + 1}`
                });
            });
            Plotly.newPlot('polar-plot', traces.length > 0 ? traces : [{
                type: 'scatterpolar',
                r: [0], theta: [0], mode: 'markers', marker: { size: 2, color: 'white' }, text: ['No data'], hoverinfo: 'text'
            }], {
                title: 'Polar Plot: Vector Paths by Digital Root',
                polar: {
                    radialaxis: { range: [0, 100], title: 'Normalized Father Value' },
                    angularaxis: { direction: 'clockwise', title: 'Angle (degrees)', rotation: 0 }
                },
                margin: { t: 50, l: 50, r: 50, b: 50 },
                width: 360,
                height: 360,
                paper_bgcolor: '#0a0a0a',
                plot_bgcolor: '#0a0a0a',
                font: { color: 'white' }
            }, { responsive: true }).catch(err => console.error('Error rendering polar-plot:', err));

            // Cosmos Tab: Spherical scatter with CMB-like overlay
            const cosmoX = [], cosmoY = [], cosmoZ = [], cosmoC = [], cosmoText = [];
            let cosmoPointCount = 0;
            for (let i = 0; i < 72; i++) {
                for (let j = 0; j < 72; j++) {
                    if (activeRoots.includes(mother[i][j])) {
                        const fatherVal = Number(father[i][j]);
                        const logVal = Math.log10(fatherVal + 1) / maxLog;
                        const theta = (j / 72) * 2 * Math.PI;
                        const phiAngle = (i / 72) * Math.PI;
                        const R = 12 + logVal * 0.5;
                        const px = R * Math.sin(phiAngle) * Math.cos(theta);
                        const py = R * Math.sin(phiAngle) * Math.sin(theta);
                        const pz = R * Math.cos(phiAngle);
                        cosmoX.push(px);
                        cosmoY.push(py);
                        cosmoZ.push(pz);
                        // CMB-like colorscale: Map digital root to temperature-like gradient
                        const tempValue = (mother[i][j] - 1) / 8; // Normalize 1-9 to 0-1
                        cosmoC.push(tempValue);
                        cosmoText.push(`Row: ${i + 1}, Seq: ${j + 1}, Root: ${mother[i][j]}, Father: ${fatherVal}, Temp: ${tempValue.toFixed(2)}`);
                        cosmoPointCount++;
                    }
                }
            }
            if (cosmoPointCount === 0) {
                cosmoX.push(0); cosmoY.push(0); cosmoZ.push(0); cosmoC.push(0);
                cosmoText.push('Fallback: No data points');
                cosmoPointCount = 1;
            }
            Plotly.newPlot('cosmo-overlay', [{
                type: 'scatter3d',
                mode: 'markers',
                x: cosmoX,
                y: cosmoY,
                z: cosmoZ,
                marker: {
                    size: 2,
                    color: cosmoC,
                    colorscale: 'Hot', // CMB-like: black-blue-red-yellow
                    opacity: 0.8,
                    showscale: true,
                    colorbar: { title: 'Temperature' }
                },
                text: cosmoText,
                hoverinfo: 'text'
            }], {
                title: 'Cosmological Overlay: CMB-Like Temperature Map',
                scene: {
                    xaxis: { title: 'X', range: [-18, 18] },
                    yaxis: { title: 'Y', range: [-18, 18] },
                    zaxis: { title: 'Z', range: [-18, 18] },
                    aspectratio: { x: 1, y: 1, z: 1 }
                },
                margin: { t: 80, l: 20, r: 20, b: 20 },
                width: 360,
                height: 360,
                paper_bgcolor: '#0a0a0a',
                plot_bgcolor: '#0a0a0a',
                font: { color: 'white' }
            }, { responsive: true }).catch(err => console.error('Error rendering cosmo-overlay:', err));

            update2DGraphs();
            window.dispatchEvent(new Event('resize')); // Force re-render
        }

        // 2D Graphs with Plotly
        function update2DGraphs() {
            const rootCounts = new Array(9).fill(0);
            mother.forEach(row => row.forEach(root => rootCounts[root - 1]++));
            const colorMap = [
                plotlyColors[1], // cyan
                plotlyColors[2], // lime
                plotlyColors[3], // orange
                plotlyColors[4], // hotpink
                plotlyColors[5], // forestgreen
                plotlyColors[6], // yellow
                plotlyColors[7], // purple
                plotlyColors[8], // gray
                plotlyColors[9]  // red
            ];
            console.log('Bar graph root counts:', rootCounts);
            console.log('Bar graph colors:', colorMap);
            Plotly.newPlot('root-counts', [{
                x: ['1', '2', '3', '4', '5', '6', '7', '8', '9'],
                y: rootCounts,
                type: 'bar',
                marker: { color: colorMap },
                text: rootCounts,
                textposition: 'auto'
            }], {
                title: 'Digital Root Counts',
                xaxis: { title: 'Digital Root' },
                yaxis: { title: 'Count', range: [0, Math.max(...rootCounts) * 1.1] },
                margin: { t: 50 },
                paper_bgcolor: '#0a0a0a',
                plot_bgcolor: '#0a0a0a',
                font: { color: 'white' }
            }, { responsive: true }).catch(err => console.error('Error rendering root-counts:', err));
        }

        // Music Playback with Web Audio API
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        let isPlaying = false;
        const noteMap = {
            1: 272.14, 2: 288.33, 3: 305.47, 4: 323.63, 5: 342.88, 6: 363.27, 7: 384.87, 8: 407.75, 9: 432
        };
        const sectorRanges = [
            { start: 1, end: 9, octave: 0 },
            { start: 10, end: 18, octave: 1 },
            { start: 19, end: 27, octave: 2 },
            { start: 28, end: 36, octave: 3 },
            { start: 37, end: 45, octave: 4 },
            { start: 46, end: 54, octave: 5 },
            { start: 55, end: 63, octave: 6 },
            { start: 64, end: 72, octave: 7 }
        ];

        // Initialize Sequence Selector
        function initSequenceSelector() {
            const sequenceSelect = document.getElementById('sequence-select');
            sequenceSelect.innerHTML = '';
            const allOption = document.createElement('option');
            allOption.value = 'all';
            allOption.textContent = 'All Sequences';
            sequenceSelect.appendChild(allOption);
            for (let i = 1; i <= 72; i++) {
                const option = document.createElement('option');
                option.value = i;
                option.textContent = i === 1 ? 'F' : `${i}F`;
                sequenceSelect.appendChild(option);
            }
            console.log('Sequence Selector initialized with', sequenceSelect.options.length, 'options');
        }

        async function playSong(mode) {
            if (isPlaying) return;
            isPlaying = true;
            if (audioCtx.state === 'suspended') await audioCtx.resume();
            const activeRoots = Array.from(document.querySelectorAll('#root-controls input:checked')).map(input => parseInt(input.value));
            const freqGraphCtx = document.getElementById('freq-canvas').getContext('2d');
            const cymaticsCanvas = document.getElementById('cymaticsCanvas');
            const cymaticsCtx = cymaticsCanvas.getContext('2d');
            const musicGrid = document.getElementById('music-grid');

            async function playNote(i, j, octaveShift) {
                const root = mother[i][j];
                if (activeRoots.includes(root)) {
                    console.log(`Playing sequence ${j + 1} at row ${i + 1}, root ${root}`);
                    musicGrid.querySelectorAll('.cell').forEach(cell => cell.classList.remove('highlight'));
                    const cell = musicGrid.querySelector(`.cell[data-row="${i}"][data-seq="${j}"]`);
                    if (cell) cell.classList.add('highlight');

                    const oscillator = audioCtx.createOscillator();
                    const gainNode = audioCtx.createGain();
                    oscillator.type = 'sine';
                    const freq = noteMap[root] * Math.pow(2, octaveShift);
                    oscillator.frequency.setValueAtTime(freq, audioCtx.currentTime);
                    oscillator.connect(gainNode);
                    gainNode.connect(audioCtx.destination);
                    gainNode.gain.setValueAtTime(0.5, audioCtx.currentTime);
                    oscillator.start();
                    oscillator.stop(audioCtx.currentTime + 0.1);

                    freqGraphCtx.clearRect(0, 0, 600, 400);
                    freqGraphCtx.fillStyle = plotlyColors[root];
                    freqGraphCtx.fillRect(j * 8, 400 - root * 40, 8, root * 40);

                    const resolution = 360;
                    const source = { x: 180, y: 180 };
                    cymaticsCtx.clearRect(0, 0, 360, 360);
                    cymaticsCtx.fillStyle = plotlyColors[root];
                    for (let x = 0; x < 360; x += 360 / resolution) {
                        for (let y = 0; y < 360; y += 360 / resolution) {
                            const k = 2 * Math.PI * freq / 5000;
                            const dist = Math.sqrt((x - source.x) ** 2 + (y - source.y) ** 2);
                            const wave = Math.sin(k * dist + (root / 9) * Math.PI) * (1 + root / 9);
                            if (Math.abs(wave) > 0.4) {
                                cymaticsCtx.fillRect(x, y, 360 / resolution, 360 / resolution);
                            }
                        }
                    }

                    const zData = Array(resolution).fill().map(() => Array(resolution).fill(0));
                    for (let x = 0; x < 360; x += 360 / resolution) {
                        for (let y = 0; y < 360; y += 360 / resolution) {
                            const k = 2 * Math.PI * freq / 5000;
                            const dist = Math.sqrt((x - source.x) ** 2 + (y - source.y) ** 2);
                            const wave = Math.sin(k * dist + (root / 9) * Math.PI) * (1 + root / 9);
                            zData[Math.floor(x / (360 / resolution))][Math.floor(y / (360 / resolution))] = wave;
                        }
                    }
                    Plotly.newPlot('frequency-3d', [{
                        type: 'surface',
                        z: zData,
                        colorscale: [[0, 'black'], [0.4, 'black'], [0.4, plotlyColors[root]], [1, plotlyColors[root]]],
                        showscale: false,
                        name: `Freq ${freq.toFixed(2)} (Root ${root})`
                    }], {
                        title: '3D Cymatics Surface',
                        scene: {
                            xaxis: { title: 'X', range: [0, 360] },
                            yaxis: { title: 'Y', range: [0, 360] },
                            zaxis: { title: 'Amplitude', range: [-2, 2] }
                        },
                        margin: { t: 50, l: 20, r: 20, b: 20 },
                        width: 360,
                        height: 360,
                        paper_bgcolor: '#0a0a0a',
                        plot_bgcolor: '#0a0a0a',
                        font: { color: 'white' }
                    }, { responsive: true }).catch(err => console.error('Error rendering frequency-3d:', err));

                    await new Promise(resolve => setTimeout(resolve, 120));
                    if (cell) cell.classList.remove('highlight');
                } else {
                    await new Promise(resolve => setTimeout(resolve, 120));
                }
            }

            if (mode === 'full') {
                for (let i = 0; i < 72 && isPlaying; i++) {
                    const sector = sectorRanges.find(s => i + 1 >= s.start && i + 1 <= s.end);
                    const octaveShift = sector ? sector.octave : 0;
                    for (let j = 0; j < 72 && isPlaying; j++) {
                        await playNote(i, j, octaveShift);
                    }
                }
            } else if (mode === 'sequences') {
                const selectedSequences = Array.from(document.getElementById('sequence-select').selectedOptions)
                    .map(option => option.value)
                    .filter(value => value !== 'all')
                    .map(Number);
                console.log('Selected sequences:', selectedSequences, 'Playing horizontally: sequence by sequence, left-to-right');
                if (selectedSequences.length === 0) {
                    isPlaying = false;
                    return;
                }
                for (const seq of selectedSequences) {
                    const sector = sectorRanges.find(s => seq >= s.start && seq <= s.end);
                    const octaveShift = sector ? sector.octave : 0;
                    for (let j = 0; j < 72 && isPlaying; j++) {
                        await playNote(seq - 1, j, octaveShift);
                    }
                }
            } else if (mode === 'sector') {
                const sectorIndex = parseInt(document.getElementById('sector-select').value);
                if (sectorIndex === 0) {
                    isPlaying = false;
                    return;
                }
                const sector = sectorRanges[sectorIndex - 1];
                for (let i = sector.start - 1; i < sector.end && isPlaying; i++) {
                    for (let j = 0; j < 72 && isPlaying; j++) {
                        await playNote(i, j, sector.octave);
                    }
                }
            }
            isPlaying = false;
            freqGraphCtx.clearRect(0, 0, 600, 400);
            cymaticsCtx.clearRect(0, 0, 360, 360);
            musicGrid.querySelectorAll('.cell').forEach(cell => cell.classList.remove('highlight'));
            Plotly.newPlot('frequency-3d', [], {});
        }

        function stopSong() {
            isPlaying = false;
            document.getElementById('freq-canvas').getContext('2d').clearRect(0, 0, 600, 400);
            document.getElementById('cymaticsCanvas').getContext('2d').clearRect(0, 0, 360, 360);
            document.getElementById('music-grid').querySelectorAll('.cell').forEach(cell => cell.classList.remove('highlight'));
            Plotly.newPlot('frequency-3d', [], {});
        }

        // 3D View Button Handlers
        function renderGlobe() {
            Plotly.newPlot('globe-plot', document.getElementById('globe-plot').data, document.getElementById('globe-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderCylinder() {
            Plotly.newPlot('cylinder-plot', document.getElementById('cylinder-plot').data, document.getElementById('cylinder-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderSpiral() {
            Plotly.newPlot('spiral-plot', document.getElementById('spiral-plot').data, document.getElementById('spiral-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderFibonacciSpiral() {
            Plotly.newPlot('fibonacci-spiral-plot', document.getElementById('fibonacci-spiral-plot').data, document.getElementById('fibonacci-spiral-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderMobius() {
            Plotly.newPlot('toroidal-mobius-plot', document.getElementById('toroidal-mobius-plot').data, document.getElementById('toroidal-mobius-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderHelicalToroidal() {
            Plotly.newPlot('helical-toroidal-plot', document.getElementById('helical-toroidal-plot').data, document.getElementById('helical-toroidal-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderCube() {
            Plotly.newPlot('cube-plot', document.getElementById('cube-plot').data, document.getElementById('cube-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function render3DPolar() {
            Plotly.newPlot('3d-polar-plot', document.getElementById('3d-polar-plot').data, document.getElementById('3d-polar-plot').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }
        function renderCosmicOverlay() {
            Plotly.newPlot('cosmo-overlay', document.getElementById('cosmo-overlay').data, document.getElementById('cosmo-overlay').layout, { responsive: true });
            window.dispatchEvent(new Event('resize'));
        }

        // Cymatics Generator
        function generateCymatics() {
            playSong('full');
        }

        // Tab Switching
        function switchTab(tabId) {
            document.querySelectorAll('.tab-button').forEach(btn => btn.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
            const tabButton = document.querySelector(`button[onclick="switchTab('${tabId}')"]`);
            if (tabButton) {
                tabButton.classList.add('active');
                console.log(`Tab switched to ${tabId}, button highlighted`);
            } else {
                console.error(`Tab button for ${tabId} not found`);
            }
            const tabContent = document.getElementById(tabId);
            if (tabContent) {
                tabContent.classList.add('active');
            } else {
                console.error(`Tab content for ${tabId} not found`);
            }
            const plotIds = ['globe-plot', 'cylinder-plot', 'spiral-plot', 'fibonacci-spiral-plot', 'toroidal-mobius-plot', 'helical-toroidal-plot', 'cube-plot', 'polar-plot', '3d-polar-plot', 'frequency-3d', 'cosmo-overlay'];
            plotIds.forEach(plotId => Plotly.purge(plotId));
            if (tabId !== 'music' && tabId !== 'cymatics') stopSong();
            if (tabId === 'grid') {
                updateGrid('mother-grid');
                update2DGraphs();
                setTimeout(() => update3DGraphs(1, 3), 3000);
            }
            if (tabId === '3d' || tabId === 'cosmos') {
                setTimeout(() => update3DGraphs(1, 3), 3000);
                window.dispatchEvent(new Event('resize')); // Force Plotly re-render
            }
            if (tabId === 'music') {
                updateGrid('music-grid');
                initSequenceSelector();
                document.getElementById('sequence-select').value = 'all';
                document.getElementById('sector-select').value = '0';
            }
        }

        // Initialize on page load
        document.addEventListener('DOMContentLoaded', function() {
            updateGrid('mother-grid');
            updateGrid('music-grid');
            initSequenceSelector();
            update2DGraphs();
            setTimeout(() => update3DGraphs(1, 3), 3000);
        });
    </script>
</body>
</html>
