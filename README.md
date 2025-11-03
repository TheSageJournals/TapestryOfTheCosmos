<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fibonacci Harmonics Explorer</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.plot.ly/plotly-2.32.0.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/tone"></script>
    <style>
        * {
            box-sizing: border-box;
        }
        body {
            margin: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #0a0a0a;
            color: white;
        }
        header {
            background: #111;
            padding: 10px 15px;
            font-size: 1.2em;
            font-weight: bold;
            text-align: center;
            color: cyan;
        }
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
