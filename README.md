<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>APK Injector · Security Testing Tool</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js">
    </script>
    <script src="https://unpkg.com/html5-qrcode@2.3.8/html5-qrcode.min.js">
    </script>
    <style>
        /* ─── RESET & BASE ─── */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            background: #0a0e17;
            color: #e0e6f0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px 16px 40px;
        }

        /* ─── HEADER ─── */
        .header {
            text-align: center;
            margin-bottom: 28px;
            width: 100%;
            max-width: 560px;
        }
        .header h1 {
            font-size: 1.65rem;
            font-weight: 700;
            background: linear-gradient(135deg, #00f0ff, #7b2ff7);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: -0.5px;
        }
        .header .badge {
            display: inline-block;
            margin-top: 6px;
            background: rgba(123, 47, 247, 0.18);
            border: 1px solid rgba(123, 47, 247, 0.35);
            padding: 4px 16px;
            border-radius: 20px;
            font-size: 0.7rem;
            color: #a78bfa;
            letter-spacing: 0.5px;
            text-transform: uppercase;
        }
        .header .sub {
            font-size: 0.85rem;
            color: #7f8a9e;
            margin-top: 8px;
        }

        /* ─── CARD ─── */
        .card {
            background: #131b2e;
            border-radius: 18px;
            border: 1px solid #1f2a44;
            padding: 24px 20px;
            width: 100%;
            max-width: 560px;
            margin-bottom: 18px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
            transition: border-color 0.25s;
        }
        .card.accent {
            border-color: #2a3a5e;
        }
        .card-title {
            font-size: 1.05rem;
            font-weight: 600;
            margin-bottom: 14px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .card-title .ico {
            width: 28px;
            height: 28px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            background: #1f2a44;
            border-radius: 8px;
            font-size: 1rem;
        }

        /* ─── DROP ZONE ─── */
        .drop-zone {
            border: 2px dashed #2a3a5e;
            border-radius: 14px;
            padding: 36px 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
            background: rgba(255, 255, 255, 0.015);
        }
        .drop-zone:hover,
        .drop-zone.dragover {
            border-color: #7b2ff7;
            background: rgba(123, 47, 247, 0.06);
        }
        .drop-zone .icon {
            font-size: 2.4rem;
            margin-bottom: 10px;
            display: block;
        }
        .drop-zone p {
            color: #8e9bb3;
            font-size: 0.9rem;
        }
        .drop-zone p strong {
            color: #c8d0e0;
        }
        .drop-zone input[type="file"] {
            display: none;
        }

        /* ─── FILE INFO ─── */
        .file-info {
            display: none;
            align-items: center;
            gap: 12px;
            background: #0f1729;
            border-radius: 12px;
            padding: 12px 16px;
            margin-top: 14px;
            border: 1px solid #1f2a44;
        }
        .file-info .fi-icon {
            font-size: 1.6rem;
            flex-shrink: 0;
        }
        .file-info .fi-details {
            flex: 1;
            min-width: 0;
        }
        .file-info .fi-name {
            font-weight: 600;
            font-size: 0.9rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .file-info .fi-size {
            font-size: 0.75rem;
            color: #8e9bb3;
        }
        .file-info .fi-remove {
            background: none;
            border: none;
            color: #f87171;
            font-size: 1.3rem;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 8px;
        }
        .file-info .fi-remove:hover {
            background: rgba(248, 113, 113, 0.12);
        }

        /* ─── QR CODE DISPLAY ─── */
        .qr-section {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 14px;
        }
        .qr-section #qrcode {
            background: #fff;
            padding: 16px;
            border-radius: 14px;
            display: inline-block;
            line-height: 0;
        }
        .qr-section #qrcode img {
            border-radius: 4px;
        }
        .qr-section .qr-url {
            font-size: 0.72rem;
            color: #7f8a9e;
            word-break: break-all;
            text-align: center;
            max-width: 100%;
            background: #0f1729;
            padding: 8px 14px;
            border-radius: 8px;
            border: 1px solid #1f2a44;
        }

        /* ─── BUTTONS ─── */
        .btn-group {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 14px;
        }
        .btn {
            flex: 1;
            min-width: 120px;
            padding: 12px 20px;
            border: none;
            border-radius: 12px;
            font-weight: 600;
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            color: #fff;
        }
        .btn:active {
            transform: scale(0.97);
        }
        .btn:disabled {
            opacity: 0.4;
            cursor: not-allowed;
            transform: none;
        }
        .btn-primary {
            background: linear-gradient(135deg, #7b2ff7, #5b1fcf);
        }
        .btn-primary:hover:not(:disabled) {
            box-shadow: 0 4px 20px rgba(123, 47, 247, 0.35);
        }
        .btn-success {
            background: linear-gradient(135deg, #10b981, #059669);
        }
        .btn-success:hover:not(:disabled) {
            box-shadow: 0 4px 20px rgba(16, 185, 129, 0.35);
        }
        .btn-danger {
            background: linear-gradient(135deg, #ef4444, #dc2626);
        }
        .btn-danger:hover:not(:disabled) {
            box-shadow: 0 4px 20px rgba(239, 68, 68, 0.3);
        }
        .btn-outline {
            background: transparent;
            border: 1px solid #2a3a5e;
            color: #c8d0e0;
        }
        .btn-outline:hover:not(:disabled) {
            border-color: #7b2ff7;
            color: #a78bfa;
        }
        .btn-sm {
            flex: 0 0 auto;
            padding: 8px 16px;
            font-size: 0.78rem;
            min-width: unset;
        }

        /* ─── SCANNER ─── */
        .scanner-wrap {
            display: none;
            margin-top: 10px;
        }
        .scanner-wrap.active {
            display: block;
        }
        .scanner-wrap #reader {
            border-radius: 12px;
            overflow: hidden;
            background: #000;
            min-height: 200px;
        }
        .scanner-wrap #reader video {
            border-radius: 12px;
        }
        .scanner-controls {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        /* ─── TOAST / NOTIFICATION ─── */
        .toast {
            position: fixed;
            top: 24px;
            left: 50%;
            transform: translateX(-50%) translateY(-80px);
            background: #1f2a44;
            border: 1px solid #2a3a5e;
            padding: 14px 24px;
            border-radius: 14px;
            font-size: 0.85rem;
            box-shadow: 0 8px 40px rgba(0, 0, 0, 0.6);
            z-index: 999;
            transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
            max-width: 90vw;
            text-align: center;
            pointer-events: none;
        }
        .toast.show {
            transform: translateX(-50%) translateY(0);
        }
        .toast.success {
            border-color: #10b981;
        }
        .toast.error {
            border-color: #ef4444;
        }

        /* ─── TAB SYSTEM ─── */
        .tabs {
            display: flex;
            gap: 4px;
            background: #0f1729;
            border-radius: 12px;
            padding: 4px;
            margin-bottom: 20px;
        }
        .tab {
            flex: 1;
            text-align: center;
            padding: 10px 12px;
            border-radius: 10px;
            font-size: 0.8rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
            color: #7f8a9e;
            border: none;
            background: none;
        }
        .tab.active {
            background: #1f2a44;
            color: #e0e6f0;
        }
        .tab:hover:not(.active) {
            color: #a78bfa;
        }

        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }

        /* ─── RESPONSIVE ─── */
        @media (max-width: 480px) {
            body {
                padding: 14px 12px 30px;
            }
            .card {
                padding: 18px 14px;
            }
            .header h1 {
                font-size: 1.35rem;
            }
            .btn {
                font-size: 0.78rem;
                padding: 10px 14px;
                min-width: 90px;
            }
        }
    </style>
</head>
<body>

    <!-- ═══ TOAST ═══ -->
    <div id="toast" class="toast"></div>

    <!-- ═══ HEADER ═══ -->
    <div class="header">
        <h1>⚡ APK Injector</h1>
        <span class="badge">🔐 Authorized Pentest Tool</span>
        <p class="sub">Upload · QR · Scan · Install</p>
    </div>

    <!-- ═══ TABS ═══ -->
    <div class="card" style="padding-bottom: 6px;">
        <div class="tabs">
            <button class="tab active" data-tab="upload">📤 Upload</button>
            <button class="tab" data-tab="scan">📷 Scan</button>
            <button class="tab" data-tab="history">📋 History</button>
        </div>
    </div>

    <!-- ═══ TAB 1 — UPLOAD ═══ -->
    <div id="tab-upload" class="tab-content active" style="width:100%;max-width:560px;">
        <div class="card accent">
            <div class="card-title">
                <span class="ico">📦</span> Drop your APK
            </div>

            <!-- Drop Zone -->
            <div class="drop-zone" id="dropZone">
                <span class="icon">📱</span>
                <p><strong>Tap to browse</strong> or drag &amp; drop an APK</p>
                <p style="font-size:0.72rem;color:#6a7a92;margin-top:6px;">Android Package (.apk)</p>
                <input type="file" id="fileInput" accept=".apk,application/vnd.android.package-archive">
            </div>

            <!-- File Info -->
            <div class="file-info" id="fileInfo">
                <span class="fi-icon">📱</span>
                <div class="fi-details">
                    <div class="fi-name" id="fileName">app.apk</div>
                    <div class="fi-size" id="fileSize">0 MB</div>
                </div>
                <button class="fi-remove" id="removeFile" title="Remove">✕</button>
            </div>

            <!-- Actions -->
            <div class="btn-group" style="margin-top:16px;">
                <button class="btn btn-primary" id="btnGenerateQR" disabled>
                    🔲 Generate QR
                </button>
                <button class="btn btn-success" id="btnServeAndInstall" disabled>
                    📲 Serve &amp; Install
                </button>
            </div>
        </div>

        <!-- QR Display Card -->
        <div class="card" id="qrCard" style="display:none;">
            <div class="card-title">
                <span class="ico">🔲</span> QR Code
            </div>
            <div class="qr-section">
                <div id="qrcode"></div>
                <div class="qr-url" id="qrUrl">—</div>
                <div class="btn-group">
                    <button class="btn btn-success btn-sm" id="btnInstallViaQR">
                        📲 Install from QR
                    </button>
                    <button class="btn btn-outline btn-sm" id="btnCopyURL">
                        📋 Copy URL
                    </button>
                    <button class="btn btn-danger btn-sm" id="btnStopServer">
                        ⏹ Stop Server
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- ═══ TAB 2 — SCAN ═══ -->
    <div id="tab-scan" class="tab-content" style="width:100%;max-width:560px;">
        <div class="card accent">
            <div class="card-title">
                <span class="ico">📷</span> QR Scanner
            </div>
            <p style="font-size:0.8rem;color:#8e9bb3;margin-bottom:12px;">
                Point your camera at a QR code containing an APK download URL.
            </p>

            <div class="scanner-wrap" id="scannerWrap">
                <div id="reader"></div>
                <div class="scanner-controls">
                    <button class="btn btn-danger btn-sm" id="btnStopScanner">⏹ Stop</button>
                </div>
            </div>

            <div class="btn-group">
                <button class="btn btn-primary" id="btnStartScanner">
                    📷 Start Camera
                </button>
            </div>

            <div id="scanResult" style="display:none;margin-top:14px;padding:14px;background:#0f1729;border-radius:12px;border:1px solid #1f2a44;">
                <div style="font-size:0.8rem;color:#8e9bb3;">Scanned URL:</div>
                <div id="scanUrlValue" style="word-break:break-all;font-size:0.85rem;margin:6px 0 10px;color:#a78bfa;"></div>
                <a id="scanInstallLink" class="btn btn-success btn-sm" style="text-decoration:none;display:inline-flex;" target="_blank">
                    📲 Open / Install
                </a>
                <button class="btn btn-outline btn-sm" id="btnCopyScanUrl" style="margin-left:6px;">
                    📋 Copy
                </button>
            </div>
        </div>
    </div>

    <!-- ═══ TAB 3 — HISTORY ═══ -->
    <div id="tab-history" class="tab-content" style="width:100%;max-width:560px;">
        <div class="card accent">
            <div class="card-title">
                <span class="ico">📋</span> Session History
            </div>
            <div id="historyList" style="font-size:0.82rem;color:#8e9bb3;">
                <p>No APKs processed yet.</p>
            </div>
            <button class="btn btn-outline btn-sm" id="btnClearHistory" style="margin-top:12px;width:100%;">
                🗑 Clear History
            </button>
        </div>
    </div>

    <!-- ═══ FOOTER ═══ -->
    <div style="margin-top:12px;font-size:0.7rem;color:#3b4a62;text-align:center;max-width:560px;">
        Authorized security assessment tool. APK data stays in-browser (Blob URL). No files are uploaded to any external server.
    </div>

    <!-- ════════════════════════════════════════════════════════════ -->
    <!-- ═══  JAVASCRIPT  ═══ -->
    <!-- ════════════════════════════════════════════════════════════ -->
    <script>
        (function() {
            'use strict';

            // ─── DOM refs ───
            const $ = id => document.getElementById(id);
            const dropZone = $('dropZone');
            const fileInput = $('fileInput');
            const fileInfo = $('fileInfo');
            const fileName = $('fileName');
            const fileSize = $('fileSize');
            const removeFileBtn = $('removeFile');
            const btnGenerateQR = $('btnGenerateQR');
            const btnServeInstall = $('btnServeAndInstall');
            const qrCard = $('qrCard');
            const qrContainer = $('qrcode');
            const qrUrl = $('qrUrl');
            const btnInstallViaQR = $('btnInstallViaQR');
            const btnCopyURL = $('btnCopyURL');
            const btnStopServer = $('btnStopServer');
            const toast = $('toast');

            // Scanner
            const scannerWrap = $('scannerWrap');
            const readerDiv = $('reader');
            const btnStartScanner = $('btnStartScanner');
            const btnStopScanner = $('btnStopScanner');
            const scanResult = $('scanResult');
            const scanUrlValue = $('scanUrlValue');
            const scanInstallLink = $('scanInstallLink');
            const btnCopyScanUrl = $('btnCopyScanUrl');

            // History
            const historyList = $('historyList');
            const btnClearHistory = $('btnClearHistory');

            // Tabs
            const tabs = document.querySelectorAll('.tab');
            const tabContents = {
                upload: $('tab-upload'),
                scan: $('tab-scan'),
                history: $('tab-history')
            };

            // ─── STATE ───
            let currentFile = null; // File object
            let qrCodeInstance = null;
            let html5Scanner = null;
            let isScannerRunning = false;
            let serverBlobUrl = null; // blob: URL for serving the APK
            let historyEntries = [];

            // Load history from sessionStorage
            try {
                const saved = sessionStorage.getItem('apk_injector_history');
                if (saved) {
                    historyEntries = JSON.parse(saved);
                    renderHistory();
                }
            } catch (_) {}

            // ─── TOAST ───
            let toastTimeout = null;

            function showToast(msg, type) {
                toast.textContent = msg;
                toast.className = 'toast' + (type ? ' ' + type : '');
                clearTimeout(toastTimeout);
                // force reflow
                void toast.offsetWidth;
                toast.classList.add('show');
                toastTimeout = setTimeout(() => toast.classList.remove('show'), 3200);
            }

            // ─── FILE HANDLING ───
            function handleFile(file) {
                if (!file) return;
                // Check extension
                const ext = file.name.toLowerCase().slice(-4);
                if (ext !== '.apk' && file.type !== 'application/vnd.android.package-archive') {
                    showToast('Please select a valid .apk file', 'error');
                    return;
                }
                if (file.size === 0) {
                    showToast('File is empty', 'error');
                    return;
                }

                currentFile = file;
                fileName.textContent = file.name;
                const sizeMB = (file.size / (1024 * 1024)).toFixed(2);
                fileSize.textContent = sizeMB + ' MB';
                fileInfo.style.display = 'flex';
                dropZone.style.display = 'none';

                btnGenerateQR.disabled = false;
                btnServeInstall.disabled = false;

                // Revoke old blob URL
                if (serverBlobUrl) {
                    URL.revokeObjectURL(serverBlobUrl);
                    serverBlobUrl = null;
                }

                // Hide previous QR
                qrCard.style.display = 'none';
                if (qrCodeInstance) {
                    qrCodeInstance.clear();
                    qrCodeInstance = null;
                }

                showToast('APK loaded: ' + file.name, 'success');

                // Add to history
                addHistory('upload', file.name, sizeMB + ' MB');
            }

            // Drop zone events
            dropZone.addEventListener('click', () => fileInput.click());

            dropZone.addEventListener('dragover', (e) => {
                e.preventDefault();
                dropZone.classList.add('dragover');
            });
            dropZone.addEventListener('dragleave', () => {
                dropZone.classList.remove('dragover');
            });
            dropZone.addEventListener('drop', (e) => {
                e.preventDefault();
                dropZone.classList.remove('dragover');
                const files = e.dataTransfer.files;
                if (files.length > 0) handleFile(files[0]);
            });

            fileInput.addEventListener('change', () => {
                if (fileInput.files.length > 0) {
                    handleFile(fileInput.files[0]);
                }
            });

            // Remove file
            removeFileBtn.addEventListener('click', () => {
                resetFileState();
                if (serverBlobUrl) {
                    URL.revokeObjectURL(serverBlobUrl);
                    serverBlobUrl = null;
                }
                qrCard.style.display = 'none';
                if (qrCodeInstance) {
                    qrCodeInstance.clear();
                    qrCodeInstance = null;
                }
                showToast('File removed', null);
            });

            function resetFileState() {
                currentFile = null;
                fileInfo.style.display = 'none';
                dropZone.style.display = 'block';
                fileInput.value = '';
                btnGenerateQR.disabled = true;
                btnServeInstall.disabled = true;
            }

            // ─── GENERATE QR ───
            btnGenerateQR.addEventListener('click', () => {
                if (!currentFile) {
                    showToast('No APK loaded', 'error');
                    return;
                }

                // Create blob URL if not already
                if (!serverBlobUrl) {
                    serverBlobUrl = URL.createObjectURL(currentFile);
                }

                const downloadUrl = serverBlobUrl;
                qrUrl.textContent = downloadUrl;

                // Clear old QR
                if (qrCodeInstance) {
                    qrCodeInstance.clear();
                }
                qrContainer.innerHTML = '';

                // Generate QR
                qrCodeInstance = new QRCode(qrContainer, {
                    text: downloadUrl,
                    width: 220,
                    height: 220,
                    colorDark: '#0a0e17',
                    colorLight: '#ffffff',
                    correctLevel: QRCode.CorrectLevel.H
                });

                qrCard.style.display = 'block';
                showToast('QR code generated', 'success');

                addHistory('qr', currentFile.name, 'QR generated');
            });

            // ─── SERVE & INSTALL ───
            btnServeInstall.addEventListener('click', () => {
                if (!currentFile) {
                    showToast('No APK loaded', 'error');
                    return;
                }

                if (!serverBlobUrl) {
                    serverBlobUrl = URL.createObjectURL(currentFile);
                }

                // For Android, we use an intent:// scheme that tells the browser
                // to download and open the APK. The blob URL works on Chrome Android
                // when opened directly in a new tab — it triggers the download.
                // We also generate a QR for the same URL.
                const url = serverBlobUrl;

                // Generate QR
                if (qrCodeInstance) {
                    qrCodeInstance.clear();
                }
                qrContainer.innerHTML = '';
                qrCodeInstance = new QRCode(qrContainer, {
                    text: url,
                    width: 220,
                    height: 220,
                    colorDark: '#0a0e17',
                    colorLight: '#ffffff',
                    correctLevel: QRCode.CorrectLevel.H
                });
                qrUrl.textContent = url;
                qrCard.style.display = 'block';

                // Try to trigger download / install via opening in new tab
                // Chrome Android will download the blob and show a notification
                window.open(url, '_blank');

                showToast('APK served — download should begin', 'success');
                addHistory('install', currentFile.name, 'Served via blob URL');
            });

            // ─── INSTALL VIA QR (opens the URL) ───
            btnInstallViaQR.addEventListener('click', () => {
                const url = qrUrl.textContent;
                if (url && url !== '—') {
                    window.open(url, '_blank');
                    showToast('Opening APK URL for install', 'success');
                } else {
                    showToast('No QR URL available', 'error');
                }
            });

            // ─── COPY URL ───
            btnCopyURL.addEventListener('click', () => {
                const url = qrUrl.textContent;
                if (url && url !== '—') {
                    if (navigator.clipboard && navigator.clipboard.writeText) {
                        navigator.clipboard.writeText(url).then(() => {
                            showToast('URL copied to clipboard', 'success');
                        }).catch(() => {
                            fallbackCopy(url);
                        });
                    } else {
                        fallbackCopy(url);
                    }
                } else {
                    showToast('No URL to copy', 'error');
                }
            });

            function fallbackCopy(text) {
                const ta = document.createElement('textarea');
                ta.value = text;
                ta.style.position = 'fixed';
                ta.style.opacity = '0';
                document.body.appendChild(ta);
                ta.select();
                try {
                    document.execCommand('copy');
                    showToast('URL copied to clipboard', 'success');
                } catch (_) {
                    showToast('Failed to copy', 'error');
                }
                document.body.removeChild(ta);
            }

            // ─── STOP SERVER (revoke blob URL) ───
            btnStopServer.addEventListener('click', () => {
                if (serverBlobUrl) {
                    URL.revokeObjectURL(serverBlobUrl);
                    serverBlobUrl = null;
                }
                if (qrCodeInstance) {
                    qrCodeInstance.clear();
                }
                qrCard.style.display = 'none';
                showToast('Server stopped, blob URL revoked', null);
                addHistory('stop', '—', 'Server stopped');
            });

            // ─── SCANNER ───
            btnStartScanner.addEventListener('click', () => {
                if (isScannerRunning) return;

                scannerWrap.classList.add('active');
                btnStartScanner.disabled = true;
                btnStartScanner.textContent = '⏳ Starting...';

                // Clean previous instance
                if (html5Scanner) {
                    try { html5Scanner.stop(); } catch (_) {}
                    html5Scanner = null;
                }

                html5Scanner = new Html5Qrcode("reader");

                html5Scanner.start({ facingMode: "environment" }, {
                    fps: 15,
                    qrbox: { width: 250, height: 250 }
                }, (decodedText) => {
                    // Success
                    showToast('QR scanned!', 'success');
                    scanUrlValue.textContent = decodedText;
                    scanInstallLink.href = decodedText;
                    scanResult.style.display = 'block';

                    // Stop scanner after successful scan
                    stopScanner();

                    addHistory('scan', 'QR scanned', decodedText.substring(0, 60));
                }, () => {
                    // Failure — keep trying, no action needed
                }).then(() => {
                    isScannerRunning = true;
                    btnStartScanner.textContent = '📷 Running...';
                    btnStartScanner.disabled = true;
                    btnStopScanner.style.display = 'inline-flex';
                }).catch((err) => {
                    showToast('Camera error: ' + err, 'error');
                    scannerWrap.classList.remove('active');
                    btnStartScanner.disabled = false;
                    btnStartScanner.textContent = '📷 Start Camera';
                    isScannerRunning = false;
                });
            });

            function stopScanner() {
                if (html5Scanner && isScannerRunning) {
                    html5Scanner.stop().then(() => {
                        isScannerRunning = false;
                        btnStartScanner.disabled = false;
                        btnStartScanner.textContent = '📷 Start Camera';
                        btnStopScanner.style.display = 'none';
                    }).catch(() => {
                        isScannerRunning = false;
                        btnStartScanner.disabled = false;
                        btnStartScanner.textContent = '📷 Start Camera';
                        btnStopScanner.style.display = 'none';
                    });
                } else {
                    isScannerRunning = false;
                    btnStartScanner.disabled = false;
                    btnStartScanner.textContent = '📷 Start Camera';
                    btnStopScanner.style.display = 'none';
                }
                scannerWrap.classList.remove('active');
            }

            btnStopScanner.addEventListener('click', stopScanner);

            // Copy scanned URL
            btnCopyScanUrl.addEventListener('click', () => {
                const url = scanUrlValue.textContent;
                if (url) {
                    if (navigator.clipboard && navigator.clipboard.writeText) {
                        navigator.clipboard.writeText(url).then(() => {
                            showToast('URL copied', 'success');
                        }).catch(() => fallbackCopy(url));
                    } else {
                        fallbackCopy(url);
                    }
                }
            });

            // ─── TABS ───
            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    tabs.forEach(t => t.classList.remove('active'));
                    tab.classList.add('active');

                    const target = tab.dataset.tab;
                    Object.keys(tabContents).forEach(key => {
                        tabContents[key].classList.toggle('active', key === target);
                    });

                    // If switching to scan, stop camera if running outside tab
                    if (target !== 'scan' && isScannerRunning) {
                        stopScanner();
                    }
                });
            });

            // ─── HISTORY ───
            function addHistory(type, label, detail) {
                const entry = {
                    type: type,
                    label: label,
                    detail: detail,
                    time: new Date().toLocaleTimeString()
                };
                historyEntries.unshift(entry);
                if (historyEntries.length > 50) historyEntries.pop();
                renderHistory();
                try {
                    sessionStorage.setItem('apk_injector_history', JSON.stringify(historyEntries));
                } catch (_) {}
            }

            function renderHistory() {
                if (historyEntries.length === 0) {
                    historyList.innerHTML = '<p>No APKs processed yet.</p>';
                    return;
                }
                let html = '';
                const icons = {
                    upload: '📤',
                    qr: '🔲',
                    install: '📲',
                    scan: '📷',
                    stop: '⏹'
                };
                historyEntries.forEach(e => {
                    const ico = icons[e.type] || '📄';
                    html += `<div style="display:flex;gap:10px;padding:8px 0;border-bottom:1px solid #1f2a44;align-items:center;">
                        <span style="font-size:1.1rem;">${ico}</span>
                        <div style="flex:1;min-width:0;">
                            <div style="font-weight:500;font-size:0.82rem;color:#c8d0e0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">${e.label}</div>
                            <div style="font-size:0.7rem;color:#6a7a92;">${e.detail} · ${e.time}</div>
                        </div>
                    </div>`;
                });
                historyList.innerHTML = html;
            }

            btnClearHistory.addEventListener('click', () => {
                historyEntries = [];
                renderHistory();
                try {
                    sessionStorage.removeItem('apk_injector_history');
                } catch (_) {}
                showToast('History cleared', null);
            });

            // ─── ANDROID INTENT INSTALL (for "Install from QR" button enhancement) ───
            // On Android Chrome, we can use intent: scheme for direct install.
            // This is used by the scanInstallLink and btnInstallViaQR.
            // Additionally, we provide a helper to build an intent:// URL.
            function buildAndroidIntentURL(apkUrl) {
                // intent://<url>#Intent;action=android.intent.action.VIEW;scheme=https;package=com.android.packageinstaller;end
                // However, the most reliable approach across Android 5-17 is:
                // 1. Direct https link — Chrome downloads APK, user taps notification to install
                // 2. intent:// scheme with fallback
                // We'll just return the raw URL since Chrome handles APK downloads natively.
                return apkUrl;
            }

            // ─── KEYBOARD ───
            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape') {
                    if (isScannerRunning) stopScanner();
                }
            });

            // ─── INIT TOAST HIDDEN ───
            toast.classList.remove('show');

            console.log('⚡ APK Injector ready');
            console.log('🔐 Authorized pentest tool — all data stays local');

        })();
    </script>
</body>
</html>
