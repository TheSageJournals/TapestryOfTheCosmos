<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Golden Rule Primes Sieve</title>
    <script src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f9;
        }
        .container {
            max-width: 1200px;
            margin: auto;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        .warning, #menu {
            background: #fff3cd;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            text-align: center;
        }
        .input-group {
            margin: 10px 0;
        }
        label {
            display: inline-block;
            width: 100px;
            font-weight: bold;
        }
        input[type="number"], input[type="text"] {
            padding: 8px;
            width: 200px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .buttons {
            margin: 20px 0;
            text-align: center;
        }
        button {
            padding: 10px 20px;
            margin: 5px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:disabled {
            background: #ccc;
            cursor: not-allowed;
        }
        button:hover:not(:disabled) {
            background: #0056b3;
        }
        #panicButton {
            background: #dc3545;
        }
        #panicButton:hover:not(:disabled) {
            background: #c82333;
        }
        #output {
            margin-top: 20px;
            padding: 15px;
            background: #e9ecef;
            border-radius: 5px;
            min-height: 100px;
        }
        #memoryMonitor {
            margin: 10px 0;
            padding: 10px;
            background: #e9ecef;
            border-radius: 5px;
            text-align: center;
            font-weight: bold;
        }
        #memoryMonitor.warning {
            background: #fff3cd;
            color: #856404;
        }
        .tab-container {
            margin-top: 20px;
        }
        .tab-buttons {
            display: flex;
            border-bottom: 2px solid #007bff;
        }
        .tab-button {
            flex: 1;
            padding: 10px;
            background: #f8f9fa;
            border: none;
            border-radius: 5px 5px 0 0;
            cursor: pointer;
            text-align: center;
            font-weight: bold;
        }
        .tab-button.active {
            background: #007bff;
            color: white;
        }
        .tab-button:hover:not(.active) {
            background: #e9ecef;
        }
        .tab-content {
            display: none;
            width: 100%;
            height: 500px;
        }
        .tab-content.active {
            display: block;
        }
        .legend {
            margin-top: 20px;
            padding: 10px;
            background: #f8f9fa;
            border-radius: 5px;
            text-align: center;
        }
        .legend-item {
            display: inline-block;
            margin-right: 15px;
        }
        .legend-color {
            display: inline-block;
            width: 15px;
            height: 15px;
            vertical-align: middle;
            margin-right: 5px;
        }
        #loading {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        .loading-bar {
            width: 200px;
            height: 20px;
            background: #ddd;
            border-radius: 10px;
            overflow: hidden;
        }
        .loading-progress {
            width: 0;
            height: 100%;
            background: #007bff;
            animation: loading 2s infinite;
        }
        @keyframes loading {
            0% { width: 0; }
            50% { width: 100%; }
            100% { width: 0; }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Golden Rule Primes Sieve</h1>
        <div id="warning" class="warning">
            <p><strong>Warning:</strong> This tool may be computationally intensive for large ranges! Proceed? (Y/N)</p>
            <input type="text" id="proceedInput" placeholder="Y or N">
            <button onclick="proceed()">Go</button>
        </div>
        <div id="menu" style="display: none;">
            <div id="memoryMonitor">Memory: Not Supported</div>
            <div class="input-group">
                <label for="numberInput">Number:</label>
                <input type="number" id="numberInput" placeholder="Enter a number" min="0">
            </div>
            <div class="input-group">
                <label for="startInput">Start:</label>
                <input type="number" id="startInput" placeholder="Enter start" min="0">
            </div>
            <div class="input-group">
                <label for="endInput">End:</label>
                <input type="number" id="endInput" placeholder="Enter end" min="0">
            </div>
            <div class="buttons">
                <button onclick="checkPrime()">A. Check Prime</button>
                <button onclick="findPrimes()">B. Find Primes</button>
                <button onclick="findNextPrime()">C. Next Prime</button>
                <button id="mersenneButton" onclick="checkMersenne()" disabled>D. Check Mersenne Primes</button>
                <button id="panicButton" onclick="panicStop()">Panic Stop</button>
                <button onclick="showMenu()">Back to Menu</button>
            </div>
            <div id="output"></div>
            <div class="tab-container">
                <div class="tab-buttons">
                    <button class="tab-button active" onclick="openTab('plotPrime')">Prime</button>
                    <button class="tab-button" onclick="openTab('plotDigitalRoot')">Digital Root</button>
                    <button class="tab-button" onclick="openTab('plotBinary')">Binary</button>
                </div>
                <div id="plotPrime" class="tab-content active"></div>
                <div id="plotDigitalRoot" class="tab-content"></div>
                <div id="plotBinary" class="tab-content"></div>
            </div>
            <div class="legend">
                <strong>Digital Root Legend:</strong>
                <span class="legend-item"><span class="legend-color" style="background: cyan;"></span>DR 1</span>
                <span class="legend-item"><span class="legend-color" style="background: green;"></span>DR 2</span>
                <span class="legend-item"><span class="legend-color" style="background: orange;"></span>DR 3</span>
                <span class="legend-item"><span class="legend-color" style="background: magenta;"></span>DR 4</span>
                <span class="legend-item"><span class="legend-color" style="background: forestgreen;"></span>DR 5</span>
                <span class="legend-item"><span class="legend-color" style="background: yellow;"></span>DR 6</span>
                <span class="legend-item"><span class="legend-color" style="background: purple;"></span>DR 7</span>
                <span class="legend-item"><span class="legend-color" style="background: gray;"></span>DR 8</span>
                <span class="legend-item"><span class="legend-color" style="background: red;"></span>DR 9</span>
            </div>
        </div>
        <div id="loading">
            <div class="loading-bar">
                <div class="loading-progress"></div>
            </div>
        </div>
    </div>

    <script>
        // Constants
        const DIGITAL_ROOT_COLORS = {
            1: 'cyan', 2: 'green', 3: 'orange', 4: 'magenta',
            5: 'forestgreen', 6: 'yellow', 7: 'purple',
            8: 'gray', 9: 'red'
        };
        const DIGITAL_ROOT_RGBA = {
            1: [0, 1, 1, 1], 2: [0, 1, 0, 1], 3: [1, 0.5, 0, 1], 4: [1, 0, 1, 1],
            5: [0.13, 0.55, 0.13, 1], 6: [1, 1, 0, 1], 7: [0.5, 0, 0.5, 1],
            8: [0.5, 0.5, 0.5, 1], 9: [1, 0, 0, 1]
        };
        let lastInput = { type: null, number: null, start: null, end: null, primes: null };
        let abortCalculation = false;
        const BATCH_SIZE = 100000;

        // Device Capability Detection
        const cpuCores = navigator.hardwareConcurrency || 2;
        const maxPoints = cpuCores <= 2 ? 5000 : 10000;
        const memoryLimit = (performance.memory && performance.memory.jsHeapSizeLimit) ?
            performance.memory.jsHeapSizeLimit / (1024 * 1024) : 1500; // Default 1.5GB

        // RAM Monitor
        function updateMemoryMonitor() {
            try {
                if (performance.memory) {
                    const used = performance.memory.usedJSHeapSize / (1024 * 1024);
                    const limit = performance.memory.jsHeapSizeLimit / (1024 * 1024);
                    const monitor = document.getElementById('memoryMonitor');
                    monitor.textContent = `Memory: ~${used.toFixed(1)} MB / ${limit.toFixed(1)} MB`;
                    if (used / limit > 0.8) {
                        monitor.classList.add('warning');
                    } else {
                        monitor.classList.remove('warning');
                    }
                }
            } catch (e) {
                console.warn('Memory monitoring not supported');
            }
            setTimeout(updateMemoryMonitor, 1000);
        }
        updateMemoryMonitor();

        // Cache Management
        function savePrimesToCache(start, end, primes) {
            try {
                const key = `primes_${start}_${end}`;
                const data = {
                    primes: primes,
                    timestamp: Date.now()
                };
                localStorage.setItem(key, JSON.stringify(data));
                console.log(`Cached ${primes.length} primes for range [${start}, ${end}]`);
            } catch (e) {
                console.error(`Error caching primes: ${e}`);
                throw new Error('Cache storage limit reached. Computing without caching.');
            }
        }

        function loadPrimesFromCache(start, end) {
            try {
                const key = `primes_${start}_${end}`;
                const data = JSON.parse(localStorage.getItem(key));
                if (data && Date.now() - data.timestamp < 24 * 60 * 60 * 1000) {
                    return data.primes.filter(p => p >= start && p <= end);
                }
            } catch (e) {
                console.error(`Error loading cache: ${e}`);
            }
            return null;
        }

        // Estimate Cache Size
        function getCacheSize() {
            let total = 0;
            for (let key in localStorage) {
                if (key.startsWith('primes_')) {
                    total += ((localStorage[key].length + key.length) * 2) / (1024 * 1024); // MB
                }
            }
            return total.toFixed(2);
        }

        // Loading Bar
        function showLoading() {
            document.getElementById('loading').style.display = 'flex';
        }

        function hideLoading() {
            document.getElementById('loading').style.display = 'none';
        }

        // Tab Management
        function openTab(tabId) {
            document.querySelectorAll('.tab-button').forEach(button => {
                button.classList.remove('active');
            });
            document.querySelectorAll('.tab-content').forEach(content => {
                content.classList.remove('active');
            });
            document.querySelector(`.tab-button[onclick="openTab('${tabId}')"]`).classList.add('active');
            document.getElementById(tabId).classList.add('active');
        }

        // Panic Stop
        function panicStop() {
            abortCalculation = true;
            hideLoading();
            const output = document.getElementById('output');
            output.innerHTML = 'Operation aborted by user.';
            Plotly.purge('plotPrime');
            Plotly.purge('plotDigitalRoot');
            Plotly.purge('plotBinary');
            document.getElementById('mersenneButton').disabled = true;
        }

        // Helper Functions
        function digitalRoot(n) {
            return n === 0 ? 0 : 1 + ((n - 1) % 9);
        }

        function isPrime(n) {
            if (abortCalculation) return false;
            if (n < 2) return false;
            if (n === 2 || n === 3 || n === 5) return true;
            if (n % 2 === 0 || n % 5 === 0) return false;
            for (let i = 3; i <= Math.sqrt(n); i += 2) {
                if (abortCalculation) return false;
                if (n % i === 0) return false;
            }
            return true;
        }

        function isNonRepetitive(n) {
            return n === 11 || new Set(String(n)).size > 1 || String(n).length === 1;
        }

        function isMersennePrime(n) {
            if (abortCalculation) return false;
            if (n < 2 || !isPrime(n)) return false;
            let p = 0;
            let temp = n + 1;
            while (temp % 2 === 0) {
                temp /= 2;
                p++;
            }
            return temp === 1 && isPrime(p);
        }

        function generate3DCoordinates(numbers, single = false) {
            if (abortCalculation) return [[], [], []];
            const t = single ? [numbers] : numbers;
            const x = t.map(n => Math.cos(n / 10) * Math.sqrt(n));
            const y = t.map(n => Math.sin(n / 10) * Math.sqrt(n));
            const z = t.map(n => n / 100);
            return [x, y, z];
        }

        function sieveDigitalRoot(start, end, dr) {
            if (abortCalculation) return [];
            if ([3, 9].includes(dr)) return start <= 3 && 3 <= end ? [3] : [];
            if (dr === 6) return [];
            const remainder = {1: 1, 2: 2, 4: 4, 5: 5, 7: 7, 8: 8}[dr];
            const primes = [];
            start = Math.max(start, dr !== 2 ? 1 : 2);
            for (let n = start + (remainder - (start % 9) + 9) % 9; n <= end; n += 9) {
                if (abortCalculation) return [];
                if ([2, 3, 5, 11].includes(n) && digitalRoot(n) === dr) {
                    primes.push(n);
                    continue;
                }
                if (n % 2 === 0 || n % 5 === 0 || !isNonRepetitive(n)) continue;
                if (digitalRoot(n) === dr && isPrime(n)) primes.push(n);
            }
            return primes;
        }

        function goldenDigitalRootSieve(start, end) {
            if (abortCalculation) return [];
            const primes = new Set();
            for (let dr of [1, 2, 4, 5, 7, 8, 3, 9]) {
                if (abortCalculation) return [];
                sieveDigitalRoot(start, end, dr).forEach(p => primes.add(p));
            }
            return Array.from(primes).sort((a, b) => a - b);
        }

        function getPrimesInRange(start, end) {
            showLoading();
            try {
                const output = document.getElementById('output');
                let allPrimes = [];
                const batchStart = Math.floor(start / BATCH_SIZE) * BATCH_SIZE;
                const batchEnd = Math.ceil((end + 1) / BATCH_SIZE) * BATCH_SIZE;

                for (let batch = batchStart; batch < batchEnd; batch += BATCH_SIZE) {
                    if (abortCalculation) return [];
                    const batchStart = batch;
                    const batchEnd = Math.min(batch + BATCH_SIZE - 1, end);
                    output.innerHTML = `Processing batch ${batchStart.toLocaleString()}–${batchEnd.toLocaleString()}... Cache size: ${getCacheSize()} MB`;
                    const cached = loadPrimesFromCache(batchStart, batchEnd);
                    if (cached && cached[cached.length - 1] >= batchEnd) {
                        console.log(`Loaded ${cached.length} primes from cache for [${batchStart}, ${batchEnd}]`);
                        allPrimes.push(...cached.filter(p => p >= start && p <= end));
                    } else {
                        const primes = goldenDigitalRootSieve(batchStart, batchEnd);
                        if (abortCalculation) return [];
                        try {
                            savePrimesToCache(batchStart, batchEnd, primes);
                        } catch (e) {
                            console.warn(e.message);
                            output.innerHTML += `<p><strong>Warning:</strong> ${e.message}</p>`;
                        }
                        allPrimes.push(...primes.filter(p => p >= start && p <= end));
                    }
                }
                allPrimes = [...new Set(allPrimes)].sort((a, b) => a - b);
                output.innerHTML = `Processed range [${start.toLocaleString()}, ${end.toLocaleString()}]. Cache size: ${getCacheSize()} MB`;
                return allPrimes;
            } finally {
                hideLoading();
            }
        }

        function findNextPrime(n) {
            if (abortCalculation) return n;
            let m = n + 1;
            while (!isPrime(m)) {
                if (abortCalculation) return n;
                m++;
            }
            return m;
        }

        // Plotting Function
        function plotThreeViews(numbers, primes, single = false, mersennePrimes = []) {
            showLoading();
            try {
                if (abortCalculation) return [[], [], [], [], [], [], []];
                const nums = single ? [numbers] : numbers;
                const primeSet = new Set(primes);
                const mersenneSet = new Set(mersennePrimes);
                let plotNums = nums;
                let step = 1;

                // Subsample for large ranges
                if (!single && nums.length > maxPoints) {
                    step = Math.ceil(nums.length / maxPoints);
                    plotNums = nums.filter((_, i) => primeSet.has(nums[i]) || i % step === 0);
                }

                const [x, y, z] = generate3DCoordinates(plotNums, single);
                const n_points = plotNums.length;

                // Calculate dot size based on range
                const rangeSize = single ? 1 : (nums[nums.length - 1] - nums[0] + 1);
                const pointSize = Math.max(0.5, Math.min(2.0, 2.5 - 0.3 * Math.log10(Math.max(10, rangeSize))));

                // Initialize color arrays
                const colorsPrime = Array(n_points).fill('rgba(128,128,128,0.5)'); // Gray for non-primes
                const colorsDigitalRoot = Array(n_points).fill('rgba(255,255,255,0.5)'); // White for non-primes
                const colorsBinary = Array(n_points).fill('rgba(255,255,255,0.5)'); // White for non-primes

                // Assign colors
                plotNums.forEach((n, i) => {
                    if (primeSet.has(n)) {
                        colorsPrime[i] = 'rgba(255,128,0,1)'; // Orange for primes
                        const dr = digitalRoot(n);
                        const rgba = DIGITAL_ROOT_RGBA[dr] || [1, 1, 1, 0.5];
                        colorsDigitalRoot[i] = `rgba(${rgba[0]*255},${rgba[1]*255},${rgba[2]*255},${rgba[3]})`; // Digital root color
                        colorsBinary[i] = 'rgba(0,0,0,1)'; // Black for primes
                    }
                    if (mersenneSet.has(n)) {
                        colorsPrime[i] = 'rgba(0,0,255,1)'; // Blue for Mersenne primes
                        colorsDigitalRoot[i] = 'rgba(255,215,0,1)'; // Gold for Mersenne primes
                        colorsBinary[i] = 'rgba(255,0,0,1)'; // Red for Mersenne primes
                    }
                });

                // Plot configurations with 20% larger scale
                const maxXY = Math.max(...x.map(Math.abs), ...y.map(Math.abs)) * 1.2; // 20% larger
                const maxZ = Math.max(...z) * 1.2 || 1; // 20% larger
                const scene = {
                    xaxis: { title: 'X', range: [-maxXY, maxXY] },
                    yaxis: { title: 'Y', range: [-maxXY, maxXY] },
                    zaxis: { title: 'Z', range: [0, maxZ] },
                    aspectmode: 'cube'
                };
                const layout = {
                    scene: scene,
                    height: 500,
                    width: '100%',
                    margin: { t: 50, b: 20, l: 20, r: 20 },
                    showlegend: true
                };

                // Plot Prime
                Plotly.newPlot('plotPrime', [{
                    type: 'scatter3d',
                    mode: 'markers',
                    x: x,
                    y: y,
                    z: z,
                    marker: { size: pointSize, color: colorsPrime, opacity: 0.5 },
                    name: 'Primes (Orange), Non-Primes (Gray), Mersenne (Blue)'
                }], {
                    ...layout,
                    title: single ? `Prime Check: ${numbers} ${primeSet.has(numbers) ? '(Prime)' : '(Non-Prime)'}` : `Primes in [${numbers[0]}, ${numbers[numbers.length-1]}]`
                });

                // Plot Digital Root
                Plotly.newPlot('plotDigitalRoot', [{
                    type: 'scatter3d',
                    mode: 'markers',
                    x: x,
                    y: y,
                    z: z,
                    marker: { size: pointSize, color: colorsDigitalRoot, opacity: 0.5 },
                    name: 'Primes (Digital Root), Non-Primes (White), Mersenne (Gold)'
                }], {
                    ...layout,
                    title: single ? 'Digital Root View' : 'Digital Root'
                });

                // Plot Binary
                Plotly.newPlot('plotBinary', [{
                    type: 'scatter3d',
                    mode: 'markers',
                    x: x,
                    y: y,
                    z: z,
                    marker: { size: pointSize, color: colorsBinary, opacity: 0.5 },
                    name: 'Primes (Black), Non-Primes (White), Mersenne (Red)'
                }], {
                    ...layout,
                    title: single ? 'Binary View' : 'Binary View'
                });

                // Ensure Prime tab is active
                openTab('plotPrime');

                return [plotNums, x, y, z, colorsPrime, colorsDigitalRoot, colorsBinary];
            } finally {
                hideLoading();
            }
        }

        // UI Functions
        function proceed() {
            const input = document.getElementById('proceedInput').value.trim().toUpperCase();
            const output = document.getElementById('output');
            output.innerHTML = '';
            if (input === 'Y') {
                document.getElementById('warning').style.display = 'none';
                document.getElementById('menu').style.display = 'block';
            } else if (input === 'N') {
                output.innerHTML = 'Operation cancelled.';
            } else {
                output.innerHTML = 'Please enter Y or N.';
            }
        }

        function showMenu() {
            abortCalculation = false;
            const output = document.getElementById('output');
            output.innerHTML = '';
            document.getElementById('numberInput').value = '';
            document.getElementById('startInput').value = '';
            document.getElementById('endInput').value = '';
            document.getElementById('mersenneButton').disabled = true;
            Plotly.purge('plotPrime');
            Plotly.purge('plotDigitalRoot');
            Plotly.purge('plotBinary');
            lastInput = { type: null, number: null, start: null, end: null, primes: null };
            openTab('plotPrime');
        }

        function checkPrime() {
            abortCalculation = false;
            showLoading();
            try {
                const n = parseInt(document.getElementById('numberInput').value);
                const output = document.getElementById('output');
                output.innerHTML = '';
                if (isNaN(n) || n < 0) {
                    output.innerHTML = 'Invalid input. Enter a non-negative integer.';
                    return;
                }
                lastInput = { type: 'single', number: n, start: null, end: null, primes: null };
                document.getElementById('mersenneButton').disabled = false;
                const isPrimeResult = isPrime(n);
                if (abortCalculation) {
                    output.innerHTML = 'Operation aborted.';
                    return;
                }
                const [nums, x, y, z] = plotThreeViews(n, isPrimeResult ? [n] : [], true);
                if (abortCalculation) {
                    output.innerHTML = 'Operation aborted.';
                    return;
                }
                output.innerHTML = `Is ${n} prime? ${isPrimeResult ? 'Yes' : 'No'}<br>Position: (${x[0].toFixed(2)}, ${y[0].toFixed(2)}, ${z[0].toFixed(2)})`;
            } finally {
                hideLoading();
            }
        }

        function findPrimes() {
            abortCalculation = false;
            showLoading();
            try {
                const start = parseInt(document.getElementById('startInput').value);
                const end = parseInt(document.getElementById('endInput').value);
                const output = document.getElementById('output');
                output.innerHTML = '';
                if (isNaN(start) || isNaN(end) || start < 0 || start > end) {
                    output.innerHTML = 'Invalid input. Enter non-negative integers with start ≤ end.';
                    return;
                }
                if (end - start > 1000000) {
                    output.innerHTML += '<p><strong>Warning:</strong> Large range may impact performance. Use the Panic Button to stop if needed.</p>';
                }
                const primes = getPrimesInRange(start, end);
                if (abortCalculation) {
                    output.innerHTML = 'Operation aborted.';
                    return;
                }
                lastInput = { type: 'range', number: null, start: start, end: end, primes: primes };
                document.getElementById('mersenneButton').disabled = false;
                const numbers = Array.from({length: end - start + 1}, (_, i) => start + i);
                const [nums, x, y, z] = plotThreeViews(numbers, primes, false);
                if (abortCalculation) {
                    output.innerHTML = 'Operation aborted.';
                    return;
                }
                let displayText = `Count: ${primes.length} primes<br>Primes: ${primes.slice(0, 10).join(', ')}${primes.length > 10 ? '...' : ''}`;
                primes.slice(0, 10).forEach((p, i) => {
                    const idx = numbers.indexOf(p);
                    if (idx !== -1 && idx < x.length) {
                        displayText += `<br>Prime ${p}: (${x[idx].toFixed(2)}, ${y[idx].toFixed(2)}, ${z[idx].toFixed(2)})`;
                    }
                });
                output.innerHTML = displayText;
            } finally {
                hideLoading();
            }
        }

        function findNextPrime() {
            abortCalculation = false;
            showLoading();
            try {
                const n = parseInt(document.getElementById('numberInput').value);
                const output = document.getElementById('output');
                output.innerHTML = '';
                if (isNaN(n) || n < 0) {
                    output.innerHTML = 'Invalid input. Enter a non-negative integer.';
                    return;
                }
                const nextPrime = findNextPrime(n);
                if (abortCalculation) {
                    output.innerHTML = 'Operation aborted.';
                    return;
                }
                lastInput = { type: 'single', number: nextPrime, start: null, end: null, primes: null };
                document.getElementById('mersenneButton').disabled = false;
                const [nums, x, y, z] = plotThreeViews(nextPrime, [nextPrime], true);
                if (abortCalculation) {
                    output.innerHTML = 'Operation aborted.';
                    return;
                }
                output.innerHTML = `Next prime after ${n} is ${nextPrime}<br>Position: (${x[0].toFixed(2)}, ${y[0].toFixed(2)}, ${z[0].toFixed(2)})`;
            } finally {
                hideLoading();
            }
        }

        function checkMersenne() {
            abortCalculation = false;
            showLoading();
            try {
                const output = document.getElementById('output');
                output.innerHTML = '';
                if (lastInput.type === 'single') {
                    const n = lastInput.number;
                    const isMersenne = isMersennePrime(n);
                    if (abortCalculation) {
                        output.innerHTML = 'Operation aborted.';
                        return;
                    }
                    const primes = isPrime(n) ? [n] : [];
                    const mersennePrimes = isMersenne ? [n] : [];
                    const [nums, x, y, z] = plotThreeViews(n, primes, true, mersennePrimes);
                    if (abortCalculation) {
                        output.innerHTML = 'Operation aborted.';
                        return;
                    }
                    let resultText = `${n} is ${isMersenne ? 'a' : 'not a'} Mersenne prime.`;
                    if (isMersenne) {
                        resultText += `<br>Position: (${x[0].toFixed(2)}, ${y[0].toFixed(2)}, ${z[0].toFixed(2)})`;
                    }
                    output.innerHTML = resultText;
                } else {
                    const primes = lastInput.primes || [];
                    const mersennePrimes = primes.filter(p => isMersennePrime(p));
                    if (abortCalculation) {
                        output.innerHTML = 'Operation aborted.';
                        return;
                    }
                    const numbers = Array.from({length: lastInput.end - lastInput.start + 1}, (_, i) => lastInput.start + i);
                    const [nums, x, y, z] = plotThreeViews(numbers, primes, false, mersennePrimes);
                    if (abortCalculation) {
                        output.innerHTML = 'Operation aborted.';
                        return;
                    }
                    let resultText = `Mersenne primes in [${lastInput.start}, ${lastInput.end}]: ${mersennePrimes.join(', ') || 'None'}<br>Count: ${mersennePrimes.length}`;
                    mersennePrimes.slice(0, 10).forEach((p, i) => {
                        const idx = numbers.indexOf(p);
                        if (idx !== -1 && idx < x.length) {
                            resultText += `<br>Position of ${p}: (${x[idx].toFixed(2)}, ${y[idx].toFixed(2)}, ${z[idx].toFixed(2)})`;
                        }
                    });
                    output.innerHTML = resultText;
                }
            } finally {
                hideLoading();
            }
        }
    </script>
</body>
</html>
