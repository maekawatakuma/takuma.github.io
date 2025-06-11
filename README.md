# takuma.github.io
AIで作成した制作物の入れ先
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ウイスキー歴史クイズゲーム</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(to right, #4a2a11, #8b4513); /* ウイスキーをイメージしたグラデーション */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .quiz-container {
            background-color: #fff;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
            padding: 30px;
            max-width: 600px;
            width: 100%;
            text-align: center;
            animation: fadeIn 0.8s ease-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        h1 {
            color: #4a2a11;
            font-size: 2.5rem;
            margin-bottom: 25px;
            font-weight: 700;
        }
        .question-text {
            font-size: 1.5rem;
            color: #333;
            margin-bottom: 30px;
            min-height: 80px; /* 質問文の高さ確保 */
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .options-container button {
            display: block;
            width: 100%;
            padding: 15px 20px;
            margin-bottom: 15px;
            border: 2px solid #d4a762; /* 樽の色をイメージ */
            border-radius: 10px;
            background-color: #fff;
            color: #4a2a11;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        .options-container button:hover:not(:disabled) {
            background-color: #f5f5f5;
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        .options-container button:disabled {
            cursor: not-allowed;
            opacity: 0.7;
        }
        .options-container button.correct {
            background-color: #a8e6cf; /* 正解色 */
            border-color: #6ee69e;
            color: #1a5e3b;
        }
        .options-container button.incorrect {
            background-color: #ffadad; /* 不正解色 */
            border-color: #ff6b6b;
            color: #8c1c1c;
        }
        .navigation-buttons button {
            padding: 12px 25px;
            font-size: 1.1rem;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            margin: 0 10px;
        }
        .navigation-buttons button#next-button {
            background: linear-gradient(to right, #d4a762, #e8c48a); /* 樽のグラデーション */
            color: #4a2a11;
        }
        .navigation-buttons button#next-button:hover:not(:disabled) {
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }
        .navigation-buttons button#next-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        .navigation-buttons button#restart-button {
            background-color: #4a2a11; /* 濃いウイスキー色 */
            color: #fff;
        }
        .navigation-buttons button#restart-button:hover {
            background-color: #6a3c1e;
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
        }
        .score-display {
            font-size: 1.2rem;
            color: #4a2a11;
            margin-top: 20px;
            font-weight: 600;
        }
        .question-counter {
            font-size: 1rem;
            color: #666;
            margin-bottom: 10px;
        }
        .feedback-message {
            margin-top: 15px;
            font-weight: bold;
            font-size: 1.2rem;
            min-height: 2.5em; /* フィードバックメッセージの高さ確保 */
        }
        .feedback-message.correct {
            color: #28a745; /* Green */
        }
        .feedback-message.incorrect {
            color: #dc3545; /* Red */
        }
        .explanation-text {
            margin-top: 15px;
            font-size: 1rem;
            color: #555;
            line-height: 1.5;
        }

        /* モバイル対応 */
        @media (max-width: 768px) {
            h1 {
                font-size: 2rem;
            }
            .question-text {
                font-size: 1.2rem;
                min-height: 60px;
            }
            .options-container button {
                font-size: 1rem;
                padding: 12px 15px;
            }
            .navigation-buttons button {
                font-size: 1rem;
                padding: 10px 20px;
                margin: 0 5px;
            }
            .feedback-message {
                font-size: 1rem;
            }
            .explanation-text {
                font-size: 0.9rem;
            }
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>🥃 ウイスキー歴史クイズ 🥃</h1>
        <div id="quiz-screen">
            <p id="question-counter" class="question-counter">問題 1 / 20</p>
            <p id="question-text" class="question-text">ここに質問文が表示されます。</p>
            <div id="options-container" class="options-container">
                <!-- 選択肢ボタンがここに動的に追加されます -->
            </div>
            <p id="feedback-message" class="feedback-message"></p>
            <p id="explanation-text" class="explanation-text"></p> <!-- 解説文を表示する要素を追加 -->
            <div class="navigation-buttons mt-6">
                <button id="next-button" class="px-6 py-3 rounded-xl bg-orange-400 text-white font-semibold shadow-md transition-all duration-300 transform hover:scale-105" disabled>次の問題</button>
            </div>
        </div>

        <div id="result-screen" class="hidden">
            <h2 class="text-3xl font-bold text-gray-800 mb-4">結果発表！</h2>
            <p id="score-display" class="score-display">あなたのスコア: 0 / 20</p>
            <button id="restart-button" class="px-6 py-3 rounded-xl bg-blue-600 text-white font-semibold shadow-md mt-8 transition-all duration-300 transform hover:scale-105">もう一度挑戦</button>
        </div>
    </div>

    <script>
        // クイズの質問と選択肢、正解の定義
        const questions = [
            {
                question: "ウイスキーの語源となったゲール語「uisce beatha」の意味は？",
                options: ["A) 神の恵み", "B) 生命の水", "C) 黄金の液体", "D) 聖なる酒"],
                correctAnswer: 1, // B) 生命の水
                feedback: "ウイスキーの語源「uisce beatha」は、直訳すると「生命の水」という意味です。これはラテン語の「aqua vitae」に由来しています。"
            },
            {
                question: "ウイスキーの発祥地について、起源説がある2つの地域は？",
                options: ["A) フランスとドイツ", "B) アイルランドとスコットランド", "C) アメリカとカナダ", "D) 日本と中国"],
                correctAnswer: 1, // B) アイルランドとスコットランド
                feedback: "ウイスキーの発祥については、アイルランドとスコットランドの両方に有力な起源説が存在し、どちらが先かは定説がありません。"
            },
            {
                question: "ウイスキーの初期の発展は、中世の何からの蒸留技術によって発展しましたか？",
                options: ["A) 農民の家庭", "B) 商業ギルド", "C) 修道院", "D) 王室の工房"],
                correctAnswer: 2, // C) 修道院
                feedback: "中世において、蒸留技術は修道院で薬用や錬金術の目的で発展しました。これがウイスキー製造の基礎となります。"
            },
            {
                question: "ウイスキーに関する最古の文献記録は、およそ何世紀頃のものですか？",
                options: ["A) 13世紀", "B) 14世紀", "C) 15世紀", "D) 16世紀"],
                correctAnswer: 2, // C) 15世紀
                feedback: "ウイスキーに関する最古の記録は、スコットランドの会計記録に1494年の記述があり、15世紀頃とされています。"
            },
            {
                question: "スコッチウイスキーの主要な5つの産地に含まれないものはどれですか？",
                options: ["A) スペイサイド", "B) ハイランド", "C) ローランド", "D) ウェストミンスター"],
                correctAnswer: 3, // D) ウェストミンスター (架空の選択肢)
                feedback: "スコッチウイスキーの5つの主要産地は、スペイサイド、ハイランド、ローランド、アイラ、キャンベルタウンです。ウェストミンスターは含まれません。"
            },
            {
                question: "スコッチウイスキーの法的規制を確立した「ウイスキー法」が制定されたのは西暦何年ですか？",
                options: ["A) 1888年", "B) 1909年", "C) 1923年", "D) 1945年"],
                correctAnswer: 1, // B) 1909年
                feedback: "1909年のウイスキー法により、スコッチウイスキーの定義が確立され、品質と製法が法的に保護されるようになりました。"
            },
            {
                question: "スコッチウイスキーにおいて、19世紀後半に発展した生産技術は何ですか？",
                options: ["A) ポットスチル蒸留", "B) ブレンデッド技術", "C) 樽のチャーリング", "D) シングルカスク熟成"],
                correctAnswer: 1, // B) ブレンデッド技術
                feedback: "19世紀後半には、異なる蒸留所のウイスキーを混ぜ合わせるブレンデッド技術が発展し、ウイスキーの普及に大きく貢献しました。"
            },
            {
                question: "アイリッシュウイスキーの伝統的な製法の特徴は何回蒸留することですか？",
                options: ["A) 1回", "B) 2回", "C) 3回", "D) 4回"],
                correctAnswer: 2, // C) 3回
                feedback: "アイリッシュウイスキーは、伝統的に3回の蒸留を行うことで、よりなめらかで軽い口当たりが特徴です。"
            },
            {
                question: "アイリッシュウイスキーが20世紀前半に衰退した主な要因は何でしたか？",
                options: ["A) 自然災害", "B) 政治的・経済的要因", "C) 新しいアルコール飲料の台頭", "D) 製造技術の停滞"],
                correctAnswer: 1, // B) 政治的・経済的要因
                feedback: "アイルランドの独立戦争や禁酒法の影響など、20世紀前半の政治的・経済的要因がアイリッシュウイスキーの衰退に繋がりました。"
            },
            {
                question: "バーボンウイスキーの製造条件として、51%以上使用しなければならない穀物は何ですか？",
                options: ["A) ライ麦", "B) 大麦", "C) コーン (トウモロコシ)", "D) 小麦"],
                correctAnswer: 2, // C) コーン (トウモロコシ)
                feedback: "バーボンウイスキーは、その原料の51%以上がコーン（トウモロコシ）でなければならないという法的規制があります。"
            },
            {
                question: "テネシーウイスキーの製造工程に特徴的な、木炭によるろ過工程は何と呼ばれていますか？",
                options: ["A) メンフィスフィルター", "B) リンカーンカウンティプロセス", "C) ナッシュビルスキーム", "D) テネシーチャージング"],
                correctAnswer: 1, // B) リンカーンカウンティプロセス
                feedback: "テネシーウイスキーは、蒸留後にサトウカエデの木炭でろ過する「リンカーンカウンティプロセス」が特徴です。"
            },
            {
                question: "アメリカの禁酒法時代はいつからいつまで続きましたか？",
                options: ["A) 1918-1928年", "B) 1920-1933年", "C) 1922-1935年", "D) 1925-1940年"],
                correctAnswer: 1, // B) 1920-1933年
                feedback: "アメリカの禁酒法は1920年から1933年まで施行され、アルコールの製造、販売、輸送が禁止されました。"
            },
            {
                question: "アメリカの禁酒法時代に合法的なウイスキー生産が停止された結果、横行した密造酒は何と呼ばれましたか？",
                options: ["A) バンドルワイン", "B) ムーンシャイン", "C) シャドウリカー", "D) ゴーストスピリット"],
                correctAnswer: 1, // B) ムーンシャイン
                feedback: "禁酒法時代には、自家製の密造酒である「ムーンシャイン」が闇で製造・流通しました。"
            },
            {
                question: "禁酒法時代に、密輸ルートとして恩恵を受けた国はどこですか？",
                options: ["A) メキシコ", "B) カナダ", "C) キューバ", "D) イギリス"],
                correctAnswer: 1, // B) カナダ
                feedback: "禁酒法時代、カナダはアメリカへの密輸ルートとして重要な役割を果たし、ウイスキー産業が一時的に恩恵を受けました。"
            },
            {
                question: "1831年にイーニアス・コフィーによって発明され、ウイスキーの大量生産を可能にした連続式蒸留器は何と呼ばれていますか？",
                options: ["A) ポットスチル", "B) コラムスチル", "C) マルチスチル", "D) リフラックススチル"],
                correctAnswer: 1, // B) コラムスチル
                feedback: "コラムスチル（連続式蒸留器）は、ポットスチルに比べてはるかに効率的に大量のウイスキーを生産することを可能にしました。"
            },
            {
                question: "伝統的な単式蒸留器の名称は何ですか？",
                options: ["A) フラッシュスチル", "B) コラムスチル", "C) ポットスチル", "D) リトルトスチル"],
                correctAnswer: 2, // C) ポットスチル
                feedback: "ポットスチルは、単式蒸留器とも呼ばれ、伝統的なスコッチウイスキーやアイリッシュウイスキーの製造に用いられます。"
            },
            {
                question: "熟成中に樽から自然に蒸発するウイスキーの量を指す言葉は何ですか？",
                options: ["A) デビルズシェア", "B) エンジェルズシェア", "C) ドワーフズドロップ", "D) フェアリーフロス"],
                correctAnswer: 1, // B) エンジェルズシェア
                feedback: "熟成中に樽から蒸発するウイスキーは「天使の分け前（エンジェルズシェア）」と呼ばれ、ウイスキーの風味を凝縮させます。"
            },
            {
                question: "日本ウイスキーの父として知られる人物は誰ですか？",
                options: ["A) 鳥井信治郎", "B) 竹鶴政孝", "C) 斎藤緑雨", "D) 樋口一葉"],
                correctAnswer: 1, // B) 竹鶴政孝
                feedback: "竹鶴政孝はスコットランドでウイスキー製造を学び、日本に本格的なウイスキー製造をもたらしたことから「日本ウイスキーの父」と呼ばれています。"
            },
            {
                question: "ブレンデッドウイスキーの先駆者として知られる人物は誰ですか？",
                options: ["A) ジョン・ジェイムソン", "B) ジョニー・ウォーカー", "C) アンドリュー・アッシャー", "D) ジェームズ・ブキャナン"],
                correctAnswer: 1, // B) ジョニー・ウォーカー
                feedback: "ジョニー・ウォーカーは、多様なモルトウイスキーとグレーンウイスキーをブレンドすることで、今日のブレンデッドウイスキーの基礎を築きました。"
            },
            {
                question: "世界最古の免許蒸留所として1608年に設立された蒸留所は何ですか？",
                options: ["A) オールド・ブッシュミルズ", "B) グレンリベット", "C) ジェイムソン", "D) ボウモア"],
                correctAnswer: 0, // A) オールド・ブッシュミルズ
                feedback: "アイルランドにあるオールド・ブッシュミルズ蒸留所は、1608年にジェームズ1世から免許を与えられ、世界最古の免許蒸留所として知られています。"
            }
        ];

        // グローバル変数
        let shuffledQuestions = [];
        let currentQuestionIndex = 0;
        let score = 0;
        let answered = false; // 質問に回答済みかどうかのフラグ

        // DOM要素の取得
        const quizScreen = document.getElementById('quiz-screen');
        const resultScreen = document.getElementById('result-screen');
        const questionCounterElement = document.getElementById('question-counter');
        const questionTextElement = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const nextButton = document.getElementById('next-button');
        const restartButton = document.getElementById('restart-button');
        const scoreDisplay = document.getElementById('score-display');
        const feedbackMessage = document.getElementById('feedback-message');
        const explanationTextElement = document.getElementById('explanation-text'); // 新しく追加

        // クイズを開始する関数
        function startQuiz() {
            // 質問をシャッフル
            shuffledQuestions = [...questions].sort(() => Math.random() - 0.5);
            currentQuestionIndex = 0;
            score = 0;
            answered = false;
            quizScreen.classList.remove('hidden');
            resultScreen.classList.add('hidden');
            feedbackMessage.textContent = '';
            explanationTextElement.textContent = ''; // 解説文をクリア
            nextButton.disabled = true; // 最初は「次の問題」ボタンを無効化
            displayQuestion();
        }

        // 現在の質問を表示する関数
        function displayQuestion() {
            const question = shuffledQuestions[currentQuestionIndex];
            questionCounterElement.textContent = `問題 ${currentQuestionIndex + 1} / ${shuffledQuestions.length}`;
            questionTextElement.textContent = question.question;
            optionsContainer.innerHTML = ''; // 選択肢をクリア
            feedbackMessage.textContent = ''; // フィードバックメッセージをクリア
            explanationTextElement.textContent = ''; // 解説文をクリア

            question.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.textContent = option;
                button.dataset.index = index; // データ属性に選択肢のインデックスを保存
                button.classList.add('px-4', 'py-2', 'rounded-lg', 'focus:outline-none', 'transition', 'duration-200');
                button.onclick = () => selectAnswer(index); // クリックイベントを設定
                optionsContainer.appendChild(button);
            });
            answered = false; // 新しい質問がロードされたので、回答済みフラグをリセット
            nextButton.disabled = true; // 次の質問がロードされたら、「次の問題」ボタンを再度無効化
        }

        // 回答を選択した際の処理
        function selectAnswer(selectedIndex) {
            if (answered) return; // すでに回答済みの場合は何もしない

            answered = true; // 回答済みフラグを立てる
            const question = shuffledQuestions[currentQuestionIndex];
            const correctIndex = question.correctAnswer;
            const optionButtons = optionsContainer.querySelectorAll('button');

            optionButtons.forEach((button, index) => {
                button.disabled = true; // 全ての選択肢を無効化

                if (index === correctIndex) {
                    button.classList.add('correct'); // 正解の選択肢に色を付ける
                } else if (index === selectedIndex) {
                    button.classList.add('incorrect'); // 不正解の選択肢に色を付ける
                }
            });

            if (selectedIndex === correctIndex) {
                score++;
                feedbackMessage.textContent = '✅ 正解！';
                feedbackMessage.classList.remove('incorrect');
                feedbackMessage.classList.add('correct');
            } else {
                feedbackMessage.textContent = '❌ 不正解...';
                feedbackMessage.classList.remove('correct');
                feedbackMessage.classList.add('incorrect');
            }
            // 解説文を表示
            explanationTextElement.textContent = question.feedback;
            nextButton.disabled = false; // 回答したら「次の問題」ボタンを有効化
        }

        // 次の質問へ進む、または結果を表示する関数
        function nextQuestion() {
            currentQuestionIndex++;
            if (currentQuestionIndex < shuffledQuestions.length) {
                displayQuestion();
            } else {
                showResult();
            }
        }

        // 結果を表示する関数
        function showResult() {
            quizScreen.classList.add('hidden');
            resultScreen.classList.remove('hidden');
            scoreDisplay.textContent = `あなたのスコア: ${score} / ${shuffledQuestions.length}`;
        }

        // イベントリスナーの設定
        nextButton.addEventListener('click', nextQuestion);
        restartButton.addEventListener('click', startQuiz);

        // ページロード時にクイズを開始
        window.onload = startQuiz;
    </script>
</body>
</html>
