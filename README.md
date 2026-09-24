<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bài 1: Cấu Trúc Của Chất, Sự Chuyển Thể - Vật Lý 12</title>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #6366f1;
            --primary-hover: #4f46e5;
            --secondary: #ec4899;
            --success: #10b981;
            --danger: #ef4444;
            --bg-dark: #0f172a;
            --card-bg: rgba(30, 41, 59, 0.7);
            --text-light: #f8fafc;
            --text-muted: #94a3b8;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Plus Jakarta Sans', sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-dark);
            color: var(--text-light);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
            position: relative;
        }

        /* Animated Background Gradient & Particles */
        .bg-glow {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 600px;
            height: 600px;
            background: radial-gradient(circle, rgba(99, 102, 241, 0.25) 0%, rgba(236, 72, 153, 0.15) 50%, rgba(0,0,0,0) 70%);
            filter: blur(60px);
            z-index: 0;
            pointer-events: none;
        }

        #particles-canvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            pointer-events: none;
        }

        /* Container Card */
        .quiz-container {
            position: relative;
            z-index: 10;
            width: 90%;
            max-width: 700px;
            background: var(--card-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.125);
            border-radius: 24px;
            padding: 32px;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
            transition: all 0.3s ease;
        }

        /* Header UI */
        .quiz-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 24px;
            padding-bottom: 16px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .title-badge {
            background: rgba(99, 102, 241, 0.2);
            color: #818cf8;
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 0.85rem;
            font-weight: 700;
            border: 1px solid rgba(99, 102, 241, 0.3);
        }

        .music-toggle {
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.15);
            color: var(--text-light);
            padding: 8px 16px;
            border-radius: 20px;
            cursor: pointer;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: 0.2s;
        }

        .music-toggle:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        /* Screen 1: Start Screen */
        .start-screen {
            text-align: center;
            padding: 20px 0;
        }

        .start-screen h1 {
            font-size: 2.2rem;
            font-weight: 800;
            margin-bottom: 12px;
            background: linear-gradient(135deg, #fff 0%, #a5b4fc 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .start-screen p {
            color: var(--text-muted);
            margin-bottom: 28px;
            line-height: 1.6;
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            color: white;
            border: none;
            padding: 16px 40px;
            font-size: 1.1rem;
            font-weight: 700;
            border-radius: 16px;
            cursor: pointer;
            box-shadow: 0 10px 25px rgba(99, 102, 241, 0.4);
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 14px 30px rgba(99, 102, 241, 0.6);
        }

        /* Screen 2: Question Screen */
        .quiz-body {
            display: none;
        }

        .timer-bar-container {
            width: 100%;
            height: 6px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 20px;
        }

        .timer-bar {
            height: 100%;
            width: 100%;
            background: linear-gradient(90deg, var(--primary), var(--secondary));
            transition: width 1s linear;
        }

        .stats-info {
            display: flex;
            justify-content: space-between;
            color: var(--text-muted);
            font-size: 0.9rem;
            font-weight: 600;
            margin-bottom: 16px;
        }

        .question-text {
            font-size: 1.25rem;
            font-weight: 700;
            margin-bottom: 24px;
            line-height: 1.5;
        }

        .options-grid {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option-card {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 16px 20px;
            border-radius: 14px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 14px;
            font-weight: 600;
            transition: all 0.2s ease;
        }

        .option-card:hover {
            background: rgba(255, 255, 255, 0.12);
            border-color: rgba(99, 102, 241, 0.5);
            transform: translateX(4px);
        }

        .option-card.correct {
            background: rgba(16, 185, 129, 0.2) !important;
            border-color: var(--success) !important;
            color: #6ee7b7;
        }

        .option-card.wrong {
            background: rgba(239, 68, 68, 0.2) !important;
            border-color: var(--danger) !important;
            color: #fca5a5;
        }

        .option-card.disabled {
            pointer-events: none;
            opacity: 0.6;
        }

        .option-prefix {
            width: 32px;
            height: 32px;
            border-radius: 8px;
            background: rgba(255, 255, 255, 0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.9rem;
            color: #a5b4fc;
        }

        /* Screen 3: Result Screen */
        .result-screen {
            display: none;
            text-align: center;
            padding: 20px 0;
        }

        .score-circle {
            width: 140px;
            height: 140px;
            border-radius: 50%;
            background: linear-gradient(135deg, rgba(99,102,241,0.2), rgba(236,72,153,0.2));
            border: 3px solid var(--primary);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin: 0 auto 24px;
        }

        .score-number {
            font-size: 2.8rem;
            font-weight: 800;
            color: #fff;
        }

        .score-label {
            font-size: 0.8rem;
            color: var(--text-muted);
            text-transform: uppercase;
        }

        .result-title {
            font-size: 1.8rem;
            font-weight: 800;
            margin-bottom: 8px;
        }

        .result-desc {
            color: var(--text-muted);
            margin-bottom: 28px;
        }
    </style>
</head>
<body>

    <div class="bg-glow"></div>
    <canvas id="particles-canvas"></canvas>

    <div class="quiz-container">
        <!-- Header -->
        <div class="quiz-header">
            <span class="title-badge">VẬT LÝ 12 THPT</span>
            <button class="music-toggle" id="musicToggleBtn" onclick="toggleMusic()">
                <span id="musicIcon">🎵</span> Nhạc nền: <span id="musicStatus">Tắt</span>
            </button>
        </div>

        <!-- Start Screen -->
        <div class="start-screen" id="startScreen">
            <h1>Bài 1: Cấu Trúc Của Chất, Sự Chuyển Thể</h1>
            <p>Trò chơi trắc nghiệm 10 câu hỏi ôn tập tổng hợp kiến thức về Mô hình động học phân tử, các thể của chất và quá trình chuyển thể.</p>
            <button class="btn-primary" onclick="startQuiz()">Bắt Đầu Bứt Phá 🚀</button>
        </div>

        <!-- Quiz Screen -->
        <div class="quiz-body" id="quizBody">
            <div class="stats-info">
                <span>Câu hỏi <strong id="currentQ">1</strong>/10</span>
                <span>Thời gian: <strong id="timerText" style="color: #a5b4fc;">20s</strong></span>
                <span>Điểm: <strong id="scoreText" style="color: var(--success);">0</strong></span>
            </div>
            
            <div class="timer-bar-container">
                <div class="timer-bar" id="timerBar"></div>
            </div>

            <div class="question-text" id="questionText">Đang tải câu hỏi...</div>

            <div class="options-grid" id="optionsGrid">
                <!-- Options injected via JS -->
            </div>
        </div>

        <!-- Result Screen -->
        <div class="result-screen" id="resultScreen">
            <div class="score-circle">
                <div class="score-number" id="finalScore">0</div>
                <div class="score-label">Điểm số</div>
            </div>
            <h2 class="result-title" id="resultTitle">Xuất sắc!</h2>
            <p class="result-desc" id="resultDesc">Bạn đã nắm vững toàn bộ kiến thức Bài 1 môn Vật Lý 12.</p>
            <button class="btn-primary" onclick="restartQuiz()">Chơi Lại Bài Quiz 🔄</button>
        </div>
    </div>

    <script>
        // Data Questions derived strictly from Image Notes
        const quizData = [
            {
                q: "Các chất được cấu tạo từ các hạt riêng biệt gọi là gì, và giữa chúng có đặc điểm gì?",
                options: [
                    "Phân tử, giữa chúng không có khoảng cách",
                    "Phân tử, giữa chúng có khoảng cách",
                    "Nguyên tử, giữa chúng luôn đứng yên",
                    "Hạt nhân, giữa chúng luôn đẩy nhau"
                ],
                answer: 1
            },
            {
                q: "Khi khoảng cách giữa các phân tử rất gần nhau thì lực tương tác phân tử có đặc điểm gì?",
                options: [
                    "Lực hút chiếm ưu thế",
                    "Lực đẩy chiếm ưu thế",
                    "Không có lực tương tác",
                    "Lực hút bằng lực đẩy"
                ],
                answer: 1
            },
            {
                q: "Ở thể khí, chuyển động của các phân tử và khoảng cách giữa chúng như thế nào?",
                options: [
                    "Chuyển động hỗn loạn; khoảng cách rất lớn (gấp hàng chục lần kích thước phân tử)",
                    "Dao động quanh vị trí cân bằng xác định; khoảng cách rất nhỏ",
                    "Dao động quanh vị trí cân bằng di chuyển; khoảng cách nhỏ",
                    "Chuyển động theo quỹ đạo tròn; khoảng cách bằng kích thước phân tử"
                ],
                answer: 0
            },
            {
                q: "Chất ở thể rắn có đặc điểm nào sau đây về hình dạng và thể tích?",
                options: [
                    "Hình dạng thay đổi, thể tích không xác định",
                    "Hình dạng xác định, thể tích xác định",
                    "Hình dạng của bình chứa, thể tích xác định",
                    "Hình dạng xác định, rất dễ bị nén"
                ],
                answer: 1
            },
            {
                q: "Quá trình chuyển từ thể rắn trực tiếp sang thể khí gọi là sự gì?",
                options: [
                    "Ngưng tụ",
                    "Nóng chảy",
                    "Thăng hoa",
                    "Ngưng kết"
                ],
                answer: 2
            },
            {
                q: "Điểm khác biệt cơ bản giữa 'Sự bay hơi' và 'Sự sôi' là gì?",
                options: [
                    "Sự bay hơi xảy ra ở nhiệt độ xác định, sự sôi xảy ra ở mọi nhiệt độ",
                    "Sự bay hơi chỉ xảy ra ở mặt thoáng, sự sôi xảy ra cả ở mặt thoáng và trong lòng chất lỏng",
                    "Sự bay hơi tỏa nhiệt lượng, sự sôi thu nhiệt lượng",
                    "Sự bay hơi làm tăng nhiệt độ chất lỏng, sự sôi làm giảm nhiệt độ"
                ],
                answer: 1
            },
            {
                q: "Phát biểu nào sau đây ĐÚNG về chất rắn kết tinh?",
                options: [
                    "Không có cấu trúc tinh thể và không có nhiệt độ nóng chảy xác định",
                    "Có cấu trúc tinh thể và có nhiệt độ nóng chảy xác định",
                    "Khi nung nóng thì mềm dần rồi mới chuyển sang thể lỏng",
                    "Luôn có tính đẳng hướng trong mọi trường hợp"
                ],
                answer: 1
            },
            {
                q: "Các chất rắn như thủy tinh, nhựa, sôcôla, sáp nến thuộc loại chất rắn nào?",
                options: [
                    "Chất rắn đơn tinh thể",
                    "Chất rắn đa tinh thể",
                    "Chất rắn vô định hình",
                    "Chất rắn bán dẫn"
                ],
                answer: 2
            },
            {
                q: "Chất rắn đơn tinh thể (như hạt muối, thạch anh, kim cương) có đặc tính vật lý nào?",
                options: [
                    "Tính đẳng hướng",
                    "Tính dị hướng",
                    "Không có nhiệt độ nóng chảy",
                    "Dễ bị nén ép"
                ],
                answer: 1
            },
            {
                q: "Trong suốt quá trình một chất rắn kết tinh đang nóng chảy, nhiệt độ của nó sẽ:",
                options: [
                    "Tăng liên tục",
                    "Giảm dần",
                    "Không thay đổi",
                    "Thay đổi liên tục"
                ],
                answer: 2
            }
        ];

        // State variables
        let currentQuestion = 0;
        let score = 0;
        let timeLeft = 20;
        let timer = null;
        let isAnswered = false;

        // Audio System (Web Audio API Synthesizer)
        let audioCtx = null;
        let isMusicPlaying = false;
        let bgMusicInterval = null;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
        }

        // Play Tone Generator
        function playTone(freq, type, duration, vol = 0.1) {
            if (!audioCtx) return;
            try {
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = type;
                osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
                gain.gain.setValueAtTime(vol, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + duration);
                osc.connect(gain);
                gain.connect(audioCtx.destination);
                osc.start();
                osc.stop(audioCtx.currentTime + duration);
            } catch (e) {}
        }

        // Sound Effects
        function soundCorrect() {
            initAudio();
            playTone(523.25, 'sine', 0.2, 0.15); // C5
            setTimeout(() => playTone(659.25, 'sine', 0.3, 0.15), 100); // E5
        }

        function soundWrong() {
            initAudio();
            playTone(220, 'sawtooth', 0.3, 0.12); // A3
            setTimeout(() => playTone(180, 'sawtooth', 0.4, 0.12), 150);
        }

        function soundTimeout() {
            initAudio();
            playTone(300, 'square', 0.15, 0.1);
            setTimeout(() => playTone(200, 'square', 0.3, 0.1), 150);
        }

        function soundComplete() {
            initAudio();
            const notes = [440, 554.37, 659.25, 880];
            notes.forEach((freq, idx) => {
                setTimeout(() => playTone(freq, 'triangle', 0.4, 0.12), idx * 120);
            });
        }

        // Chill Background Music Generator (Lo-Fi Ambient Chords)
        function toggleMusic() {
            initAudio();
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            isMusicPlaying = !isMusicPlaying;
            const statusEl = document.getElementById('musicStatus');
            const iconEl = document.getElementById('musicIcon');

            if (isMusicPlaying) {
                statusEl.innerText = "Bật";
                iconEl.innerText = "🎶";
                startChillMusic();
            } else {
                statusEl.innerText = "Tắt";
                iconEl.innerText = "🎵";
                stopChillMusic();
            }
        }

        function startChillMusic() {
            const chords = [
                [261.63, 329.63, 392.00, 493.88], // Cmaj7
                [220.00, 261.63, 329.63, 392.00], // Am7
                [174.61, 220.00, 261.63, 329.63], // Fmaj7
                [196.00, 246.94, 293.66, 349.23]  // G7
            ];
            let chordIdx = 0;

            bgMusicInterval = setInterval(() => {
                if (!isMusicPlaying || !audioCtx) return;
                const chord = chords[chordIdx];
                chord.forEach(freq => {
                    playTone(freq, 'sine', 2.5, 0.02);
                });
                chordIdx = (chordIdx + 1) % chords.length;
            }, 3000);
        }

        function stopChillMusic() {
            if (bgMusicInterval) clearInterval(bgMusicInterval);
        }

        // Quiz Logic Functions
        function startQuiz() {
            initAudio();
            document.getElementById('startScreen').style.display = 'none';
            document.getElementById('quizBody').style.display = 'block';
            currentQuestion = 0;
            score = 0;
            loadQuestion();
        }

        function loadQuestion() {
            isAnswered = false;
            clearInterval(timer);
            timeLeft = 20;

            const qData = quizData[currentQuestion];
            document.getElementById('currentQ').innerText = currentQuestion + 1;
            document.getElementById('scoreText').innerText = score;
            document.getElementById('questionText').innerText = qData.q;
            document.getElementById('timerText').innerText = timeLeft + 's';

            const optionsGrid = document.getElementById('optionsGrid');
            optionsGrid.innerHTML = '';

            const prefixes = ['A', 'B', 'C', 'D'];
            qData.options.forEach((opt, idx) => {
                const card = document.createElement('div');
                card.className = 'option-card';
                card.onclick = () => checkAnswer(idx);
                card.innerHTML = `
                    <div class="option-prefix">${prefixes[idx]}</div>
                    <div>${opt}</div>
                `;
                optionsGrid.appendChild(card);
            });

            startTimer();
        }

        function startTimer() {
            const timerBar = document.getElementById('timerBar');
            timerBar.style.transition = 'none';
            timerBar.style.width = '100%';

            setTimeout(() => {
                timerBar.style.transition = 'width 1s linear';
            }, 50);

            timer = setInterval(() => {
                timeLeft--;
                document.getElementById('timerText').innerText = timeLeft + 's';
                timerBar.style.width = (timeLeft / 20 * 100) + '%';

                if (timeLeft <= 0) {
                    clearInterval(timer);
                    soundTimeout();
                    highlightCorrectAnswer();
                    setTimeout(nextQuestion, 1500);
                }
            }, 1000);
        }

        function checkAnswer(selectedIndex) {
            if (isAnswered) return;
            isAnswered = true;
            clearInterval(timer);

            const cards = document.querySelectorAll('.option-card');
            cards.forEach(card => card.classList.add('disabled'));

            const correctIndex = quizData[currentQuestion].answer;

            if (selectedIndex === correctIndex) {
                cards[selectedIndex].classList.add('correct');
                score += 10;
                document.getElementById('scoreText').innerText = score;
                soundCorrect();
            } else {
                cards[selectedIndex].classList.add('wrong');
                cards[correctIndex].classList.add('correct');
                soundWrong();
            }

            setTimeout(nextQuestion, 1600);
        }

        function highlightCorrectAnswer() {
            const cards = document.querySelectorAll('.option-card');
            const correctIndex = quizData[currentQuestion].answer;
            cards.forEach(card => card.classList.add('disabled'));
            cards[correctIndex].classList.add('correct');
        }

        function nextQuestion() {
            currentQuestion++;
            if (currentQuestion < quizData.length) {
                loadQuestion();
            } else {
                showResult();
            }
        }

        function showResult() {
            document.getElementById('quizBody').style.display = 'none';
            document.getElementById('resultScreen').style.display = 'block';

            document.getElementById('finalScore').innerText = score;
            soundComplete();

            const titleEl = document.getElementById('resultTitle');
            const descEl = document.getElementById('resultDesc');

            if (score >= 80) {
                titleEl.innerText = "Xuất Sắc! 🌟";
                descEl.innerText = "Bạn nắm rất vững toàn bộ lý thuyết Cấu trúc chất & Sự chuyển thể!";
            } else if (score >= 50) {
                titleEl.innerText = "Khá Tốt! 👍";
                descEl.innerText = "Bạn đã hiểu hầu hết kiến thức, hãy ôn lại các định nghĩa chi tiết nhé.";
            } else {
                titleEl.innerText = "Cần Cố Gắng Hơn! 💪";
                descEl.innerText = "Hãy đọc lại bảng so sánh 3 thể và chất rắn kết tinh trong bài học nhé.";
            }
        }

        function restartQuiz() {
            document.getElementById('resultScreen').style.display = 'none';
            startQuiz();
        }

        // Particle Animation Canvas
        const canvas = document.getElementById('particles-canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }

        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        class Particle {
            constructor() {
                this.reset();
            }
            reset() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.radius = Math.random() * 2 + 1;
                this.alpha = Math.random() * 0.5 + 0.1;
                this.vx = (Math.random() - 0.5) * 0.5;
                this.vy = (Math.random() - 0.5) * 0.5;
            }
            update() {
                this.x += this.vx;
                this.y += this.vy;
                if (this.x < 0 || this.x > canvas.width || this.y < 0 || this.y > canvas.height) {
                    this.reset();
                }
            }
            draw() {
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                ctx.fillStyle = `rgba(165, 180, 252, ${this.alpha})`;
                ctx.fill();
            }
        }

        for (let i = 0; i < 50; i++) {
            particles.push(new Particle());
        }

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            particles.forEach(p => {
                p.update();
                p.draw();
            });
            requestAnimationFrame(animateParticles);
        }
        animateParticles();
    </script>
</body>
</html>
