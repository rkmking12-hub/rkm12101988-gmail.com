# gujjos-dev-code.
<!DOCTYPE html>
<html lang="gu">
<head>
    <meta charset="UTF-8">
    <title>GujjOS Master AI Engine</title>
    <style>
        body { font-family: 'Arial', sans-serif; background: #eef2f3; display: flex; justify-content: center; padding: 20px; }
        .app-card { background: white; width: 100%; max-width: 600px; padding: 30px; border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); text-align: center; }
        .hero-img { font-size: 80px; margin: 10px 0; transition: transform 0.3s; }
        .hero-img:hover { transform: scale(1.1); }
        .input-group { display: flex; gap: 10px; margin: 20px 0; }
        input, select { padding: 12px; border: 2px solid #ddd; border-radius: 10px; flex: 1; }
        button { padding: 12px 20px; background: #c0392b; color: white; border: none; border-radius: 10px; cursor: pointer; font-weight: bold; }
        .result-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 20px; }
        .card { background: #f9f9f9; padding: 15px; border-radius: 12px; border-bottom: 4px solid #c0392b; }
        .label { font-size: 12px; color: #888; text-transform: uppercase; }
        .val { font-size: 18px; font-weight: bold; color: #2c3e50; }
    </style>
</head>
<body>

<div class="app-card">
    <div id="character-emoji" class="hero-img">🤖</div>
    <h2 id="ai-greeting">GujjOS AI Engine</h2>
    <p id="instruction" style="color: #666;">નમસ્તે! હું તમારો ગાઈડ છું. 4 Number Magic સાથે શીખવા માટે તૈયાર?</p>

    <div class="input-group">
        <input type="text" id="userInput" placeholder="શબ્દ અથવા Element લખો...">
        <select id="langSelect">
            <option value="gu-IN">Gujarati</option>
            <option value="ja-JP">Japanese</option>
            <option value="zh-CN">Chinese</option>
            <option value="hi-IN">Hindi</option>
        </select>
        <button onclick="processRequest()">Execute</button>
    </div>

    <div class="result-grid">
        <div class="card"><div class="label">Translation</div><div id="out-trans" class="val">-</div></div>
        <div class="card"><div class="label">Logic / Element</div><div id="out-logic" class="val">-</div></div>
        <div class="card"><div class="label">Mascot Tip</div><div id="out-tip" class="val">-</div></div>
        <div class="card"><div class="label">AI Voice Status</div><div id="out-voice" class="val">Ready 🟢</div></div>
    </div>
</div>

<script>
    const elementsData = {
        "hydrogen": { atomic: 1, tip: "સૌથી હલકું તત્વ!" },
        "beryllium": { atomic: 4, tip: "યાદ રાખજો, આ આપણું 'Magic 4' એલિમેન્ટ છે!" }
    };

    async function processRequest() {
        const input = document.getElementById('userInput').value.toLowerCase();
        const lang = document.getElementById('langSelect').value;
        const targetLang = lang.split('-')[0];

        // 1. Translation AI
        const res = await fetch(`https://translate.googleapis.com/translate_a/single?client=gtx&sl=en&tl=${targetLang}&dt=t&q=${input}`);
        const data = await res.json();
        const translated = data[0][0][0];

        // 2. Element Check
        let logicInfo = "Coding Word";
        let tip = "Jarvis says: Keep Coding!";
        if(elementsData[input]) {
            logicInfo = "Atomic No: " + elementsData[input].atomic;
            tip = elementsData[input].tip;
            document.getElementById('character-emoji').innerText = "🕷️";
        }

        // 3. Update UI
        document.getElementById('out-trans').innerText = translated;
        document.getElementById('out-logic').innerText = logicInfo;
        document.getElementById('out-tip').innerText = tip;

        // 4. Voice Output
        speak(translated, lang);
    }

    function speak(text, lang) {
        const utterance = new SpeechSynthesisUtterance(text);
        utterance.lang = lang;
        window.speechSynthesis.speak(utterance);
    }
</script>
</body>
</html>
