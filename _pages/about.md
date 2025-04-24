---
permalink: /
title: "Academic Pages is a ready-to-fork GitHub Pages template for academic personal websites"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

<!DOCTYPE html>
<html>

<head>
    <title>诗匣寻句</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            font-family: Arial, sans-serif;
        }

        #gameContainer {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin: 20px;
            width: 700px;
        }

        .word {
            padding: 15px;
            border: 2px solid #4CAF50;
            border-radius: 5px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }

        #answerBox {
            min-height: 60px;
            border: 2px dashed #2196F3;
            padding: 10px;
            margin: 20px;
            width: 700px;
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            align-items: center;
            font-size: 1.2em;
        }

        .complete-verse {
            color: #4CAF50;
            font-weight: bold;
            width: 100%;
            text-align: center;
            animation: fadeIn 1s;
        }

        .correct {
            background-color: #e8f5e9 !important;
        }

        @keyframes shake {
            0% {
                background-color: red;
            }

            100% {
                background-color: white;
            }
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
            }

            to {
                opacity: 1;
            }
        }

        .wrong {
            animation: shake 0.5s;
        }

        #head {
            font-family: cursive;
        }

        #message {
            font-size: 1.2em;
            font-family: cursive;
            color: #2196F3;
            margin: 10px;
            /* opacity: 0; */
        }

        #restartButton {
            margin: 20px auto;
            padding: 10px 30px;
            background-color: #2196F3;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1.1em;
            transition: background-color 0.3s;
            display: none;
        }

        #restartButton:hover {
            background-color: #1976D2;
        }
    </style>
</head>

<body>
    <h2 id="head">小明不小心打翻了诗匣，散落的文字里藏着诗句，请你帮他找一找！</h2>
    <div id="message">请按正确顺序点击诗句文字</div>
    <div id="gameContainer"></div>
    <div id="answerBox"></div>
    <button id="restartButton">再来一次</button>

    <script>
        const verses = [
            "明月几时有，把酒问青天",
            "不知天上宫阙，今夕是何年",
            "我欲乘风归去，又恐琼楼玉宇，高处不胜寒",
            "起舞弄清影，何似在人间",
            "转朱阁，低绮户，照无眠",
            "不应有恨，何事长向别时圆",
            "人有悲欢离合，月有阴晴圆缺，此事古难全",
            "但愿人长久，千里共婵娟"
        ];

        const commonWords = "的一是在不了有和人这中大为上个国我以要他时来用们生到作地于出就分对成会可主发年动同工也能下过子说产种面而方后多定行学法所民得经十三之进着等部度家电力里如水化高自二理起小物现实加量都两体制机当使点从业本去把性好应开它合还因由其些然前外天政四日那社义事平形相全表间样与关各重新线内数正心反你明看原又么利比或但质气第向道命此变条只没结解问意建月公无系军很情者最立代想已通并提直题党程展五果料象员革位入常文总次品式活设及管特件长求老头基资边流路级少图山统接知较将组见计别她手角期根论运农指几九区强放决西被干做必战先回则任取据处队南给色光门即保治北造百规热领七海口东导器压志世金增争济阶油思术极交受联什认六共权收证改清己美再采转更单风切打白教速花带安场身车例真务具万每目至达走积示议声报斗完类八离华名确才科张信马节话米整空元况今集温传土许步群广石记需段研界拉林律叫且究观越织装影算低持音众书布复容儿须际商非验连断深难近矿千周委素技备半办青省列习响约支般史感劳便团往酸历市克何除消构府称太准精值号率族维划选标写存候亲毛快效斯院查江型眼王按格养易置派层片始却专状育厂京识适属圆包火住调满县局照参红细引听该铁价严";

        let currentStep = 0;
        let originalVerse = "";
        let cleanVerse = "";
        const answerBox = document.getElementById('answerBox');
        const gameContainer = document.getElementById('gameContainer');
        const restartButton = document.getElementById('restartButton');

        function getRandomVerse() {
            originalVerse = verses[Math.floor(Math.random() * verses.length)];
            cleanVerse = originalVerse.replace(/[，。、]/g, '');
            return cleanVerse.split('');
        }

        function generateGameWords() {
            const correctVerse = getRandomVerse();
            const gameWords = [...correctVerse];

            while (gameWords.length < 20) {
                const randomWord = commonWords[Math.floor(Math.random() * commonWords.length)];
                if (!gameWords.includes(randomWord)) gameWords.push(randomWord);
            }

            return gameWords.sort(() => Math.random() - 0.5);
        }

        function initGame() {
            currentStep = 0;
            answerBox.innerHTML = '';
            restartButton.style.display = 'none';
            const words = generateGameWords();
            gameContainer.innerHTML = '';
            words.forEach(word => {
                const div = document.createElement('div');
                div.className = 'word';
                div.textContent = word;
                div.onclick = handleClick;
                gameContainer.appendChild(div);
            });
        }

        function handleClick(e) {
            const selectedWord = e.target.textContent;
            const correctVerse = cleanVerse.split('');

            if (selectedWord === correctVerse[currentStep]) {
                e.target.style.background = "#C8E6C9";
                e.target.style.cursor = "default";
                e.target.onclick = null;

                const answerWord = document.createElement('div');
                answerWord.className = 'word correct';
                answerWord.textContent = selectedWord;
                answerBox.appendChild(answerWord);

                currentStep++;

                if (currentStep === correctVerse.length) {
                    new Audio('data:audio/wav;base64,UklGRl9vT19XQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YU').play();
                    setTimeout(() => {
                        answerBox.innerHTML = `<div class="complete-verse">${originalVerse}</div>`;
                        restartButton.style.display = 'block';
                    }, 500);
                }
            } else {
                e.target.classList.add('wrong');
                setTimeout(() => e.target.classList.remove('wrong'), 500);
            }
        }

        restartButton.onclick = () => {
            initGame();
            // 重置所有文字状态
            document.querySelectorAll('.word').forEach(word => {
                word.style.background = "";
                word.style.cursor = "pointer";
                word.onclick = handleClick;
            });
        };

        initGame();
    </script>
</body>

</html>
