<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Φ Field - Full Lattice Explorer</title>
    <style>
        body { margin: 0; padding: 0; background-color: #1a1a1a; color: white; font-family: sans-serif; }
        input[type="range"] { accent-color: #2563eb; }
        button:hover { opacity: 0.9; }
        select { cursor: pointer; }
    </style>
</head>
<body>
    <div id="app" style="width: 100%; min-height: 100vh; background-color: #1a1a1a; padding: 16px;">
        <div style="max-width: 1280px; margin: 0 auto; display: flex; flex-direction: column; gap: 16px;">
            <div>
                <h2 style="font-size: 24px; font-weight: bold; color: white; margin-bottom: 4px;">Φ Field - Full Lattice Explorer</h2>
                <p style="color: #d1d5db; font-size: 12px;">Interactive 3D lattice slices or projections • 360³ domain • 48 time units (2 Pisano cycles) • Fourier integration</p>
            </div>
            
            <div style="display: flex; justify-content: center;">
                <canvas id="canvas" width="720" height="720" style="border: 2px solid #4b5563; border-radius: 8px; cursor: crosshair; background-color: #0f172a;"></canvas>
            </div>
            
            <div id="controls" style="background-color: #1f2937; padding: 16px; border-radius: 8px; display: flex; flex-direction: column; gap: 16px;">
                <div style="display: flex; gap: 12px; flex-wrap: wrap; align-items: center;">
                    <button id="playPauseBtn" style="display: flex; align-items: center; gap: 8px; padding: 8px 16px; background-color: #2563eb; color: white; border-radius: 4px; cursor: pointer; border: none;">Play</button>
                    
                    <button id="resetBtn" style="display: flex; align-items: center; gap: 8px; padding: 8px 16px; background-color: #4b5563; color: white; border-radius: 4px; cursor: pointer; border: none;">Reset</button>
                    
                    <select id="latticeSizeSelect" style="padding: 8px 12px; background-color: #374151; color: white; border-radius: 4px; cursor: pointer; border: none;">
                        <option value="72">72³ Lattice</option>
                        <option value="144">144³ Lattice</option>
                        <option value="360">360³ Lattice</option>
                    </select>
                    
                    <select id="projectionTypeSelect" style="padding: 8px 12px; background-color: #374151; color: white; border-radius: 4px; cursor: pointer; border: none;">
                        <option value="slice">Slice</option>
                        <option value="max">Max Projection</option>
                        <option value="min">Min Projection</option>
                        <option value="avg">Avg Projection</option>
                    </select>
                    
                    <div style="display: flex; gap: 8px;">
                        <button id="heatmapBtn" class="viz-btn" style="display: flex; align-items: center; gap: 4px; padding: 8px 12px; border-radius: 4px; background-color: #7c3aed; color: white; cursor: pointer; border: none;">Heat</button>
                        <button id="contourBtn" class="viz-btn" style="display: flex; align-items: center; gap: 4px; padding: 8px 12px; border-radius: 4px; background-color: #374151; color: #d1d5db; cursor: pointer; border: none;">Contour</button>
                        <button id="digitalBtn" class="viz-btn" style="display: flex; align-items: center; gap: 4px; padding: 8px 12px; border-radius: 4px; background-color: #374151; color: #d1d5db; cursor: pointer; border: none;">Roots</button>
                        <button id="3dBtn" class="viz-btn" style="display: flex; align-items: center; gap: 4px; padding: 8px 12px; border-radius: 4px; background-color: #374151; color: #d1d5db; cursor: pointer; border: none;">3D</button>
                        <button id="fourierBtn" class="viz-btn" style="display: flex; align-items: center; gap: 4px; padding: 8px 12px; border-radius: 4px; background-color: #374151; color: #d1d5db; cursor: pointer; border: none;">Fourier</button>
                    </div>
                    
                    <label style="display: flex; align-items: center; gap: 8px; color: white; font-size: 12px;">
                        <input id="showGridCheckbox" type="checkbox" style="width: 16px; height: 16px;">
                        Show Grid
                    </label>
                </div>
                
                <div style="display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 8px;">
                    <button id="xyPlaneBtn" class="axis-btn" style="padding: 8px 12px; border-radius: 4px; font-size: 12px; background-color: #0d9488; color: white; cursor: pointer; border: none;">XY Plane</button>
                    <button id="xzPlaneBtn" class="axis-btn" style="padding: 8px 12px; border-radius: 4px; font-size: 12px; background-color: #374151; color: #d1d5db; cursor: pointer; border: none;">XZ Plane</button>
                    <button id="yzPlaneBtn" class="axis-btn" style="padding: 8px 12px; border-radius: 4px; font-size: 12px; background-color: #374151; color: #d1d5db; cursor: pointer; border: none;">YZ Plane</button>
                </div>
                
                <div id="slicePositionDiv" style="display: flex; flex-direction: column; gap: 8px;">
                    <label id="sliceLabel" style="color: white; font-size: 12px;">Slice Position (Z): 180.0</label>
                    <input id="sliceSlider" type="range" min="0" max="360" step="0.5" value="180" style="width: 100%;">
                </div>
                
                <div style="display: flex; flex-direction: column; gap: 8px;">
                    <label id="timeLabel" style="color: white; font-size: 12px;">Time: 0.00 / 48</label>
                    <input id="timeSlider" type="range" min="0" max="48" step="0.1" value="0" style="width: 100%;">
                </div>
            </div>
            
            <div style="background-color: #1f2937; padding: 12px; border-radius: 4px; color: #d1d5db; font-size: 10px; display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 12px;">
                <div><strong>S₀:</strong> 576</div>
                <div><strong>Scale (s):</strong> 5</div>
                <div><strong>Modes:</strong> 39</div>
                <div><strong>Lattice:</strong> 72³</div>
                <div><strong>Domain:</strong> 0-360³</div>
                <div><strong>Pisano:</strong> 24×2</div>
            </div>
        </div>
    </div>

    <script>
        // Parameters from TotC
        const S0 = 576;
        const A_m = [432, 432, 720, 432, 432, 720, 432, 432, 1152];
        const s = 5;
        const pairs = [[1,8], [2,7], [3,6], [4,5]];
        const A_pair = [36, 36, 100, 36];

        let time = 0;
        let isPlaying = false;
        let vizMode = 'heatmap';
        let viewAxis = 'z';
        let slicePos = 180;
        let showGrid = false;
        let selectedPoint = null;
        let latticeSize = 72;
        let projectionType = 'slice';
        let animationInterval = null;

        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        const phi = (x, y, z, t) => {
            let total = S0;
            
            // Individual terms
            for (let n = 0; n < 3; n++) {
                for (let m = 1; m <= 9; m++) {
                    const amp = A_m[m-1] / Math.pow(s, n);
                    const freq_t = (m / 24.0) * Math.pow(s, n);
                    const freq_s = (m / 360.0) * Math.pow(s, n);
                    const phase = 2 * Math.PI * (m - 1) / 9 + 2 * Math.PI * n / 9;
                    const arg = 2 * Math.PI * (freq_t * t + freq_s * x + freq_s * y + freq_s * z) + phase;
                    total += amp * Math.sin(arg);
                }
            }
            
            // Pair terms (all pairs sum to 9)
            for (let idx = 0; idx < pairs.length; idx++) {
                const [m, nn] = pairs[idx];
                for (let n = 0; n < 3; n++) {
                    const amp = A_pair[idx] / Math.pow(s, n);
                    const freq_t = ((m + nn) / 48.0) * Math.pow(s, n);
                    const freq_s = ((m + nn) / 720.0) * Math.pow(s, n);
                    const phase = 2 * Math.PI * (m + nn - 2) / 18 + 2 * Math.PI * n / 18;
                    const arg = 2 * Math.PI * (freq_t * t + freq_s * x + freq_s * y + freq_s * z) + phase;
                    total += amp * Math.sin(arg);
                }
            }
            
            return total;
        };

        const digitalRoot = (num) => {
            let n = Math.abs(Math.round(num));
            while (n > 9) {
                n = n.toString().split('').reduce((a, b) => a + parseInt(b), 0);
            }
            return n === 0 ? 9 : n;
        };

        const valueToColor = (val, min, max) => {
            const normalized = Math.max(0, Math.min(1, (val - min) / (max - min)));
            let r, g, b;
            if (normalized < 0.25) {
                const t = normalized / 0.25;
                r = 0; g = Math.floor(t * 255); b = 255;
            } else if (normalized < 0.5) {
                const t = (normalized - 0.25) / 0.25;
                r = 0; g = 255; b = Math.floor((1 - t) * 255);
            } else if (normalized < 0.75) {
                const t = (normalized - 0.5) / 0.25;
                r = Math.floor(t * 255); g = 255; b = 0;
            } else {
                const t = (normalized - 0.75) / 0.25;
                r = 255; g = Math.floor((1 - t) * 255); b = 0;
            }
            return `rgb(${r},${g},${b})`;
        };

        const digitalRootColor = (root) => {
            const colors = [
                '#00FFFF', // 1 - Cyan
                '#00FF00', // 2 - Green
                '#FFA500', // 3 - Orange
                '#FFC0CB', // 4 - Pink
                '#228B22', // 5 - Forest Green
                '#FFFF00', // 6 - Yellow
                '#800080', // 7 - Purple
                '#808080', // 8 - Gray
                '#FF0000'  // 9 - Red
            ];
            return colors[root - 1] || '#FFFFFF';
        };

        // Simple radix-2 FFT (forward dir=1, inverse dir=-1)
        const fft1d = (dir, real, imag) => {
            const n = real.length;
            if (n <= 1) return;

            // Bit reversal
            let j = 0;
            for (let i = 1; i < n; i++) {
                let bit = n >> 1;
                while (j >= bit) {
                    j -= bit;
                    bit >>= 1;
                }
                j += bit;
                if (i < j) {
                    [real[i], real[j]] = [real[j], real[i]];
                    [imag[i], imag[j]] = [imag[j], imag[i]];
                }
            }

            // Butterfly
            for (let len = 2; len <= n; len <<= 1) {
                const ang = (2 * Math.PI / len) * dir;
                const wlen_r = Math.cos(ang);
                const wlen_i = Math.sin(ang);
                for (let i = 0; i < n; i += len) {
                    let wr = 1;
                    let wi = 0;
                    for (let k = 0; k < len / 2; k++) {
                        const u_r = real[i + k];
                        const u_i = imag[i + k];
                        const v_r = wr * real[i + k + len / 2] - wi * imag[i + k + len / 2];
                        const v_i = wr * imag[i + k + len / 2] + wi * real[i + k + len / 2];
                        real[i + k] = u_r + v_r;
                        imag[i + k] = u_i + v_i;
                        real[i + k + len / 2] = u_r - v_r;
                        imag[i + k + len / 2] = u_i - v_i;
                        const wtr = wr * wlen_r - wi * wlen_i;
                        const wti = wr * wlen_i + wi * wlen_r;
                        wr = wtr;
                        wi = wti;
                    }
                }
            }

            if (dir === -1) {
                const scale = 1 / n;
                for (let i = 0; i < n; i++) {
                    real[i] *= scale;
                    imag[i] *= scale;
                }
            }
        };

        function draw() {
            const width = canvas.width;
            const height = canvas.height;
            
            // Clear canvas
            ctx.clearRect(0, 0, width, height);
            ctx.fillStyle = '#1a1a1a';
            ctx.fillRect(0, 0, width, height);
            
            const samplingRes = vizMode === 'fourier' 
                ? Math.pow(2, Math.ceil(Math.log2(Math.max(128, latticeSize)))) 
                : latticeSize;
            
            const resolution = width / samplingRes;
            const values = [];
            let minVal = Infinity;
            let maxVal = -Infinity;
            
            // Determine dimensions
            let dim1, dim2, dim3;
            if (viewAxis === 'z') {
                dim1 = 'x'; dim2 = 'y'; dim3 = 'z';
            } else if (viewAxis === 'y') {
                dim1 = 'x'; dim2 = 'z'; dim3 = 'y';
            } else {
                dim1 = 'y'; dim2 = 'z'; dim3 = 'x';
            }
            
            const isSlice = projectionType === 'slice';
            const step3 = isSlice ? 1 : Math.max(1, Math.ceil(samplingRes / 72));
            const numSamples3 = isSlice ? 1 : Math.floor((samplingRes - 1) / step3) + 1;
            
            // Compute field values or Fourier
            if (vizMode === 'fourier') {
                const N = samplingRes; // Power of 2
                const real = Array.from({length: N}, () => new Array(N).fill(0));
                const imag = Array.from({length: N}, () => new Array(N).fill(0));
                
                // Sample the slice or projection
                for (let i = 0; i < N; i++) {
                    for (let j = 0; j < N; j++) {
                        let pos1 = (i / (N - 1)) * 360;
                        let pos2 = (j / (N - 1)) * 360;
                        let acc = 0;
                        let localMin = Infinity;
                        let localMax = -Infinity;
                        let count = 0;
                        for (let k = 0; k < numSamples3; k++) {
                            let kk = isSlice ? 0 : k * step3;
                            let pos3 = isSlice ? slicePos : (kk / (N - 1)) * 360;
                            let x = dim1 === 'x' ? pos1 : dim2 === 'x' ? pos2 : pos3;
                            let y = dim1 === 'y' ? pos1 : dim2 === 'y' ? pos2 : pos3;
                            let z = dim1 === 'z' ? pos1 : dim2 === 'z' ? pos2 : pos3;
                            const val = phi(x, y, z, time);
                            if (projectionType === 'max') localMax = Math.max(localMax, val);
                            else if (projectionType === 'min') localMin = Math.min(localMin, val);
                            else acc += val;
                            count++;
                        }
                        let finalVal;
                        if (isSlice || projectionType === 'avg') finalVal = acc / count;
                        else if (projectionType === 'max') finalVal = localMax;
                        else if (projectionType === 'min') finalVal = localMin;
                        real[i][j] = finalVal;
                    }
                }
                
                // 2D FFT: rows then columns
                for (let i = 0; i < N; i++) {
                    fft1d(1, real[i], imag[i]);
                }
                for (let j = 0; j < N; j++) {
                    const col_real = real.map(row => row[j]);
                    const col_imag = imag.map(row => row[j]);
                    fft1d(1, col_real, col_imag);
                    for (let i = 0; i < N; i++) {
                        real[i][j] = col_real[i];
                        imag[i][j] = col_imag[i];
                    }
                }
                
                // Compute magnitude, log scale, find min/max
                for (let i = 0; i < N; i++) {
                    for (let j = 0; j < N; j++) {
                        const mag = Math.sqrt(real[i][j] ** 2 + imag[i][j] ** 2);
                        const logMag = Math.log(1 + mag);
                        values.push(logMag);
                        minVal = Math.min(minVal, logMag);
                        maxVal = Math.max(maxVal, logMag);
                    }
                }
                
                // Draw shifted spectrum (DC in center)
                for (let i = 0; i < N; i++) {
                    for (let j = 0; j < N; j++) {
                        const shiftI = (i + N / 2) % N;
                        const shiftJ = (j + N / 2) % N;
                        const val = values[shiftI * N + shiftJ];
                        ctx.fillStyle = valueToColor(val, minVal, maxVal);
                        ctx.fillRect(j * resolution, i * resolution, resolution + 0.1, resolution + 0.1);
                    }
                }
            } else {
                // Standard modes: compute values
                for (let j = 0; j < samplingRes; j++) {
                    for (let i = 0; i < samplingRes; i++) {
                        let pos1 = (i / (samplingRes - 1)) * 360;
                        let pos2 = (j / (samplingRes - 1)) * 360;
                        let acc = 0;
                        let localMin = Infinity;
                        let localMax = -Infinity;
                        let count = 0;
                        for (let k = 0; k < numSamples3; k++) {
                            let kk = isSlice ? 0 : k * step3;
                            let pos3 = isSlice ? slicePos : (kk / (samplingRes - 1)) * 360;
                            let x = dim1 === 'x' ? pos1 : dim2 === 'x' ? pos2 : pos3;
                            let y = dim1 === 'y' ? pos1 : dim2 === 'y' ? pos2 : pos3;
                            let z = dim1 === 'z' ? pos1 : dim2 === 'z' ? pos2 : pos3;
                            const val = phi(x, y, z, time);
                            if (projectionType === 'max') localMax = Math.max(localMax, val);
                            else if (projectionType === 'min') localMin = Math.min(localMin, val);
                            else acc += val;
                            count++;
                        }
                        let finalVal;
                        if (isSlice || projectionType === 'avg') finalVal = acc / count;
                        else if (projectionType === 'max') finalVal = localMax;
                        else if (projectionType === 'min') finalVal = localMin;
                        values.push(finalVal);
                        minVal = Math.min(minVal, finalVal);
                        maxVal = Math.max(maxVal, finalVal);
                    }
                }
                
                const getIndex = (i, j) => j * samplingRes + i;
                
                // Draw based on mode
                if (vizMode === 'heatmap' || vizMode === 'contour') {
                    for (let j = 0; j < samplingRes; j++) {
                        for (let i = 0; i < samplingRes; i++) {
                            const val = values[getIndex(i, j)];
                            ctx.fillStyle = valueToColor(val, minVal, maxVal);
                            ctx.fillRect(i * resolution, j * resolution, resolution + 0.1, resolution + 0.1);
                        }
                    }
                    
                    if (vizMode === 'contour') {
                        // Improved contour (simple threshold lines)
                        const levels = 10;
                        ctx.lineWidth = 1;
                        for (let level = 0; level < levels; level++) {
                            const threshold = minVal + (maxVal - minVal) * (level / levels);
                            ctx.strokeStyle = 'rgba(255, 255, 255, 0.3)';
                            
                            for (let j = 0; j < samplingRes - 1; j++) {
                                for (let i = 0; i < samplingRes - 1; i++) {
                                    const v00 = values[getIndex(i, j)];
                                    const v10 = values[getIndex(i + 1, j)];
                                    const v01 = values[getIndex(i, j + 1)];
                                    const v11 = values[getIndex(i + 1, j + 1)];
                                    
                                    // Horizontal
                                    if ((v00 < threshold) !== (v10 < threshold)) {
                                        const t = (threshold - v00) / (v10 - v00);
                                        ctx.beginPath();
                                        ctx.moveTo((i + t) * resolution, j * resolution);
                                        ctx.lineTo((i + t) * resolution, (j + 1) * resolution);
                                        ctx.stroke();
                                    }
                                    
                                    // Vertical
                                    if ((v00 < threshold) !== (v01 < threshold)) {
                                        const t = (threshold - v00) / (v01 - v00);
                                        ctx.beginPath();
                                        ctx.moveTo(i * resolution, (j + t) * resolution);
                                        ctx.lineTo((i + 1) * resolution, (j + t) * resolution);
                                        ctx.stroke();
                                    }
                                }
                            }
                        }
                    }
                } else if (vizMode === 'digital') {
                    for (let j = 0; j < samplingRes; j++) {
                        for (let i = 0; i < samplingRes; i++) {
                            const val = values[getIndex(i, j)];
                            const root = digitalRoot(val);
                            ctx.fillStyle = digitalRootColor(root);
                            ctx.fillRect(i * resolution, j * resolution, resolution + 0.1, resolution + 0.1);
                            
                            if (resolution > 6) {
                                ctx.fillStyle = 'rgba(0, 0, 0, 0.7)';
                                ctx.font = `${Math.min(resolution * 0.6, 12)}px monospace`;
                                ctx.textAlign = 'center';
                                ctx.textBaseline = 'middle';
                                ctx.fillText(root, (i + 0.5) * resolution, (j + 0.5) * resolution);
                            }
                        }
                    }
                } else if (vizMode === '3d') {
                    const points = [];
                    if (isSlice) {
                        // Surface plot for slice
                        const step = Math.max(1, Math.floor(samplingRes / 60)); // About 60x60 points for performance
                        for (let j = 0; j < samplingRes; j += step) {
                            for (let i = 0; i < samplingRes; i += step) {
                                const val = values[getIndex(Math.min(i, samplingRes - 1), Math.min(j, samplingRes - 1))];
                                const normX = (i / (samplingRes - 1)) * 2 - 1;
                                const normY = (j / (samplingRes - 1)) * 2 - 1;
                                const heightScale = ((val - minVal) / (maxVal - minVal) * 2 - 1) * 0.2;
                                
                                const rotY = time * 0.05;
                                const rotX = 0.5; // Fixed tilt for better view
                                
                                // Simple 3D projection
                                let x3d = normX * Math.cos(rotY) - heightScale * Math.sin(rotY);
                                let y3d = normY * Math.cos(rotX) - heightScale * Math.sin(rotX);
                                let z3d = normX * Math.sin(rotY) + heightScale * Math.cos(rotY) + normY * Math.sin(rotX);
                                
                                const perspective = width / (width + z3d * 200);
                                const screenX = width / 2 + x3d * perspective * (width / 2);
                                const screenY = height / 2 + y3d * perspective * (height / 2);
                                
                                points.push({ x: screenX, y: screenY, z: z3d, val });
                            }
                        }
                    } else {
                        // Full volume scatter
                        const step3d = Math.max(1, Math.ceil(latticeSize / 30));
                        for (let ii = 0; ii < latticeSize; ii += step3d) {
                            for (let jj = 0; jj < latticeSize; jj += step3d) {
                                for (let kk = 0; kk < latticeSize; kk += step3d) {
                                    const x = (ii / (latticeSize - 1)) * 360;
                                    const y = (jj / (latticeSize - 1)) * 360;
                                    const z = (kk / (latticeSize - 1)) * 360;
                                    const val = phi(x, y, z, time);
                                    minVal = Math.min(minVal, val);
                                    maxVal = Math.max(maxVal, val);
                                    const normX = (ii / (latticeSize - 1)) * 2 - 1;
                                    const normY = (jj / (latticeSize - 1)) * 2 - 1;
                                    const normZ = (kk / (latticeSize - 1)) * 2 - 1;
                                    const rotY = time * 0.05;
                                    const rotX = 0.5;
                                    let x3d = normX * Math.cos(rotY) - normZ * Math.sin(rotY);
                                    let y3d = normY * Math.cos(rotX) - normZ * Math.sin(rotX);
                                    let z3d = normX * Math.sin(rotY) + normZ * Math.cos(rotY) + normY * Math.sin(rotX);
                                    const perspective = width / (width + z3d * 200);
                                    const screenX = width / 2 + x3d * perspective * (width / 2);
                                    const screenY = height / 2 + y3d * perspective * (height / 2);
                                    points.push({ x: screenX, y: screenY, z: z3d, val });
                                }
                            }
                        }
                    }
                    
                    points.sort((a, b) => a.z - b.z);
                    
                    for (const pt of points) {
                        const size = 3 * (1 + (pt.z + 1) / 2);
                        ctx.fillStyle = valueToColor(pt.val, minVal, maxVal);
                        ctx.beginPath();
                        ctx.arc(pt.x, pt.y, size, 0, Math.PI * 2);
                        ctx.fill();
                    }
                }
            }
            
            // Draw grid overlay
            if (showGrid && vizMode !== 'digital' && vizMode !== 'fourier') {
                ctx.strokeStyle = 'rgba(255, 255, 255, 0.15)';
                ctx.lineWidth = 0.5;
                for (let i = 0; i <= samplingRes; i++) {
                    const pos = i * resolution;
                    ctx.beginPath();
                    ctx.moveTo(pos, 0);
                    ctx.lineTo(pos, height);
                    ctx.stroke();
                    ctx.beginPath();
                    ctx.moveTo(0, pos);
                    ctx.lineTo(width, pos);
                    ctx.stroke();
                }
            }
            
            // Draw info overlay
            ctx.fillStyle = 'rgba(0, 0, 0, 0.85)';
            ctx.fillRect(10, 10, 220, 120);
            ctx.fillStyle = 'white';
            ctx.font = '12px monospace';
            ctx.fillText(`t = ${time.toFixed(2)} / 48`, 20, 30);
            if (isSlice) {
                ctx.fillText(`${viewAxis.toUpperCase()} = ${slicePos.toFixed(1)}`, 20, 50);
            } else {
                ctx.fillText(`${projectionType.charAt(0).toUpperCase() + projectionType.slice(1)} along ${viewAxis.toUpperCase()}`, 20, 50);
            }
            ctx.fillText(`min = ${minVal.toFixed(1)}`, 20, 70);
            ctx.fillText(`max = ${maxVal.toFixed(1)}`, 20, 90);
            ctx.fillText(`Res = ${samplingRes}x${samplingRes}`, 20, 110);
            
            // Draw selected point info
            if (selectedPoint && isSlice) {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.85)';
                ctx.fillRect(10, height - 100, 220, 90);
                ctx.fillStyle = '#4ECDC4';
                ctx.font = '11px monospace';
                ctx.fillText(`X: ${selectedPoint.x.toFixed(1)} Y: ${selectedPoint.y.toFixed(1)} Z: ${selectedPoint.z.toFixed(1)}`, 20, height - 80);
                ctx.fillText(`φ = ${selectedPoint.val.toFixed(2)}`, 20, height - 50);
                ctx.fillText(`Root = ${selectedPoint.root}`, 20, height - 20);
            }
            
            // Draw colorbar
            if (vizMode !== 'digital') {
                const barWidth = 20;
                const barHeight = height - 150;
                const barX = width - 35;
                const barY = 70;
                
                for (let i = 0; i < barHeight; i++) {
                    const norm = 1 - i / barHeight;
                    const val = minVal + (maxVal - minVal) * norm;
                    ctx.fillStyle = valueToColor(val, minVal, maxVal);
                    ctx.fillRect(barX, barY + i, barWidth, 1);
                }
                
                ctx.strokeStyle = 'white';
                ctx.lineWidth = 1;
                ctx.strokeRect(barX, barY, barWidth, barHeight);
                
                ctx.fillStyle = 'white';
                ctx.font = '10px monospace';
                ctx.textAlign = 'left';
                ctx.fillText(maxVal.toFixed(0), barX - 45, barY + 10);
                ctx.fillText(minVal.toFixed(0), barX - 45, barY + barHeight - 5);
                if (vizMode === 'fourier') {
                    ctx.fillText('Log Mag', barX - 45, barY - 5);
                }
            } else {
                // Digital root legend
                const legendX = width - 110;
                const legendY = 70;
                ctx.fillStyle = 'rgba(0, 0, 0, 0.85)';
                ctx.fillRect(legendX - 5, legendY - 15, 105, 180);
                ctx.fillStyle = 'white';
                ctx.font = '11px monospace';
                ctx.fillText('Digital Roots:', legendX, legendY);
                for (let i = 1; i <= 9; i++) {
                    ctx.fillStyle = digitalRootColor(i);
                    ctx.fillRect(legendX, legendY + i * 16, 15, 12);
                    ctx.fillStyle = 'white';
                    ctx.fillText(i.toString(), legendX + 25, legendY + i * 16 + 10);
                }
            }
        }

        function startAnimation() {
            if (animationInterval) clearInterval(animationInterval);
            animationInterval = setInterval(() => {
                time = (time + 0.1) % 48;
                document.getElementById('timeSlider').value = time;
                document.getElementById('timeLabel').textContent = `Time: ${time.toFixed(2)} / 48`;
                draw();
            }, 50);
        }

        function stopAnimation() {
            if (animationInterval) clearInterval(animationInterval);
        }

        function reset() {
            time = 0;
            isPlaying = false;
            selectedPoint = null;
            document.getElementById('playPauseBtn').textContent = 'Play';
            document.getElementById('timeSlider').value = 0;
            document.getElementById('timeLabel').textContent = `Time: 0.00 / 48`;
            stopAnimation();
            draw();
        }

        function handleCanvasClick(e) {
            if (projectionType !== 'slice') {
                selectedPoint = null;
                return;
            }
            const rect = canvas.getBoundingClientRect();
            const normX = (e.clientX - rect.left) / rect.width;
            const normY = (e.clientY - rect.top) / rect.height;
            const spatialX = normX * 360;
            const spatialY = normY * 360;
            
            let px, py, pz;
            if (viewAxis === 'z') {
                px = spatialX; py = spatialY; pz = slicePos;
            } else if (viewAxis === 'y') {
                px = spatialX; pz = spatialY; py = slicePos;
            } else {
                py = spatialX; pz = spatialY; px = slicePos;
            }
            
            const val = phi(px, py, pz, time);
            const root = digitalRoot(val);
            selectedPoint = { x: px, y: py, z: pz, val, root };
            draw();
        }

        // Event listeners
        document.getElementById('playPauseBtn').addEventListener('click', () => {
            isPlaying = !isPlaying;
            document.getElementById('playPauseBtn').textContent = isPlaying ? 'Pause' : 'Play';
            if (isPlaying) startAnimation();
            else stopAnimation();
        });

        document.getElementById('resetBtn').addEventListener('click', reset);

        document.getElementById('latticeSizeSelect').addEventListener('change', (e) => {
            latticeSize = parseInt(e.target.value);
            draw();
        });

        document.getElementById('projectionTypeSelect').addEventListener('change', (e) => {
            projectionType = e.target.value;
            const sliceDiv = document.getElementById('slicePositionDiv');
            sliceDiv.style.display = projectionType === 'slice' ? 'flex' : 'none';
            selectedPoint = null;
            draw();
        });

        const vizBtns = document.getElementsByClassName('viz-btn');
        for (let btn of vizBtns) {
            btn.addEventListener('click', (e) => {
                vizMode = e.target.id.replace('Btn', '');
                for (let b of vizBtns) {
                    b.style.backgroundColor = '#374151';
                    b.style.color = '#d1d5db';
                }
                e.target.style.backgroundColor = '#7c3aed';
                e.target.style.color = 'white';
                draw();
            });
        }

        document.getElementById('showGridCheckbox').addEventListener('change', (e) => {
            showGrid = e.target.checked;
            draw();
        });

        const axisBtns = document.getElementsByClassName('axis-btn');
        for (let btn of axisBtns) {
            btn.addEventListener('click', (e) => {
                viewAxis = e.target.id === 'xyPlaneBtn' ? 'z' : e.target.id === 'xzPlaneBtn' ? 'y' : 'x';
                for (let b of axisBtns) {
                    b.style.backgroundColor = '#374151';
                    b.style.color = '#d1d5db';
                }
                e.target.style.backgroundColor = '#0d9488';
                e.target.style.color = 'white';
                document.getElementById('sliceLabel').textContent = `Slice Position (${viewAxis.toUpperCase()}): ${slicePos.toFixed(1)}`;
                draw();
            });
        }

        document.getElementById('sliceSlider').addEventListener('input', (e) => {
            slicePos = parseFloat(e.target.value);
            document.getElementById('sliceLabel').textContent = `Slice Position (${viewAxis.toUpperCase()}): ${slicePos.toFixed(1)}`;
            draw();
        });

        document.getElementById('timeSlider').addEventListener('input', (e) => {
            time = parseFloat(e.target.value);
            document.getElementById('timeLabel').textContent = `Time: ${time.toFixed(2)} / 48`;
            draw();
        });

        canvas.addEventListener('click', handleCanvasClick);

        // Initial draw
        draw();
    </script>
</body>
</html>
