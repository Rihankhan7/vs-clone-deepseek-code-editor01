# vs-clone-deepseek-code-editor01
MADE A VS CODE EDITOR WHICH USE DEAP SEEK AS CODE EDITOR MAKING PROGRESS ON IT 15/5/2026

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Studio Pro - DeepSeek AI</title>
    <style>
        :root {
            --bg: #1e1e1e;
            --bg2: #252526;
            --bg3: #2d2d2d;
            --border: #3c3c3c;
            --text: #d4d4d4;
            --accent: #007acc;
            --gradient: linear-gradient(135deg, #667eea, #764ba2);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background: var(--bg);
            color: var(--text);
            height: 100vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        /* Top Bar */
        .topbar {
            height: 42px;
            background: #323233;
            display: flex;
            align-items: center;
            padding: 0 12px;
            gap: 8px;
            border-bottom: 1px solid #000;
            flex-shrink: 0;
        }
        .topbar .logo {
            font-weight: 600;
            color: #fff;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .topbar button {
            padding: 6px 14px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
            color: white;
            white-space: nowrap;
            transition: all 0.2s;
        }
        .topbar button:hover { opacity: 0.85; transform: translateY(-1px); }
        .btn-primary { background: var(--accent); }
        .btn-secondary { background: #3c3c3c; }
        .btn-success { background: #388a34; }
        .btn-warning { background: #e67e22; }
        .btn-danger { background: #c72e2e; }
        .api-status {
            margin-left: auto;
            font-size: 11px;
            display: flex;
            align-items: center;
            gap: 6px;
            padding: 4px 10px;
            border-radius: 12px;
            background: #2d2d2d;
        }
        .api-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            display: inline-block;
        }
        .api-dot.online { background: #4caf50; box-shadow: 0 0 6px #4caf50; }
        .api-dot.offline { background: #f44336; }

        /* Main Layout */
        .main-layout {
            display: flex;
            flex: 1;
            overflow: hidden;
        }

        /* Sidebar */
        .sidebar {
            width: 48px;
            background: #333;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-top: 8px;
            border-right: 1px solid #000;
            gap: 4px;
        }
        .sidebar button {
            width: 40px;
            height: 40px;
            background: none;
            border: none;
            color: #858585;
            cursor: pointer;
            font-size: 18px;
            border-radius: 6px;
            transition: all 0.2s;
        }
        .sidebar button:hover { background: #444; color: #fff; }
        .sidebar button.active { color: #fff; }

        /* Editor Section */
        .editor-section {
            flex: 1;
            display: flex;
            flex-direction: column;
            min-width: 0;
        }

        .tabs-bar {
            height: 34px;
            background: var(--bg3);
            display: flex;
            border-bottom: 1px solid #000;
            overflow-x: auto;
        }
        .tab {
            padding: 0 16px;
            display: flex;
            align-items: center;
            font-size: 12px;
            cursor: pointer;
            border-right: 1px solid #1e1e1e;
            color: #999;
            gap: 8px;
            white-space: nowrap;
        }
        .tab.active { background: var(--bg); color: #fff; border-top: 2px solid var(--accent); }
        .tab .close-tab { font-size: 14px; padding: 2px 5px; border-radius: 3px; }
        .tab .close-tab:hover { background: rgba(255,255,255,0.2); }

        .editor-wrap {
            flex: 1;
            position: relative;
            overflow: hidden;
        }
        .line-nums {
            position: absolute;
            left: 0;
            top: 0;
            width: 46px;
            height: 100%;
            background: var(--bg);
            color: #858585;
            padding: 12px 6px 12px 0;
            text-align: right;
            font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
            font-size: 13px;
            line-height: 1.55;
            border-right: 1px solid #333;
            overflow: hidden;
            pointer-events: none;
            z-index: 1;
        }
        #codeEditor {
            width: 100%;
            height: 100%;
            background: var(--bg);
            color: #d4d4d4;
            border: none;
            outline: none;
            resize: none;
            font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
            font-size: 13px;
            line-height: 1.55;
            padding: 12px 12px 12px 56px;
            tab-size: 4;
            white-space: pre;
            overflow: auto;
        }

        .statusbar {
            height: 24px;
            background: var(--accent);
            color: #fff;
            display: flex;
            align-items: center;
            padding: 0 12px;
            font-size: 11px;
            justify-content: space-between;
            flex-shrink: 0;
        }

        /* AI Panel */
        .ai-panel {
            width: 420px;
            min-width: 420px;
            background: var(--bg2);
            display: flex;
            flex-direction: column;
            border-left: 1px solid #000;
        }

        .ai-header {
            background: var(--gradient);
            padding: 14px 16px;
            color: #fff;
            display: flex;
            align-items: center;
            gap: 10px;
            flex-shrink: 0;
        }
        .ai-header .model-badge {
            font-size: 10px;
            background: rgba(255,255,255,0.2);
            padding: 2px 8px;
            border-radius: 10px;
            margin-left: auto;
        }

        .ai-chat {
            flex: 1;
            padding: 12px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .msg {
            padding: 10px 14px;
            border-radius: 10px;
            font-size: 13px;
            line-height: 1.5;
            max-width: 90%;
            word-wrap: break-word;
            animation: msgIn 0.3s ease;
        }
        @keyframes msgIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .msg-user {
            background: var(--accent);
            color: #fff;
            align-self: flex-end;
            border-bottom-right-radius: 4px;
        }
        .msg-ai {
            background: var(--bg3);
            align-self: flex-start;
            border-bottom-left-radius: 4px;
        }
        .msg-ai pre {
            background: #1e1e1e;
            padding: 12px;
            border-radius: 6px;
            margin: 8px 0;
            overflow-x: auto;
            font-family: 'Cascadia Code', 'Consolas', monospace;
            font-size: 12px;
            border: 1px solid #444;
            max-height: 300px;
            overflow-y: auto;
        }
        .msg-ai code {
            font-family: 'Cascadia Code', 'Consolas', monospace;
        }
        .msg-ai .thinking {
            display: flex;
            gap: 4px;
            padding: 4px 0;
        }
        .msg-ai .thinking span {
            width: 6px;
            height: 6px;
            background: #999;
            border-radius: 50%;
            animation: think 1.4s infinite;
        }
        .msg-ai .thinking span:nth-child(2) { animation-delay: 0.2s; }
        .msg-ai .thinking span:nth-child(3) { animation-delay: 0.4s; }
        @keyframes think {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-6px); }
        }

        .code-actions {
            display: flex;
            gap: 5px;
            margin-top: 8px;
            flex-wrap: wrap;
        }
        .code-actions button {
            padding: 6px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 11px;
            color: #fff;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 4px;
        }
        .code-actions button:hover { filter: brightness(1.2); transform: translateY(-1px); }
        .ca-insert { background: #388a34; }
        .ca-replace { background: #007acc; }
        .ca-copy { background: #555; }
        .ca-newfile { background: #8e44ad; }
        .ca-explain { background: #e67e22; }
        .ca-improve { background: #2ecc71; }

        .ai-input-area {
            padding: 12px;
            border-top: 1px solid #333;
            background: var(--bg3);
            flex-shrink: 0;
        }
        .model-selector {
            display: flex;
            gap: 6px;
            margin-bottom: 8px;
        }
        .model-btn {
            padding: 4px 10px;
            border: 1px solid #555;
            border-radius: 12px;
            cursor: pointer;
            font-size: 10px;
            background: #3c3c3c;
            color: #ccc;
            transition: all 0.2s;
        }
        .model-btn:hover { border-color: #667eea; }
        .model-btn.active { background: #667eea; border-color: #667eea; color: #fff; }

        .quick-chips {
            display: flex;
            gap: 5px;
            margin-bottom: 8px;
            flex-wrap: wrap;
        }
        .chip {
            padding: 5px 11px;
            background: #3c3c3c;
            border: 1px solid #555;
            border-radius: 14px;
            cursor: pointer;
            font-size: 11px;
            white-space: nowrap;
            transition: all 0.2s;
        }
        .chip:hover { background: #4e4e4e; border-color: var(--accent); }

        .input-row {
            display: flex;
            gap: 8px;
        }
        #aiPrompt {
            flex: 1;
            padding: 10px 14px;
            background: #1e1e1e;
            border: 2px solid #555;
            border-radius: 8px;
            color: #ccc;
            font-size: 13px;
            font-family: inherit;
            resize: none;
            outline: none;
            transition: border-color 0.3s;
            min-height: 44px;
            max-height: 100px;
        }
        #aiPrompt:focus { border-color: #667eea; }
        #sendBtn {
            padding: 10px 20px;
            background: var(--gradient);
            color: #fff;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            white-space: nowrap;
            transition: all 0.2s;
        }
        #sendBtn:hover { transform: scale(1.05); box-shadow: 0 4px 15px rgba(102,126,234,0.4); }
        #sendBtn:disabled { background: #555; cursor: not-allowed; transform: none; box-shadow: none; }

        /* Settings Modal */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.6);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
        .modal {
            background: var(--bg2);
            border: 1px solid #444;
            border-radius: 12px;
            padding: 28px;
            width: 480px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.5);
        }
        .modal h3 { color: #fff; margin-bottom: 16px; font-size: 16px; }
        .modal label { display: block; color: #ccc; font-size: 12px; margin-bottom: 6px; }
        .modal input, .modal select {
            width: 100%;
            padding: 10px 14px;
            background: #1e1e1e;
            border: 1px solid #555;
            color: #ccc;
            border-radius: 6px;
            margin-bottom: 14px;
            font-size: 13px;
            outline: none;
        }
        .modal input:focus { border-color: #667eea; }
        .modal .btn-row { display: flex; gap: 8px; justify-content: flex-end; }
        .modal button {
            padding: 8px 20px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 13px;
            color: #fff;
        }
        .modal .btn-save { background: var(--accent); }
        .modal .btn-cancel { background: #555; }

        .toast-container {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 9999;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .toast {
            padding: 10px 20px;
            border-radius: 8px;
            color: #fff;
            font-size: 13px;
            animation: toastIn 0.3s;
            text-align: center;
        }
        @keyframes toastIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .toast-success { background: #388a34; }
        .toast-error { background: #c72e2e; }
        .toast-info { background: #007acc; }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #444; border-radius: 3px; }
    </style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
    <div class="logo">🤖 Code Studio Pro</div>
    <button class="btn-secondary" onclick="Studio.newFile()">📄 New</button>
    <button class="btn-secondary" onclick="Studio.openFile()">📂 Open</button>
    <button class="btn-secondary" onclick="Studio.saveFile()">💾 Save</button>
    <button class="btn-secondary" onclick="Studio.formatCode()">🔧 Format</button>
    <button class="btn-success" onclick="Studio.runCode()">▶ Run</button>
    <button class="btn-warning" onclick="Studio.togglePreview()">🌐 Preview</button>
    <div class="api-status" id="apiStatus">
        <span class="api-dot" id="apiDot"></span>
        <span id="apiLabel">Checking API...</span>
    </div>
</div>

<!-- MAIN LAYOUT -->
<div class="main-layout">
    <!-- SIDEBAR -->
    <div class="sidebar">
        <button onclick="Studio.newFile()" title="New File">📄</button>
        <button onclick="Studio.openFile()" title="Open">📂</button>
        <button onclick="Studio.saveFile()" title="Save">💾</button>
        <button onclick="Studio.formatCode()" title="Format">🔧</button>
        <button onclick="Studio.runCode()" title="Run">▶</button>
    </div>

    <!-- EDITOR -->
    <div class="editor-section">
        <div class="tabs-bar" id="tabsBar">
            <div class="tab active" data-file="main.js" onclick="Studio.switchFile('main.js')">
                <span>📄 main.js</span>
                <span class="close-tab" onclick="event.stopPropagation();Studio.closeTab('main.js')">×</span>
            </div>
        </div>
        <div class="editor-wrap">
            <div class="line-nums" id="lineNumbers">1</div>
            <textarea id="codeEditor" placeholder="// Start coding here...&#10;// Or use the AI assistant →" spellcheck="false">// 🚀 Code Studio Pro with DeepSeek AI
// Type code here or ask the AI assistant

function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log("Fibonacci(10):", fibonacci(10));</textarea>
        </div>
        <div class="statusbar">
            <span id="langLabel">JavaScript</span>
            <span id="cursorLabel">Ln 1, Col 1</span>
            <span id="encodingLabel">UTF-8</span>
        </div>
    </div>

    <!-- AI PANEL -->
    <div class="ai-panel">
        <div class="ai-header">
            <span style="font-size:22px;">🧠</span>
            <div>
                <div style="font-weight:600;">AI Code Assistant</div>
                <div style="font-size:10px;opacity:0.8;">Powered by DeepSeek</div>
            </div>
            <span class="model-badge" id="modelBadge">DeepSeek-V3</span>
        </div>

        <div class="ai-chat" id="chatBox">
            <div class="msg msg-ai">
                👋 <b>Hello! I'm an advanced AI coding assistant.</b><br><br>
                I can help you with:<br>
                • 🐍 <b>Python</b> - Functions, APIs, algorithms<br>
                • 🌐 <b>HTML/CSS</b> - Complete pages, components<br>
                • ⚡ <b>C++</b> - Algorithms, data structures<br>
                • ⚛️ <b>React</b> - Components, hooks, state<br>
                • 📜 <b>JavaScript</b> - APIs, async, ES6+<br>
                • 🐛 <b>Debugging</b> - Find and fix issues<br>
                • 📊 <b>Analysis</b> - Code review, optimization<br><br>
                <b>Try:</b> "Create a REST API in Python with Flask"<br>
                <b>Or:</b> "Build a responsive navbar with HTML/CSS"
            </div>
        </div>

        <div class="ai-input-area">
            <div class="model-selector">
                <button class="model-btn active" onclick="Studio.setModel('deepseek')">🧠 DeepSeek API</button>
                <button class="model-btn" onclick="Studio.setModel('local')">💻 Local AI</button>
            </div>
            <div class="quick-chips">
                <span class="chip" onclick="Studio.setPrompt('Create a Python REST API with Flask and SQLite')">🐍 Flask API</span>
                <span class="chip" onclick="Studio.setPrompt('Build a complete HTML dashboard with charts')">📊 Dashboard</span>
                <span class="chip" onclick="Studio.setPrompt('Write a C++ implementation of merge sort')">⚡ Merge Sort</span>
                <span class="chip" onclick="Studio.setPrompt('Create a React todo app with local storage')">⚛️ Todo App</span>
                <span class="chip" onclick="Studio.setPrompt('Review my code and suggest improvements')">🔍 Review</span>
            </div>
            <div class="input-row">
                <textarea id="aiPrompt" rows="2" placeholder="Ask me to write, explain, or improve code..."></textarea>
                <button id="sendBtn" onclick="Studio.sendPrompt()">📤<br>Send</button>
            </div>
        </div>
    </div>
</div>

<!-- SETTINGS MODAL -->
<div class="modal-overlay" id="settingsModal" style="display:none;">
    <div class="modal">
        <h3>⚙️ DeepSeek API Settings</h3>
        <label>DeepSeek API Key</label>
        <input type="password" id="apiKeyInput" placeholder="sk-xxxxxxxxxxxxxxxxxxxxxxxx">
        <label style="font-size:11px;color:#999;">
            Get your API key at <a href="https://platform.deepseek.com/api_keys" target="_blank" style="color:#667eea;">platform.deepseek.com</a>
        </label>
        <label>Model</label>
        <select id="modelSelect">
            <option value="deepseek-chat">DeepSeek-V3 (Chat)</option>
            <option value="deepseek-coder">DeepSeek-Coder (Code Specialist)</option>
        </select>
        <div class="btn-row">
            <button class="btn-cancel" onclick="Studio.closeSettings()">Cancel</button>
            <button class="btn-save" onclick="Studio.saveSettings()">Save & Connect</button>
        </div>
    </div>
</div>

<input type="file" id="fileInput" style="display:none;">

<!-- TOAST CONTAINER -->
<div class="toast-container" id="toastContainer"></div>

<script>
// ============================================
// CODE STUDIO PRO - ADVANCED AI ASSISTANT
// ============================================

const Studio = {
    // State
    currentFile: 'main.js',
    openFiles: ['main.js'],
    fileData: { 'main.js': '' },
    processing: false,
    apiKey: '',
    model: 'deepseek-chat',
    useLocalAI: false,
    
    // DOM Elements
    editor: null,
    lineNums: null,
    promptInput: null,
    sendBtn: null,
    chatBox: null,
    fileInput: null,
    
    // Initialize
    init() {
        this.editor = document.getElementById('codeEditor');
        this.lineNums = document.getElementById('lineNumbers');
        this.promptInput = document.getElementById('aiPrompt');
        this.sendBtn = document.getElementById('sendBtn');
        this.chatBox = document.getElementById('chatBox');
        this.fileInput = document.getElementById('fileInput');
        
        // Load saved data
        this.apiKey = localStorage.getItem('deepseek_api_key') || '';
        this.model = localStorage.getItem('deepseek_model') || 'deepseek-chat';
        this.useLocalAI = localStorage.getItem('use_local_ai') === 'true';
        
        if (this.apiKey) {
            document.getElementById('apiKeyInput').value = this.apiKey;
            document.getElementById('modelSelect').value = this.model;
        }
        
        const saved = localStorage.getItem('code_main.js');
        if (saved) {
            this.editor.value = saved;
            this.fileData['main.js'] = saved;
        } else {
            this.fileData['main.js'] = this.editor.value;
        }
        
        this.updateLines();
        this.updateCursor();
        this.checkAPIStatus();
        
        // Update model buttons
        if (this.useLocalAI) {
            document.querySelectorAll('.model-btn').forEach(b => b.classList.remove('active'));
            document.querySelectorAll('.model-btn')[1].classList.add('active');
            document.getElementById('modelBadge').textContent = 'Local AI';
        }
        
        // Events
        this.editor.addEventListener('input', () => {
            this.updateLines();
            this.fileData[this.currentFile] = this.editor.value;
            localStorage.setItem('code_' + this.currentFile, this.editor.value);
        });
        
        this.editor.addEventListener('scroll', () => {
            this.lineNums.scrollTop = this.editor.scrollTop;
        });
        
        this.editor.addEventListener('click', () => this.updateCursor());
        this.editor.addEventListener('keyup', () => this.updateCursor());
        
        this.promptInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                this.sendPrompt();
            }
        });
        
        this.fileInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = (ev) => {
                this.fileData[this.currentFile] = this.editor.value;
                this.currentFile = file.name;
                if (!this.openFiles.includes(file.name)) this.openFiles.push(file.name);
                this.editor.value = ev.target.result;
                this.fileData[file.name] = ev.target.result;
                this.updateLines();
                this.updateLang();
                this.updateTabs();
                this.toast('Opened: ' + file.name, 'success');
            };
            reader.readAsText(file);
            this.fileInput.value = '';
        });
        
        // Keyboard shortcuts
        document.addEventListener('keydown', (e) => {
            if (e.ctrlKey || e.metaKey) {
                if (e.key === 's') { e.preventDefault(); this.saveFile(); }
                if (e.key === 'o') { e.preventDefault(); this.openFile(); }
                if (e.key === 'n') { e.preventDefault(); this.newFile(); }
                if (e.key === ',') { e.preventDefault(); this.showSettings(); }
            }
        });
        
        // Show settings on first use if no API key
        if (!this.apiKey && !this.useLocalAI) {
            setTimeout(() => {
                this.addAIMessage('⚠️ <b>DeepSeek API key not set.</b><br><br>Options:<br>1. Click <b>⚙️ Settings</b> to add your API key<br>2. Switch to <b>Local AI</b> mode (free, no key needed)<br><br>Press <b>Ctrl+,</b> for settings or click Local AI button below.');
            }, 500);
        }
        
        console.log('✅ Code Studio Pro initialized!');
    },
    
    // ============ EDITOR FUNCTIONS ============
    updateLines() {
        const lines = this.editor.value.split('\n');
        this.lineNums.textContent = Array.from({length: lines.length}, (_, i) => i + 1).join('\n');
    },
    
    updateCursor() {
        const pos = this.editor.selectionStart;
        const text = this.editor.value.substring(0, pos);
        const lines = text.split('\n');
        document.getElementById('cursorLabel').textContent = 
            `Ln ${lines.length}, Col ${lines[lines.length-1].length + 1}`;
    },
    
    updateLang() {
        const ext = this.currentFile.split('.').pop()?.toLowerCase() || 'js';
        const map = {js:'JavaScript',py:'Python',html:'HTML',css:'CSS',cpp:'C++',java:'Java',
                     ts:'TypeScript',jsx:'React JSX',php:'PHP',rb:'Ruby',go:'Go',rs:'Rust',
                     sql:'SQL',json:'JSON',md:'Markdown'};
        document.getElementById('langLabel').textContent = map[ext] || 'Plain Text';
    },
    
    updateTabs() {
        document.getElementById('tabsBar').innerHTML = this.openFiles.map(f => {
            const active = f === this.currentFile ? ' active' : '';
            const closeBtn = this.openFiles.length > 1 ? 
                `<span class="close-tab" onclick="event.stopPropagation();Studio.closeTab('${f}')">×</span>` : '';
            return `<div class="tab${active}" data-file="${f}" onclick="Studio.switchFile('${f}')">
                <span>📄 ${f}</span>${closeBtn}</div>`;
        }).join('');
    },
    
    newFile() {
        const name = prompt('File name:', 'untitled.js');
        if (!name) return;
        this.fileData[this.currentFile] = this.editor.value;
        this.currentFile = name;
        if (!this.openFiles.includes(name)) this.openFiles.push(name);
        const ext = name.split('.').pop()?.toLowerCase();
        const templates = {
            js: '// JavaScript\n\nconsole.log("Hello World!");\n',
            py: '# Python\n\nprint("Hello World!")\n',
            html: '<!DOCTYPE html>\n<html lang="en">\n<head>\n    <meta charset="UTF-8">\n    <title>Page</title>\n</head>\n<body>\n    <h1>Hello World!</h1>\n</body>\n</html>',
            css: '/* CSS */\n\nbody {\n    font-family: Arial, sans-serif;\n    margin: 0;\n    padding: 20px;\n}\n',
            cpp: '#include <iostream>\nusing namespace std;\n\nint main() {\n    cout << "Hello World!" << endl;\n    return 0;\n}\n'
        };
        this.editor.value = templates[ext] || '';
        this.fileData[name] = this.editor.value;
        this.updateLines();
        this.updateLang();
        this.updateTabs();
        this.toast('Created: ' + name, 'success');
    },
    
    openFile() { this.fileInput.click(); },
    
    saveFile() {
        const blob = new Blob([this.editor.value], {type:'text/plain'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = this.currentFile;
        a.click();
        URL.revokeObjectURL(url);
        this.fileData[this.currentFile] = this.editor.value;
        localStorage.setItem('code_' + this.currentFile, this.editor.value);
        this.toast('Saved: ' + this.currentFile, 'success');
    },
    
    formatCode() {
        let lines = this.editor.value.split('\n');
        let indent = 0;
        lines = lines.map(line => {
            const t = line.trim();
            if (!t) return '';
            if (t.startsWith('}') || t.startsWith(']') || t.startsWith(')')) indent = Math.max(0, indent-1);
            const r = '    '.repeat(indent) + t;
            if (t.endsWith('{') || t.endsWith('[') || t.endsWith('(')) indent++;
            return r;
        });
        this.editor.value = lines.join('\n');
        this.updateLines();
        this.toast('Code formatted', 'success');
    },
    
    runCode() {
        try {
            const result = eval(this.editor.value);
            this.toast('✅ Executed! Result: ' + (result !== undefined ? result : 'undefined'), 'success');
        } catch(e) {
            this.toast('❌ Error: ' + e.message, 'error');
        }
    },
    
    togglePreview() {
        this.toast('Preview feature - save as .html and open in browser', 'info');
    },
    
    switchFile(name) {
        this.fileData[this.currentFile] = this.editor.value;
        this.currentFile = name;
        this.editor.value = this.fileData[name] || '';
        this.updateLines();
        this.updateLang();
        this.updateTabs();
    },
    
    closeTab(name) {
        if (this.openFiles.length <= 1) return;
        const idx = this.openFiles.indexOf(name);
        this.openFiles.splice(idx, 1);
        if (this.currentFile === name) {
            this.currentFile = this.openFiles[Math.max(0, idx-1)];
            this.editor.value = this.fileData[this.currentFile] || '';
        }
        this.updateLines();
        this.updateLang();
        this.updateTabs();
    },
    
    // ============ AI FUNCTIONS ============
    checkAPIStatus() {
        const dot = document.getElementById('apiDot');
        const label = document.getElementById('apiLabel');
        
        if (this.useLocalAI) {
            dot.className = 'api-dot online';
            label.textContent = 'Local AI Ready';
            return;
        }
        
        if (this.apiKey) {
            // Test API key
            fetch('https://api.deepseek.com/v1/models', {
                headers: { 'Authorization': `Bearer ${this.apiKey}` }
            })
            .then(r => {
                if (r.ok) {
                    dot.className = 'api-dot online';
                    label.textContent = 'DeepSeek API Connected';
                } else {
                    throw new Error('Invalid key');
                }
            })
            .catch(() => {
                dot.className = 'api-dot offline';
                label.textContent = 'API Key Invalid';
            });
        } else {
            dot.className = 'api-dot offline';
            label.textContent = 'No API Key Set';
        }
    },
    
    setModel(type) {
        this.useLocalAI = type === 'local';
        localStorage.setItem('use_local_ai', this.useLocalAI);
        
        document.querySelectorAll('.model-btn').forEach(b => b.classList.remove('active'));
        if (type === 'deepseek') {
            document.querySelectorAll('.model-btn')[0].classList.add('active');
            document.getElementById('modelBadge').textContent = 'DeepSeek-V3';
            if (!this.apiKey) {
                this.showSettings();
                return;
            }
        } else {
            document.querySelectorAll('.model-btn')[1].classList.add('active');
            document.getElementById('modelBadge').textContent = 'Local AI';
        }
        
        this.checkAPIStatus();
        this.toast(`Switched to ${type === 'deepseek' ? 'DeepSeek API' : 'Local AI'}`, 'info');
    },
    
    setPrompt(text) {
        this.promptInput.value = text;
        this.promptInput.focus();
        this.promptInput.style.height = 'auto';
        this.promptInput.style.height = Math.min(this.promptInput.scrollHeight, 100) + 'px';
    },
    
    async sendPrompt() {
        const text = this.promptInput.value.trim();
        if (!text) { this.toast('Please type a message', 'error'); return; }
        if (this.processing) { this.toast('Still processing...', 'error'); return; }
        
        // Add user message
        this.addUserMessage(text);
        this.promptInput.value = '';
        this.promptInput.style.height = '44px';
        
        // Show thinking
        const thinkingDiv = this.addThinking();
        
        this.processing = true;
        this.sendBtn.disabled = true;
        this.sendBtn.innerHTML = '⏳';
        
        let response;
        
        if (this.useLocalAI || !this.apiKey) {
            // Use local AI
            response = await this.callLocalAI(text);
        } else {
            // Use DeepSeek API
            response = await this.callDeepSeekAPI(text);
        }
        
        // Remove thinking
        thinkingDiv.remove();
        
        // Add AI response
        this.addAIResponse(response);
        
        this.processing = false;
        this.sendBtn.disabled = false;
        this.sendBtn.innerHTML = '📤<br>Send';
    },
    
    addUserMessage(text) {
        const div = document.createElement('div');
        div.className = 'msg msg-user';
        div.textContent = text;
        this.chatBox.appendChild(div);
        this.chatBox.scrollTop = this.chatBox.scrollHeight;
    },
    
    addThinking() {
        const div = document.createElement('div');
        div.className = 'msg msg-ai';
        div.innerHTML = '<div class="thinking"><span></span><span></span><span></span></div><small style="color:#999;">Generating code...</small>';
        this.chatBox.appendChild(div);
        this.chatBox.scrollTop = this.chatBox.scrollHeight;
        return div;
    },
    
    addAIMessage(html) {
        const div = document.createElement('div');
        div.className = 'msg msg-ai';
        div.innerHTML = html;
        this.chatBox.appendChild(div);
        this.chatBox.scrollTop = this.chatBox.scrollHeight;
    },
    
    addAIResponse(response) {
        const div = document.createElement('div');
        div.className = 'msg msg-ai';
        
        let html = '';
        
        if (response.text) {
            html += `<div>${response.text}</div>`;
        }
        
        if (response.code) {
            html += `<pre><code>${this.escHtml(response.code)}</code></pre>`;
            html += '<div class="code-actions">';
            html += `<button class="ca-insert" onclick="Studio.insertCode(\`${this.escJS(response.code)}\`)">📋 Insert</button>`;
            html += `<button class="ca-replace" onclick="Studio.replaceCode(\`${this.escJS(response.code)}\`)">🔄 Replace</button>`;
            html += `<button class="ca-copy" onclick="Studio.copyCode(\`${this.escJS(response.code)}\`)">📝 Copy</button>`;
            html += `<button class="ca-newfile" onclick="Studio.newFromCode(\`${this.escJS(response.code)}\`,'${response.lang||'js'}')">📄 New File</button>`;
            if (response.explain) {
                html += `<button class="ca-explain" onclick="Studio.explainCode(\`${this.escJS(response.code)}\`)">💡 Explain</button>`;
            }
            html += '</div>';
        }
        
        div.innerHTML = html;
        this.chatBox.appendChild(div);
        this.chatBox.scrollTop = this.chatBox.scrollHeight;
    },
    
    escHtml(t) {
        const d = document.createElement('div');
        d.textContent = t;
        return d.innerHTML;
    },
    
    escJS(t) {
        return t.replace(/\\/g,'\\\\').replace(/`/g,'\\`').replace(/\$/g,'\\$').replace(/'/g,"\\'");
    },
    
    // ============ DEEPSEEK API ============
    async callDeepSeekAPI(prompt) {
        try {
            const systemPrompt = `You are an expert software engineer and coding assistant. 
Generate high-quality, production-ready code with:
- Clear comments and documentation
- Error handling
- Best practices
- Complete working examples
- Type hints (where applicable)
Respond with the code in markdown code blocks. Include a brief explanation.`;

            const response = await fetch('https://api.deepseek.com/v1/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${this.apiKey}`
                },
                body: JSON.stringify({
                    model: this.model,
                    messages: [
                        { role: 'system', content: systemPrompt },
                        { role: 'user', content: `Current file: ${this.currentFile}\n\nCode in editor:\n\`\`\`\n${this.editor.value}\n\`\`\`\n\nUser request: ${prompt}` }
                    ],
                    temperature: 0.5,
                    max_tokens: 3000,
                    top_p: 0.95
                })
            });
            
            if (!response.ok) {
                const err = await response.json().catch(() => ({}));
                if (response.status === 401) {
                    throw new Error('Invalid API key. Please check your key in Settings.');
                } else if (response.status === 402) {
                    throw new Error('Insufficient balance. Please top up your DeepSeek account.');
                } else if (response.status === 429) {
                    throw new Error('Rate limit exceeded. Please wait and try again.');
                }
                throw new Error(err.error?.message || `API Error: ${response.status}`);
            }
            
            const data = await response.json();
            const content = data.choices[0].message.content;
            
            return this.parseAIResponse(content);
            
        } catch (error) {
            console.error('DeepSeek API Error:', error);
            return {
                text: `❌ <b>DeepSeek API Error:</b> ${error.message}<br><br>Falling back to Local AI...`,
                code: await this.generateLocalCode(prompt),
                lang: this.detectLangFromPrompt(prompt)
            };
        }
    },
    
    parseAIResponse(content) {
        // Extract code blocks
        const codeMatch = content.match(/```(\w+)?\s*\n([\s\S]*?)```/);
        const code = codeMatch ? codeMatch[2].trim() : '';
        const lang = codeMatch ? codeMatch[1] || this.detectLangFromCode(code) : 'text';
        
        // Extract explanation (everything before/after code)
        let text = content.replace(/```[\s\S]*?```/, '').trim();
        if (!text) text = '✅ <b>Generated Code:</b>';
        
        return { text, code, lang, explain: true };
    },
    
    // ============ LOCAL AI (Advanced) ============
    async callLocalAI(prompt) {
        // Simulate API delay for realism
        await new Promise(r => setTimeout(r, 800 + Math.random() * 1200));
        const code = this.generateLocalCode(prompt);
        const lang = this.detectLangFromPrompt(prompt);
        return {
            text: `✅ <b>Generated ${lang.toUpperCase()} Code:</b>`,
            code,
            lang,
            explain: true
        };
    },
    
    generateLocalCode(prompt) {
        const p = prompt.toLowerCase();
        
        // Python - REST API
        if (p.includes('rest') || (p.includes('api') && p.includes('python')) || p.includes('flask')) {
            return `from flask import Flask, request, jsonify
from flask_cors import CORS
from datetime import datetime
import sqlite3

app = Flask(__name__)
CORS(app)

# Database setup
def init_db():
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS items (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            description TEXT,
            price REAL DEFAULT 0.0,
            created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        )
    ''')
    conn.commit()
    conn.close()

init_db()

# Routes
@app.route('/api/items', methods=['GET'])
def get_items():
    """Fetch all items from database."""
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM items ORDER BY created_at DESC')
    items = [{'id': row[0], 'name': row[1], 'description': row[2], 
              'price': row[3], 'created_at': row[4]} for row in cursor.fetchall()]
    conn.close()
    return jsonify({'items': items, 'count': len(items)})

@app.route('/api/items/<int:item_id>', methods=['GET'])
def get_item(item_id):
    """Fetch a single item by ID."""
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    cursor.execute('SELECT * FROM items WHERE id = ?', (item_id,))
    row = cursor.fetchone()
    conn.close()
    if not row:
        return jsonify({'error': 'Item not found'}), 404
    return jsonify({'id': row[0], 'name': row[1], 'description': row[2], 
                    'price': row[3], 'created_at': row[4]})

@app.route('/api/items', methods=['POST'])
def create_item():
    """Create a new item."""
    data = request.get_json()
    if not data or 'name' not in data:
        return jsonify({'error': 'Name is required'}), 400
    
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    cursor.execute('INSERT INTO items (name, description, price) VALUES (?, ?, ?)',
                   (data['name'], data.get('description', ''), data.get('price', 0.0)))
    conn.commit()
    item_id = cursor.lastrowid
    conn.close()
    return jsonify({'id': item_id, 'message': 'Item created'}), 201

@app.route('/api/items/<int:item_id>', methods=['PUT'])
def update_item(item_id):
    """Update an existing item."""
    data = request.get_json()
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    cursor.execute('UPDATE items SET name=?, description=?, price=? WHERE id=?',
                   (data.get('name'), data.get('description'), data.get('price'), item_id))
    conn.commit()
    conn.close()
    return jsonify({'message': 'Item updated'})

@app.route('/api/items/<int:item_id>', methods=['DELETE'])
def delete_item(item_id):
    """Delete an item."""
    conn = sqlite3.connect('app.db')
    cursor = conn.cursor()
    cursor.execute('DELETE FROM items WHERE id = ?', (item_id,))
    conn.commit()
    conn.close()
    return jsonify({'message': 'Item deleted'})

if __name__ == '__main__':
    app.run(debug=True, port=5000)`;
        }
        
        // HTML Dashboard
        if (p.includes('dashboard') || (p.includes('html') && p.includes('chart'))) {
            return `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analytics Dashboard</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:'Segoe UI',system-ui,sans-serif;background:#f0f2f5;display:flex}
        .sidebar{width:250px;background:#1a1a2e;color:#fff;min-height:100vh;padding:20px}
        .sidebar h2{margin-bottom:30px;font-size:20px}
        .sidebar a{color:#b0b0b0;text-decoration:none;display:block;padding:10px;margin:5px 0;border-radius:8px;transition:all 0.3s}
        .sidebar a:hover,.sidebar a.active{background:#16213e;color:#fff}
        .main{flex:1;padding:30px}
        .header{display:flex;justify-content:space-between;align-items:center;margin-bottom:30px}
        .header h1{font-size:28px;color:#1a1a2e}
        .stats{display:grid;grid-template-columns:repeat(4,1fr);gap:20px;margin-bottom:30px}
        .stat-card{background:#fff;padding:25px;border-radius:12px;box-shadow:0 2px 10px rgba(0,0,0,0.08)}
        .stat-card h3{color:#666;font-size:13px;text-transform:uppercase;margin-bottom:10px}
        .stat-card .value{font-size:32px;font-weight:700;color:#1a1a2e}
        .stat-card .change{font-size:13px;margin-top:5px}
        .change.up{color:#2ecc71}
        .change.down{color:#e74c3c}
        .charts{display:grid;grid-template-columns:2fr 1fr;gap:20px}
        .chart-card{background:#fff;padding:25px;border-radius:12px;box-shadow:0 2px 10px rgba(0,0,0,0.08)}
        .chart-card h3{margin-bottom:20px;color:#333}
        table{width:100%;border-collapse:collapse}
        th{text-align:left;padding:12px;border-bottom:2px solid #eee;color:#666;font-size:12px;text-transform:uppercase}
        td{padding:12px;border-bottom:1px solid #f0f0f0;font-size:14px}
        .status{display:inline-block;padding:4px 10px;border-radius:12px;font-size:11px;font-weight:600}
        .status.active{background:#e8f5e9;color:#2ecc71}
        .status.pending{background:#fff3e0;color:#f39c12}
    </style>
</head>
<body>
    <div class="sidebar">
        <h2>📊 Analytics</h2>
        <a href="#" class="active">Dashboard</a>
        <a href="#">Analytics</a>
        <a href="#">Users</a>
        <a href="#">Reports</a>
        <a href="#">Settings</a>
    </div>
    <div class="main">
        <div class="header">
            <h1>Dashboard Overview</h1>
            <div>Welcome back, Admin 👋</div>
        </div>
        <div class="stats">
            <div class="stat-card">
                <h3>Total Users</h3>
                <div class="value">12,847</div>
                <div class="change up">↑ 12.5% from last month</div>
            </div>
            <div class="stat-card">
                <h3>Revenue</h3>
                <div class="value">$48,295</div>
                <div class="change up">↑ 8.2% from last month</div>
            </div>
            <div class="stat-card">
                <h3>Orders</h3>
                <div class="value">2,456</div>
                <div class="change down">↓ 3.1% from last month</div>
            </div>
            <div class="stat-card">
                <h3>Conversion</h3>
                <div class="value">3.24%</div>
                <div class="change up">↑ 0.8% from last month</div>
            </div>
        </div>
        <div class="charts">
            <div class="chart-card">
                <h3>Recent Orders</h3>
                <table>
                    <tr><th>Order ID</th><th>Customer</th><th>Amount</th><th>Status</th></tr>
                    <tr><td>#ORD-001</td><td>John Doe</td><td>$120.00</td><td><span class="status active">Completed</span></td></tr>
                    <tr><td>#ORD-002</td><td>Jane Smith</td><td>$85.50</td><td><span class="status pending">Pending</span></td></tr>
                    <tr><td>#ORD-003</td><td>Bob Johnson</td><td>$250.00</td><td><span class="status active">Completed</span></td></tr>
                    <tr><td>#ORD-004</td><td>Alice Williams</td><td>$45.00</td><td><span class="status pending">Processing</span></td></tr>
                </table>
            </div>
            <div class="chart-card">
                <h3>Top Products</h3>
                <div style="padding:10px 0;">🥇 Product A - 456 sales</div>
                <div style="padding:10px 0;">🥈 Product B - 389 sales</div>
                <div style="padding:10px 0;">🥉 Product C - 312 sales</div>
                <div style="padding:10px 0;">4. Product D - 278 sales</div>
                <div style="padding:10px 0;">5. Product E - 234 sales</div>
            </div>
        </div>
    </div>
</body>
</html>`;
        }
        
        // C++ Merge Sort
        if ((p.includes('c++') || p.includes('cpp')) && (p.includes('merge') || p.includes('sort'))) {
            return `#include <iostream>
#include <vector>
#include <chrono>
using namespace std;
using namespace std::chrono;

// Merge two sorted subarrays
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    vector<int> L(n1), R(n2);
    
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];
    
    int i = 0, j = 0, k = left;
    
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    while (i < n1) {
        arr[k] = L[i];
        i++; k++;
    }
    
    while (j < n2) {
        arr[k] = R[j];
        j++; k++;
    }
}

// Merge Sort
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

// Print array
void printArray(const vector<int>& arr, const string& label) {
    cout << label << ": ";
    for (int num : arr) cout << num << " ";
    cout << endl;
}

int main() {
    vector<int> arr = {64, 34, 25, 12, 22, 11, 90, 45, 33, 77, 55, 88};
    
    printArray(arr, "Original array");
    
    auto start = high_resolution_clock::now();
    mergeSort(arr, 0, arr.size() - 1);
    auto end = high_resolution_clock::now();
    
    printArray(arr, "Sorted array  ");
    
    auto duration = duration_cast<microseconds>(end - start);
    cout << "\\nTime taken: " << duration.count() << " microseconds" << endl;
    cout << "Array size: " << arr.size() << " elements" << endl;
    
    return 0;
}`;
        }
        
        // React Todo App
        if (p.includes('react') && (p.includes('todo') || p.includes('app'))) {
            return `import React, { useState, useEffect } from 'react';

const TodoApp = () => {
    const [todos, setTodos] = useState(() => {
        const saved = localStorage.getItem('todos');
        return saved ? JSON.parse(saved) : [];
    });
    const [input, setInput] = useState('');
    const [filter, setFilter] = useState('all');
    
    useEffect(() => {
        localStorage.setItem('todos', JSON.stringify(todos));
    }, [todos]);
    
    const addTodo = () => {
        if (!input.trim()) return;
        const newTodo = {
            id: Date.now(),
            text: input.trim(),
            completed: false,
            createdAt: new Date().toISOString()
        };
        setTodos(prev => [newTodo, ...prev]);
        setInput('');
    };
    
    const toggleTodo = (id) => {
        setTodos(prev => prev.map(todo =>
            todo.id === id ? { ...todo, completed: !todo.completed } : todo
        ));
    };
    
    const deleteTodo = (id) => {
        setTodos(prev => prev.filter(todo => todo.id !== id));
    };
    
    const clearCompleted = () => {
        setTodos(prev => prev.filter(todo => !todo.completed));
    };
    
    const filteredTodos = todos.filter(todo => {
        if (filter === 'active') return !todo.completed;
        if (filter === 'completed') return todo.completed;
        return true;
    });
    
    const stats = {
        total: todos.length,
        active: todos.filter(t => !t.completed).length,
        completed: todos.filter(t => t.completed).length
    };
    
    return (
        <div style={styles.container}>
            <h1 style={styles.title}>📝 Todo App</h1>
            
            <div style={styles.inputGroup}>
                <input
                    type="text"
                    value={input}
                    onChange={e => setInput(e.target.value)}
                    onKeyPress={e => e.key === 'Enter' && addTodo()}
                    placeholder="Add a new todo..."
                    style={styles.input}
                />
                <button onClick={addTodo} style={styles.addBtn}>Add</button>
            </div>
            
            <div style={styles.filters}>
                {['all', 'active', 'completed'].map(f => (
                    <button
                        key={f}
                        onClick={() => setFilter(f)}
                        style={{
                            ...styles.filterBtn,
                            background: filter === f ? '#667eea' : '#e0e0e0',
                            color: filter === f ? '#fff' : '#333'
                        }}
                    >
                        {f.charAt(0).toUpperCase() + f.slice(1)}
                    </button>
                ))}
            </div>
            
            <div style={styles.list}>
                {filteredTodos.length === 0 ? (
                    <p style={styles.empty}>No todos found 📭</p>
                ) : (
                    filteredTodos.map(todo => (
                        <div key={todo.id} style={styles.todoItem}>
                            <input
                                type="checkbox"
                                checked={todo.completed}
                                onChange={() => toggleTodo(todo.id)}
                                style={styles.checkbox}
                            />
                            <span style={{
                                ...styles.todoText,
                                textDecoration: todo.completed ? 'line-through' : 'none',
                                color: todo.completed ? '#999' : '#333'
                            }}>
                                {todo.text}
                            </span>
                            <button
                                onClick={() => deleteTodo(todo.id)}
                                style={styles.deleteBtn}
                            >
                                ×
                            </button>
                        </div>
                    ))
                )}
            </div>
            
            <div style={styles.footer}>
                <span>{stats.active} items left</span>
                {stats.completed > 0 && (
                    <button onClick={clearCompleted} style={styles.clearBtn}>
                        Clear completed ({stats.completed})
                    </button>
                )}
            </div>
        </div>
    );
};

const styles = {
    container: {
        maxWidth: '500px',
        margin: '40px auto',
        padding: '30px',
        background: '#fff',
        borderRadius: '16px',
        boxShadow: '0 4px 30px rgba(0,0,0,0.1)',
        fontFamily: 'Segoe UI, sans-serif'
    },
    title: {
        textAlign: 'center',
        color: '#333',
        marginBottom: '24px',
        fontSize: '28px'
    },
    inputGroup: {
        display: 'flex',
        gap: '8px',
        marginBottom: '20px'
    },
    input: {
        flex: 1,
        padding: '12px 16px',
        border: '2px solid #e0e0e0',
        borderRadius: '10px',
        fontSize: '16px',
        outline: 'none',
        transition: 'border-color 0.3s'
    },
    addBtn: {
        padding: '12px 24px',
        background: 'linear-gradient(135deg, #667eea, #764ba2)',
        color: '#fff',
        border: 'none',
        borderRadius: '10px',
        cursor: 'pointer',
        fontSize: '16px',
        fontWeight: '600'
    },
    filters: {
        display: 'flex',
        gap: '8px',
        marginBottom: '20px',
        justifyContent: 'center'
    },
    filterBtn: {
        padding: '6px 16px',
        border: 'none',
        borderRadius: '20px',
        cursor: 'pointer',
        fontSize: '13px',
        fontWeight: '500',
        transition: 'all 0.2s'
    },
    list: {
        marginBottom: '20px',
        minHeight: '200px'
    },
    empty: {
        textAlign: 'center',
        color: '#999',
        padding: '40px'
    },
    todoItem: {
        display: 'flex',
        alignItems: 'center',
        padding: '12px',
        borderBottom: '1px solid #f0f0f0',
        gap: '12px'
    },
    checkbox: {
        width: '20px',
        height: '20px',
        cursor: 'pointer'
    },
    todoText: {
        flex: 1,
        fontSize: '15px',
        transition: 'all 0.3s'
    },
    deleteBtn: {
        background: 'none',
        border: 'none',
        color: '#e74c3c',
        fontSize: '22px',
        cursor: 'pointer',
        padding: '0 8px',
        borderRadius: '4px'
    },
    footer: {
        display: 'flex',
        justifyContent: 'space-between',
        alignItems: 'center',
        padding: '12px 0',
        borderTop: '2px solid #f0f0f0',
        fontSize: '13px',
        color: '#666'
    },
    clearBtn: {
        background: 'none',
        border: 'none',
        color: '#e74c3c',
        cursor: 'pointer',
        fontSize: '13px'
    }
};

export default TodoApp;`;
        }
        
        // Default: JavaScript
        return `// Generated JavaScript Code

class Solution {
    constructor(data) {
        this.data = data || [];
    }
    
    /**
     * Process the data with error handling
     * @returns {Object} Processed results
     */
    process() {
        try {
            if (!this.data.length) {
                throw new Error('No data to process');
            }
            
            const results = {
                original: [...this.data],
                filtered: this.data.filter(item => item !== null && item !== undefined),
                mapped: this.data.map((item, index) => ({
                    index,
                    value: item,
                    doubled: item * 2
                })),
                statistics: this.calculateStats()
            };
            
            return results;
        } catch (error) {
            console.error('Processing error:', error);
            return { error: error.message };
        }
    }
    
    calculateStats() {
        const validData = this.data.filter(n => typeof n === 'number');
        const sum = validData.reduce((a, b) => a + b, 0);
        const avg = validData.length ? sum / validData.length : 0;
        
        return {
            count: validData.length,
            sum,
            average: avg.toFixed(2),
            min: Math.min(...validData),
            max: Math.max(...validData)
        };
    }
}

// Usage example
const solver = new Solution([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);
const result = solver.process();
console.log('Results:', JSON.stringify(result, null, 2));`;
    },
    
    detectLangFromPrompt(prompt) {
        const p = prompt.toLowerCase();
        if (p.includes('python') || p.includes('flask') || p.includes('django')) return 'py';
        if (p.includes('html') || p.includes('css') || p.includes('webpage') || p.includes('dashboard')) return 'html';
        if (p.includes('c++') || p.includes('cpp')) return 'cpp';
        if (p.includes('react') || p.includes('jsx')) return 'jsx';
        if (p.includes('javascript') || p.includes('node')) return 'js';
        return 'js';
    },
    
    detectLangFromCode(code) {
        if (code.includes('def ') || code.includes('import ') || code.includes('print(')) return 'py';
        if (code.includes('<!DOCTYPE') || code.includes('<html>')) return 'html';
        if (code.includes('#include') || code.includes('cout')) return 'cpp';
        if (code.includes('import React') || code.includes('useState')) return 'jsx';
        return 'js';
    },
    
    // ============ CODE ACTIONS ============
    insertCode(code) {
        const pos = this.editor.selectionStart;
        this.editor.value = this.editor.value.substring(0, pos) + '\n' + code + '\n' + this.editor.value.substring(pos);
        this.updateLines();
        this.fileData[this.currentFile] = this.editor.value;
        this.editor.focus();
        this.toast('✅ Code inserted at cursor!', 'success');
    },
    
    replaceCode(code) {
        if (this.editor.value.trim() && !confirm('Replace all code in editor?')) return;
        this.editor.value = code;
        this.updateLines();
        this.fileData[this.currentFile] = code;
        this.updateLang();
        this.toast('🔄 Code replaced!', 'success');
    },
    
    copyCode(code) {
        if (navigator.clipboard?.writeText) {
            navigator.clipboard.writeText(code).then(() => this.toast('📝 Copied!', 'success'));
        } else {
            const ta = document.createElement('textarea');
            ta.value = code;
            ta.style.cssText = 'position:fixed;left:-9999px';
            document.body.appendChild(ta);
            ta.select();
            document.execCommand('copy');
            document.body.removeChild(ta);
            this.toast('📝 Copied!', 'success');
        }
    },
    
    newFromCode(code, lang) {
        const exts = {py:'py',js:'js',html:'html',css:'css',cpp:'cpp',jsx:'jsx',ts:'ts'};
        const ext = exts[lang] || 'js';
        const name = `generated_${Date.now()}.${ext}`;
        this.fileData[this.currentFile] = this.editor.value;
        this.currentFile = name;
        this.openFiles.push(name);
        this.editor.value = code;
        this.fileData[name] = code;
        this.updateLines();
        this.updateLang();
        this.updateTabs();
        this.toast('📄 Created: ' + name, 'success');
    },
    
    explainCode(code) {
        this.setPrompt(`Explain this code in detail:\n\`\`\`\n${code}\n\`\`\``);
        this.sendPrompt();
    },
    
    // ============ SETTINGS ============
    showSettings() {
        document.getElementById('settingsModal').style.display = 'flex';
        document.getElementById('apiKeyInput').value = this.apiKey;
        document.getElementById('modelSelect').value = this.model;
    },
    
    closeSettings() {
        document.getElementById('settingsModal').style.display = 'none';
    },
    
    saveSettings() {
        this.apiKey = document.getElementById('apiKeyInput').value.trim();
        this.model = document.getElementById('modelSelect').value;
        
        localStorage.setItem('deepseek_api_key', this.apiKey);
        localStorage.setItem('deepseek_model', this.model);
        
        if (this.apiKey) {
            this.useLocalAI = false;
            localStorage.setItem('use_local_ai', 'false');
            document.querySelectorAll('.model-btn').forEach(b => b.classList.remove('active'));
            document.querySelectorAll('.model-btn')[0].classList.add('active');
            document.getElementById('modelBadge').textContent = 'DeepSeek-V3';
        }
        
        this.closeSettings();
        this.checkAPIStatus();
        this.toast(this.apiKey ? '✅ API key saved! Connected to DeepSeek.' : '⚠️ No API key set. Using Local AI.', 
                  this.apiKey ? 'success' : 'info');
    },
    
    // ============ UTILITIES ============
    toast(msg, type) {
        const container = document.getElementById('toastContainer');
        const toast = document.createElement('div');
        toast.className = `toast toast-${type}`;
        toast.textContent = msg;
        container.appendChild(toast);
        setTimeout(() => {
            toast.style.opacity = '0';
            toast.style.transition = 'opacity 0.3s';
            setTimeout(() => toast.remove(), 300);
        }, 2000);
    }
};

// Initialize
Studio.init();
console.log('🚀 Code Studio Pro ready!');
console.log('🧠 DeepSeek API:', Studio.apiKey ? 'Configured' : 'Not set - using Local AI');
console.log('💡 Press Ctrl+, for settings');
</script>
</body>
</html>
