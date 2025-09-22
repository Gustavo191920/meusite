<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Link Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: #0a0a0a;
            color: #e1e1e1;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            overflow-x: hidden;
        }

        .container {
            background: rgba(18, 18, 18, 0.95);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(40, 40, 40, 0.5);
            border-radius: 24px;
            padding: 48px;
            width: 100%;
            max-width: 460px;
            box-shadow: 0 32px 64px rgba(0, 0, 0, 0.5);
            position: relative;
        }

        .container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 1px;
            background: linear-gradient(90deg, transparent, #333, transparent);
        }

        h1 {
            font-size: 2rem;
            font-weight: 700;
            text-align: center;
            margin-bottom: 48px;
            background: linear-gradient(135deg, #ffffff 0%, #888888 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: -0.02em;
        }

        .input-group {
            margin-bottom: 32px;
            position: relative;
        }

        .input-group:last-of-type {
            margin-bottom: 40px;
        }

        label {
            display: block;
            font-size: 0.875rem;
            font-weight: 500;
            color: #888;
            margin-bottom: 12px;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        input[type="text"] {
            width: 100%;
            background: rgba(26, 26, 26, 0.8);
            border: 1px solid rgba(40, 40, 40, 0.6);
            border-radius: 12px;
            padding: 16px 20px;
            font-size: 1rem;
            color: #e1e1e1;
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
            font-family: 'SF Mono', 'Monaco', 'Cascadia Code', monospace;
        }

        input[type="text"]::placeholder {
            color: #555;
        }

        input[type="text"]:focus {
            outline: none;
            border-color: #333;
            background: rgba(30, 30, 30, 0.9);
            transform: translateY(-1px);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        .generate-btn {
            width: 100%;
            background: linear-gradient(135deg, #1a1a1a 0%, #2a2a2a 100%);
            border: 1px solid #333;
            border-radius: 12px;
            padding: 18px;
            font-size: 1rem;
            font-weight: 600;
            color: #e1e1e1;
            cursor: pointer;
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
        }

        .generate-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            transition: left 0.5s;
        }

        .generate-btn:hover {
            transform: translateY(-2px);
            border-color: #444;
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.4);
        }

        .generate-btn:hover::before {
            left: 100%;
        }

        .generate-btn:active {
            transform: translateY(0);
        }

        .result {
            margin-top: 40px;
            opacity: 0;
            transform: translateY(20px);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            pointer-events: none;
        }

        .result.show {
            opacity: 1;
            transform: translateY(0);
            pointer-events: auto;
        }

        .result-label {
            font-size: 0.875rem;
            font-weight: 500;
            color: #888;
            margin-bottom: 12px;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        .result-box {
            background: rgba(12, 12, 12, 0.8);
            border: 1px solid rgba(30, 30, 30, 0.8);
            border-radius: 12px;
            padding: 20px;
            position: relative;
            overflow: hidden;
        }

        .result-text {
            font-family: 'SF Mono', 'Monaco', 'Cascadia Code', monospace;
            font-size: 0.95rem;
            color: #e1e1e1;
            word-break: break-all;
            line-height: 1.6;
            margin-bottom: 16px;
        }

        .copy-btn {
            background: transparent;
            border: 1px solid rgba(40, 40, 40, 0.8);
            border-radius: 8px;
            padding: 10px 16px;
            font-size: 0.875rem;
            color: #888;
            cursor: pointer;
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .copy-btn:hover {
            border-color: #555;
            color: #e1e1e1;
        }

        .copy-btn.copied {
            border-color: #4ade80;
            color: #4ade80;
        }

        .floating-elements {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .floating-elements::before,
        .floating-elements::after {
            content: '';
            position: absolute;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(40, 40, 40, 0.1) 0%, transparent 70%);
            animation: float 20s infinite linear;
        }

        .floating-elements::before {
            width: 300px;
            height: 300px;
            top: 20%;
            right: -150px;
            animation-delay: -10s;
        }

        .floating-elements::after {
            width: 200px;
            height: 200px;
            bottom: 20%;
            left: -100px;
            animation-delay: -5s;
        }

        @keyframes float {
            0% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
            100% { transform: translateY(0px) rotate(360deg); }
        }

        @media (max-width: 600px) {
            .container {
                padding: 32px 24px;
                margin: 16px;
                border-radius: 20px;
            }
            
            h1 {
                font-size: 1.75rem;
                margin-bottom: 36px;
            }
            
            .input-group {
                margin-bottom: 24px;
            }
        }

        /* Smooth scrollbar */
        ::-webkit-scrollbar {
            width: 4px;
        }

        ::-webkit-scrollbar-track {
            background: #0a0a0a;
        }

        ::-webkit-scrollbar-thumb {
            background: #333;
            border-radius: 2px;
        }
    </style>
</head>
<body>
    <div class="floating-elements"></div>
    
    <div class="container">
        <h1>Link Generator</h1>
        
        <div class="input-group">
            <label for="link1">Link 1</label>
            <input type="text" id="link1" placeholder="https://..." autocomplete="off" spellcheck="false">
        </div>

        <div class="input-group">
            <label for="link2">Link 2</label>
            <input type="text" id="link2" placeholder="https://..." autocomplete="off" spellcheck="false">
        </div>

        <button class="generate-btn" onclick="generateLink()">
            Generate
        </button>

        <div id="result" class="result">
            <div class="result-label">Output</div>
            <div class="result-box">
                <div id="result-text" class="result-text"></div>
                <button class="copy-btn" onclick="copyResult()">
                    <span id="copy-icon">⧉</span>
                    <span id="copy-text">Copy</span>
                </button>
            </div>
        </div>
    </div>

    <script>
        function generateLink() {
            const link1 = document.getElementById('link1').value.trim();
            const link2 = document.getElementById('link2').value.trim();
            
            if (!link1 || !link2) {
                // Subtle error indication
                const emptyFields = [];
                if (!link1) emptyFields.push(document.getElementById('link1'));
                if (!link2) emptyFields.push(document.getElementById('link2'));
                
                emptyFields.forEach(field => {
                    field.style.borderColor = 'rgba(239, 68, 68, 0.5)';
                    setTimeout(() => {
                        field.style.borderColor = 'rgba(40, 40, 40, 0.6)';
                    }, 2000);
                });
                return;
            }
            
            const result = `<${link1}>[s](${link2})`;
            
            document.getElementById('result-text').textContent = result;
            document.getElementById('result').classList.add('show');
        }

        function copyResult() {
            const text = document.getElementById('result-text').textContent;
            const copyBtn = document.querySelector('.copy-btn');
            const copyIcon = document.getElementById('copy-icon');
            const copyText = document.getElementById('copy-text');
            
            navigator.clipboard.writeText(text).then(() => {
                copyBtn.classList.add('copied');
                copyIcon.textContent = '✓';
                copyText.textContent = 'Copied';
                
                setTimeout(() => {
                    copyBtn.classList.remove('copied');
                    copyIcon.textContent = '⧉';
                    copyText.textContent = 'Copy';
                }, 2000);
            }).catch(() => {
                // Fallback for older browsers
                const textArea = document.createElement('textarea');
                textArea.value = text;
                document.body.appendChild(textArea);
                textArea.select();
                document.execCommand('copy');
                document.body.removeChild(textArea);
                
                copyBtn.classList.add('copied');
                copyIcon.textContent = '✓';
                copyText.textContent = 'Copied';
                
                setTimeout(() => {
                    copyBtn.classList.remove('copied');
                    copyIcon.textContent = '⧉';
                    copyText.textContent = 'Copy';
                }, 2000);
            });
        }

        // Keyboard shortcuts
        document.addEventListener('keydown', function(e) {
            if (e.key === 'Enter' && (e.ctrlKey || e.metaKey)) {
                generateLink();
            }
            if (e.key === 'Escape') {
                document.getElementById('result').classList.remove('show');
            }
        });

        // Auto-focus first input
        document.getElementById('link1').focus();

        // Smooth input transitions
        document.querySelectorAll('input').forEach(input => {
            input.addEventListener('keyup', function() {
                if (this.value) {
                    this.style.borderColor = 'rgba(60, 60, 60, 0.8)';
                } else {
                    this.style.borderColor = 'rgba(40, 40, 40, 0.6)';
                }
            });
        });
    </script>
</body>
</html>
