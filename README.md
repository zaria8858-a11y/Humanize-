<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text Paraphraser</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
               background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
               min-height: 100vh; padding: 20px; }
        .container { max-width: 900px; margin: 0 auto; background: white; 
                     border-radius: 20px; box-shadow: 0 20px 40px rgba(0,0,0,0.1); 
                     overflow: hidden; }
        .header { background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); 
                  color: white; padding: 30px; text-align: center; }
        .header h1 { font-size: 2.5em; margin-bottom: 10px; }
        .content { padding: 40px; }
        textarea { width: 100%; height: 150px; padding: 15px; border: 2px solid #e1e5e9; 
                   border-radius: 10px; font-size: 16px; resize: vertical; 
                   transition: border-color 0.3s; }
        textarea:focus { outline: none; border-color: #4facfe; }
        .btn { background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%); 
               color: white; border: none; padding: 15px 30px; border-radius: 50px; 
               font-size: 16px; cursor: pointer; transition: transform 0.2s; 
               margin: 20px 10px 0 0; }
        .btn:hover { transform: translateY(-2px); }
        .btn:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }
        .output { margin-top: 30px; padding: 20px; background: #f8f9fa; 
                  border-radius: 10px; border-left: 5px solid #4facfe; 
                  white-space: pre-wrap; min-height: 150px; }
        .stats { display: flex; justify-content: space-between; margin-top: 20px; 
                 font-size: 14px; color: #666; }
        .tip { background: #e8f5e8; padding: 15px; border-radius: 10px; 
               margin-top: 20px; border-left: 5px solid #28a745; }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>✨ Text Paraphraser</h1>
            <p>Rewrite your text naturally and clearly</p>
        </div>
        <div class="content">
            <textarea id="input" placeholder="Paste your text here..."></textarea>
            <br>
            <button class="btn" onclick="paraphrase()">Paraphrase Text</button>
            <button class="btn" onclick="clearAll()">Clear</button>
            
            <div class="output" id="output"></div>
            
            <div class="stats">
                <span id="inputCount">0 / 5000 characters</span>
                <span id="outputCount">-</span>
            </div>
            
            <div class="tip">
                <strong>💡 Tip:</strong> This tool helps rephrase text for clarity. 
                Always review and edit the output to ensure it matches your voice!
            </div>
        </div>
    </div>

    <script>
        const input = document.getElementById('input');
        const output = document.getElementById('output');
        const inputCount = document.getElementById('inputCount');
        const outputCount = document.getElementById('outputCount');

        // Simple synonym replacement and restructuring
        const synonyms = {
            'very': ['quite', 'really', 'truly', 'extremely'],
            'good': ['excellent', 'great', 'solid', 'fine'],
            'important': ['crucial', 'key', 'vital', 'essential'],
            'show': ['demonstrate', 'reveal', 'display', 'illustrate'],
            'make': ['create', 'build', 'produce', 'form'],
            'get': ['obtain', 'acquire', 'receive', 'gain'],
            'use': ['employ', 'utilize', 'apply', 'leverage']
        };

        input.addEventListener('input', updateCount);
        output.addEventListener('input', () => outputCount.textContent = `${output.textContent.length} chars`);

        function updateCount() {
            inputCount.textContent = `${input.value.length} / 5000 characters`;
        }

        function paraphrase() {
            const text = input.value.trim();
            if (!text) {
                alert('Please enter some text first!');
                return;
            }

            const btn = event.target;
            btn.disabled = true;
            btn.textContent = 'Processing...';

            setTimeout(() => {
                let result = rephraseText(text);
                output.textContent = result;
                outputCount.textContent = `${result.length} chars`;
                btn.disabled = false;
                btn.textContent = 'Paraphrase Text';
            }, 800);
        }

        function rephraseText(text) {
            return text
                // Split into sentences
                .split(/[.?!]\s+/)
                .map(sentence => rephraseSentence(sentence.trim()))
                .filter(s => s)
                .join('. ')
                .replace(/\. \./g, '. ') + '.';
        }

        function rephraseSentence(sentence) {
            // Basic restructuring patterns
            let result = sentence;
            
            // Replace common words with synonyms randomly
            Object.keys(synonyms).forEach(word => {
                const regex = new RegExp(`\\b${word}\\b`, 'gi');
                if (regex.test(result)) {
                    const syn = synonyms[word][Math.floor(Math.random() * synonyms[word].length)];
                    result = result.replace(regex, syn);
                }
            });

            // Simple restructuring
            result = result
                .replace(/the (\w+)/g, (m, g1) => `${g1} the`)
                .replace(/is (\w+)/g, (m, g1) => `appears ${g1}`)
                .replace(/are (\w+)/g, (m, g1) => `seem ${g1}`);

            // Add variety in sentence starters
            const starters = ['Indeed', 'Actually', 'Notably', 'Clearly', ''];
            if (Math.random() > 0.7) {
                result = `${starters[Math.floor(Math.random() * starters.length)]}, ${result}`;
            }

            return result.charAt(0).toUpperCase() + result.slice(1);
        }

        function clearAll() {
            input.value = '';
            output.textContent = '';
            updateCount();
            outputCount.textContent = '-';
        }

        updateCount();
    </script>
</body>
</html>
