<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>介護の基本 - 過去問演習</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <!-- Google Fonts Noto Sans JP -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700;900&display=swap" rel="stylesheet">

    <style>
        body {
            font-family: 'Noto Sans JP', sans-serif;
            background-color: #f1f5f9;
            color: #1e293b;
            min-height: 100vh;
        }

        /* ルビ（フリガナ）の共通装飾設定 */
        ruby {
            ruby-align: center;
        }
        rt {
            font-size: 0.55em;
            font-weight: 700;
            color: #b91c1c; /* 視認性の高い濃い赤色 */
            line-height: 1.1;
        }

        /* カードの立体感と影 */
        .card-shadow {
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.05), 0 8px 10px -6px rgba(0, 0, 0, 0.05);
        }

        /* 選択肢ボタンのインタラクティブ効果 */
        .option-btn {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
            -webkit-tap-highlight-color: transparent;
        }
        .option-btn:active {
            transform: scale(0.98);
        }

        /* 正解・不正解選択時のスタイル */
        .correct-selected {
            background-color: #ecfdf5 !important;
            border-color: #10b981 !important;
            color: #065f46 !important;
        }
        .wrong-selected {
            background-color: #fef2f2 !important;
            border-color: #ef4444 !important;
            color: #991b1b !important;
        }
    </style>
</head>
<body class="bg-slate-100 flex flex-col items-center min-h-screen p-3 md:p-6">

    <!-- ヘッダー（戻るボタン ＆ 単元タイトル） -->
    <header class="w-full max-w-md md:max-w-2xl bg-white rounded-2xl p-4 mb-4 card-shadow border border-slate-200 flex items-center justify-between">
        <a href="index.html" class="flex items-center gap-1 text-xs font-bold text-slate-500 hover:text-emerald-600 bg-slate-100 px-3 py-1.5 rounded-lg border border-slate-200 transition">
            <i class="fa-solid fa-chevron-left"></i>
            <span><ruby>一覧<rt>いちらん</rt></ruby>へ</span>
        </a>
        <div class="flex items-center gap-2">
            <span class="bg-emerald-600 text-white font-bold text-xs px-2.5 py-1 rounded-full">
                <ruby>介護<rt>かいご</rt></ruby>の<ruby>基本<rt>きほん</rt></ruby>
            </span>
        </div>
        <div id="progressBadge" class="text-xs text-slate-500 font-bold bg-emerald-50 text-emerald-700 px-2.5 py-1 rounded-lg border border-emerald-200">
            1 / 3 問
        </div>
    </header>

    <!-- メインコンテンツ（問題演習カード） -->
    <main id="quizCard" class="w-full max-w-md md:max-w-2xl bg-white rounded-3xl p-5 md:p-7 card-shadow border-2 border-emerald-400 flex-1 flex flex-col justify-between mb-4">
        <div>
            <!-- 問題タイトルバッジ -->
            <div class="flex items-center justify-between mb-3">
                <span id="questionNumBadge" class="bg-emerald-100 text-emerald-800 text-xs font-bold px-3 py-1 rounded-full flex items-center gap-1.5">
                    <i class="fa-solid fa-circle-question text-emerald-600"></i>
                    <span><ruby>問題<rt>もんだい</rt></ruby> 1</span>
                </span>
                <span id="questionCountText" class="text-xs text-slate-400 font-medium">3問中 1問目</span>
            </div>

            <!-- 問題文エリア -->
            <div id="questionText" class="bg-slate-50 p-4 rounded-2xl border border-slate-200 text-sm md:text-base leading-relaxed text-slate-800 font-medium mb-5">
                <!-- JavaScriptで注入 -->
            </div>

            <!-- 選択肢ボタンリスト -->
            <div id="optionsContainer" class="space-y-2.5 text-xs md:text-sm font-medium">
                <!-- JavaScriptで注入 -->
            </div>
        </div>

        <!-- 解説＆次の問題エリア -->
        <div class="mt-6 pt-4 border-t border-slate-200">
            <!-- 解説表示ボタン -->
            <button id="checkBtn" onclick="toggleAnswer()" class="w-full bg-slate-700 hover:bg-slate-800 text-white font-bold py-3 rounded-2xl text-xs md:text-sm flex items-center justify-center gap-2 transition shadow-md active:scale-95 mb-3">
                <i class="fa-solid fa-lightbulb text-amber-300"></i>
                <span><ruby>正解<rt>せいかい</rt></ruby>とワンポイント<ruby>解説<rt>かいせつ</rt></ruby>を<ruby>確認<rt>かくにん</rt></ruby>する</span>
            </button>

            <!-- 解説ボックス（初期状態は非表示） -->
            <div id="answerBox" class="hidden p-4 bg-emerald-50 border-2 border-emerald-300 rounded-2xl text-xs md:text-sm text-slate-800 mb-3">
                <div id="answerTitle" class="font-bold text-emerald-900 mb-1.5 flex items-center gap-1.5 text-sm md:text-base">
                    <!-- JavaScriptで正解を表示 -->
                </div>
                <p id="answerDescription" class="leading-relaxed text-slate-700 text-xs md:text-sm">
                    <!-- JavaScriptで解説を表示 -->
                </p>
            </div>

            <!-- 次の問題に進むボタン -->
            <button id="nextBtn" onclick="nextQuestion()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3.5 rounded-2xl text-xs md:text-sm flex items-center justify-center gap-2 transition shadow-md active:scale-95">
                <span><ruby>次<rt>つぎ</rt></ruby>の<ruby>問題<rt>もんだい</rt></ruby>へ</span>
                <i class="fa-solid fa-arrow-right"></i>
            </button>
        </div>
    </main>

    <!-- 結果発表画面（初期状態は非表示） -->
    <div id="resultCard" class="hidden w-full max-w-md md:max-w-2xl bg-white rounded-3xl p-6 md:p-8 card-shadow border-2 border-emerald-400 text-center my-auto">
        <div class="w-16 h-16 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
            <i class="fa-solid fa-trophy"></i>
        </div>
        <h2 class="text-xl md:text-2xl font-black text-slate-800 mb-2">
            おつかれさまでした！
        </h2>
        <p class="text-xs md:text-sm text-slate-500 mb-6">
            「<ruby>介護<rt>かいご</rt></ruby>の<ruby>基本<rt>きほん</rt></ruby>」の演習問題が完了しました。
        </p>

        <!-- スコア表示 -->
        <div class="bg-slate-50 border-2 border-slate-200 rounded-2xl p-5 mb-6">
            <div class="text-xs text-slate-400 font-bold mb-1"><ruby>正解数<rt>せいかいすう</rt></ruby></div>
            <div class="text-3xl md:text-4xl font-black text-emerald-600">
                <span id="scoreText">0</span> / <span id="totalScoreText">3</span> <span class="text-lg text-slate-600 font-bold"><ruby>問正解<rt>もんせいかい</rt></ruby></span>
            </div>
        </div>

        <!-- アクションボタン -->
        <div class="space-y-3">
            <button onclick="restartQuiz()" class="w-full bg-emerald-600 hover:bg-emerald-700 text-white font-bold py-3.5 rounded-2xl text-xs md:text-sm flex items-center justify-center gap-2 transition shadow-md active:scale-95">
                <i class="fa-solid fa-rotate-right"></i>
                <span>もう<ruby>一度<rt>いちど</rt></ruby><ruby>挑戦<rt>ちょうせん</rt></ruby>する</span>
            </button>
            <a href="index.html" class="block w-full bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold py-3 rounded-2xl text-xs md:text-sm border border-slate-300 transition">
                <ruby>単元一覧<rt>たんげんいちらん</rt></ruby>に<ruby>戻<rt>もど</rt></ruby>る
            </a>
        </div>
    </div>

    <!-- フッター -->
    <footer class="w-full text-center text-slate-400 text-[11px] py-2 mt-auto">
        介護福祉士国家試験対策 学習アプリ
    </footer>

    <script>
        /*
         * 問題データ（配列表式）
         * この中に何問入っていても、プログラムが自動計算して順繰りに表示します。
         */
        const questions = [
            {
                id: 1,
                question: '<ruby>ICF<rt>アイシーエフ</rt></ruby>（<ruby>国際<rt>こくさい</rt></ruby><ruby>生活<rt>せいかつ</rt></ruby><ruby>機能<rt>きのう</rt></ruby><ruby>分類<rt>ぶんるい</rt></ruby>）の「<ruby>環境<rt>かんきょう</rt></ruby><ruby>要因<rt>よういん</rt></ruby>」に<ruby>該当<rt>がいとう</rt></ruby>するものとして、<ruby>最も<rt>もっとも</rt></ruby><ruby>適切<rt>てきせつ</rt></ruby>なものを1つ<ruby>選<rt>えら</rt></ruby>びなさい。',
                choices: [
                    '<ruby>本人<rt>ほんじん</rt></ruby>の<ruby>趣味<rt>しゅみ</rt></ruby>や<ruby>価値観<rt>かちかん</rt></ruby>',
                    '<ruby>脳梗塞<rt>のうこうそく</rt></ruby>による<ruby>麻痺<rt>まひ</rt></ruby>の<ruby>症状<rt>しょうじょう</rt></ruby>',
                    '<ruby>自宅<rt>じたく</rt></ruby>の<ruby>玄関<rt>げんかん</rt></ruby>に<ruby>設置<rt>せっち</rt></ruby>された<ruby>手<rt>て</rt></ruby>すり',
                    '<ruby>着替え<rt>きがえ</rt></ruby>や<ruby>入浴<rt>にゅうよく</rt></ruby>を<ruby>自力<rt>じりょく</rt></ruby>でおこなうこと',
                    '<ruby>地域<rt>ちいき</rt></ruby>のボランティア<ruby>活動<rt>かつどう</rt></ruby>に<ruby>参加<rt>さんか</rt></ruby>すること'
                ],
                correct: 3,
                explanation: '「手すり」「車椅子」「家族」「福祉用具」などは<strong><ruby>環境<rt>かんきょう</rt></ruby><ruby>要因<rt>よういん</rt></ruby></strong>です。<br>1は個人要因、2は心身機能、4は活動、5は参加に分類されます。'
            },
            {
                id: 2,
                question: '<ruby>介護<rt>かいご</rt></ruby>における「<ruby>自立<rt>じりつ</rt></ruby><ruby>支援<rt>しえん</rt></ruby>」の<ruby>考え方<rt>かんがえかた</rt></ruby>として、<ruby>最も<rt>もっとも</rt></ruby><ruby>適切<rt>てきせつ</rt></ruby>なものを1つ<ruby>選<rt>えら</rt></ruby>びなさい。',
                choices: [
                    '<ruby>利用者<rt>りようしゃ</rt></ruby>ができないことも含めて、すべての<ruby>介護<rt>かいご</rt></ruby>を<ruby>職員<rt>しょくいん</rt></ruby>が行う。',
                    '<ruby>利用者<rt>りようしゃ</rt></ruby>が<ruby>自分<rt>じぶん</rt></ruby>の<ruby>意思<rt>いし</rt></ruby>で<ruby>決定<rt>けってい</rt></ruby>し、<ruby>自分<rt>じぶん</rt></ruby>らしい<ruby>生活<rt>せいかつ</rt></ruby>を送れるよう<ruby>支援<rt>しえん</rt></ruby>する。',
                    '<ruby>家族<rt>かぞく</rt></ruby>の<ruby>意向<rt>いこう</rt></ruby>を<ruby>最優先<rt>さいゆうせん</rt></ruby>にしてケアプランを<ruby>作成<rt>さくせい</rt></ruby>する。',
                    '<ruby>身体機能<rt>しんたいきのう</rt></ruby>の<ruby>回復<rt>かいふく</rt></ruby>だけを<ruby>目標<rt>もくひょう</rt></ruby>とする。',
                    '<ruby>危険<rt>きけん</rt></ruby>を防ぐため、<ruby>利用者<rt>りようしゃ</rt></ruby>の<ruby>行動<rt>こうどう</rt></ruby>をすべて<ruby>制限<rt>せいげん</rt></ruby>する。'
                ],
                correct: 2,
                explanation: '自立支援とは、単に手足を動かすことだけでなく、<strong><ruby>自己<rt>じこ</rt></ruby><ruby>決定<rt>けってい</rt></ruby></strong>に基づき自分らしい生活を送れるようにサポートすることです。'
            },
            {
                id: 3,
                question: '<ruby>介護<rt>かいご</rt></ruby><ruby>現場<rt>げんば</rt></ruby>における<ruby>プライバシー<rt>プライバシー</rt></ruby>の<ruby>保護<rt>ほご</rt></ruby>として、<ruby>適切<rt>てきせつ</rt></ruby>なものを1つ<ruby>選<rt>えら</rt></ruby>びなさい。',
                choices: [
                    '<ruby>着替え<rt>きがえ</rt></ruby>の際、カーテンを閉めずにそのまま行う。',
                    '<ruby>利用者<rt>りようしゃ</rt></ruby>の<ruby>個人情報<rt>こじんじょうほう</rt></ruby>をSNSに<ruby>投稿<rt>とうこう</rt></ruby>する。',
                    '<ruby>排泄<rt>はいせつ</rt></ruby><ruby>介助<rt>かいじょ</rt></ruby>の際、ドアやカーテンを閉めて<ruby>尊厳<rt>そんげん</rt></ruby;を守る。',
                    '<ruby>他職員<rt>たしょくいん</rt></ruby>への申し送りノートを<ruby>誰<rt>だれ</rt></ruby>でも読める場所に放置する。',
                    '<ruby>利用者<rt>りようしゃ</rt></ruby>の<ruby>許可<rt>きょか</rt></ruby>なく<ruby>私物<rt>しぶつ</rt></ruby>をすべて処分する。'
                ],
                correct: 3,
                explanation: '排泄や着替えの際には、周囲から見えないようカーテンやドアを閉め、<strong><ruby>尊厳<rt>そんげん</rt></ruby>とプライバシー</strong>を守ることが基本です。'
            }
        ];

        let currentQuestionIndex = 0;
        let selectedOption = null;
        let score = 0;

        // ページロード時の初期表示
        window.onload = function() {
            renderQuestion();
        };

        // 問題の描画機能
        function renderQuestion() {
            const q = questions[currentQuestionIndex];
            selectedOption = null;

            // ヘッダーやバッジの更新
            document.getElementById('progressBadge').innerText = `${currentQuestionIndex + 1} / ${questions.length} 問`;
            document.getElementById('questionNumBadge').innerHTML = `<i class="fa-solid fa-circle-question text-emerald-600"></i><span><ruby>問題<rt>もんだい</rt></ruby> ${currentQuestionIndex + 1}</span>`;
            document.getElementById('questionCountText').innerText = `${questions.length}問中 ${currentQuestionIndex + 1}問目`;

            // 問題文セット
            document.getElementById('questionText').innerHTML = q.question;

            // 選択肢ボタンの生成
            const container = document.getElementById('optionsContainer');
            container.innerHTML = '';

            q.choices.forEach((choiceText, index) => {
                const num = index + 1;
                const btn = document.createElement('button');
                btn.id = `btn-${num}`;
                btn.onclick = () => selectOption(num);
                btn.className = "option-btn w-full text-left p-3.5 rounded-2xl border-2 border-slate-200 bg-white hover:border-blue-400 flex items-center justify-between group";
                btn.innerHTML = `
                    <div class="flex items-center gap-3">
                        <span id="badge-${num}" class="w-7 h-7 rounded-full bg-slate-100 text-slate-700 font-bold flex items-center justify-center text-xs flex-shrink-0 group-hover:bg-blue-500 group-hover:text-white transition">${num}</span>
                        <span class="text-slate-800">${choiceText}</span>
                    </div>
                    <i id="icon-${num}" class="fa-regular fa-circle text-slate-300 text-base flex-shrink-0"></i>
                `;
                container.appendChild(btn);
            });

            // 解説ボックスを非表示にする
            document.getElementById('answerBox').classList.add('hidden');
        }

        // 選択肢を選んだ時の処理
        function selectOption(selectedNum) {
            const q = questions[currentQuestionIndex];
            selectedOption = selectedNum;

            // リセット
            for (let i = 1; i <= q.choices.length; i++) {
                const btn = document.getElementById(`btn-${i}`);
                const icon = document.getElementById(`icon-${i}`);
                if (btn) btn.className = "option-btn w-full text-left p-3.5 rounded-2xl border-2 border-slate-200 bg-white hover:border-blue-400 flex items-center justify-between group";
                if (icon) icon.className = "fa-regular fa-circle text-slate-300 text-base flex-shrink-0";
            }

            // 選択ボタンのスタイリング
            const selectedBtn = document.getElementById(`btn-${selectedNum}`);
            const selectedIcon = document.getElementById(`icon-${selectedNum}`);

            if (selectedNum === q.correct) {
                selectedBtn.classList.add("correct-selected");
                selectedIcon.className = "fa-solid fa-circle-check text-emerald-600 text-lg flex-shrink-0";
            } else {
                selectedBtn.classList.add("wrong-selected");
                selectedIcon.className = "fa-solid fa-circle-xmark text-red-500 text-lg flex-shrink-0";
            }

            // 自動で解説を開く
            showAnswer();
        }

        // 解説を表示する機能
        function showAnswer() {
            const q = questions[currentQuestionIndex];
            const answerBox = document.getElementById('answerBox');
            const answerTitle = document.getElementById('answerTitle');
            const answerDescription = document.getElementById('answerDescription');

            answerTitle.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-600"></i><span>【<ruby>正解<rt>せいかい</rt></ruby>】 ${q.correct} 番</span>`;
            answerDescription.innerHTML = q.explanation;
            answerBox.classList.remove('hidden');

            // 正解肢のハイライト強める
            const correctBtn = document.getElementById(`btn-${q.correct}`);
            const correctIcon = document.getElementById(`icon-${q.correct}`);
            if (correctBtn && !correctBtn.classList.contains("wrong-selected")) {
                correctBtn.classList.add("correct-selected");
                correctIcon.className = "fa-solid fa-circle-check text-emerald-600 text-lg flex-shrink-0";
            }
        }

        function toggleAnswer() {
            const answerBox = document.getElementById('answerBox');
            if (answerBox.classList.contains('hidden')) {
                showAnswer();
            } else {
                answerBox.classList.add('hidden');
            }
        }

        // 次の問題に進む処理
        function nextQuestion() {
            const q = questions[currentQuestionIndex];
            if (selectedOption === q.correct) {
                score++;
            }

            currentQuestionIndex++;

            if (currentQuestionIndex < questions.length) {
                renderQuestion();
            } else {
                showResults();
            }
        }

        // 全問題終了時の結果画面
        function showResults() {
            document.getElementById('quizCard').classList.add('hidden');
            document.getElementById('resultCard').classList.remove('hidden');
            document.getElementById('scoreText').innerText = score;
            document.getElementById('totalScoreText').innerText = questions.length;
        }

        // もう一度やり直す処理
        function restartQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            document.getElementById('resultCard').classList.add('hidden');
            document.getElementById('quizCard').classList.remove('hidden');
            renderQuestion();
        }
    </script>
</body>
</html>
