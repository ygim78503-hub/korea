<!DOCTYPE HTML>
<HTML>
<HEAD>
    <TITLE>개인용ai 노트</TITLE>
    <style>
        /* 🖤 전체 기본 화면 세팅 */
        body {
            background-color: #000000;
            color: #ffffff;
            margin: 0;
            overflow: hidden;
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", Roboto, sans-serif;
        }

        /* 🌈 메인 화면용 정중앙 정렬 방 */
        .main-container {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-align: center;
        }

        h1 {
            font-size: 56px;
            background: linear-gradient(to right, #4285f4, #9b51e0, #ea4335, #fbbc05, #4285f4);
            background-size: 200% auto;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            animation: rainbow-flow 2s linear infinite;
        }

        @keyframes rainbow-flow {
            0% { background-position: 0% center; }
            100% { background-position: -200% center; }
        }
        
        button {
            padding: 10px 20px;
            background-color: #2997ff;
            color: white;
            font-size: 18px;
            border: none;
            border-radius: 20px;
            cursor: pointer;
        }

        /* 🗺️ 좌/우 분할 레이아웃 컨트롤러 */
        .memo-layout-workspace {
            display: none;
            width: 100vw;
            height: 100vh;
            box-sizing: border-box;
        }

        /* 👈 [사이드바 구역] */
        .left-sidebar {
            width: 80px;
            height: 100vh;
            padding: 20px 0 0 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            box-sizing: border-box;
            float: left;
            align-items: center;
        }

        /* 🔙 뒤로가기 버튼 */
        #back-btn {
            width: 40px;
            height: 40px;
            padding: 0;
            background-color: #1c1c1e;
            color: #2997ff;
            font-size: 16px;
            border: 1px solid #2997ff;
            border-radius: 8px;
            cursor: pointer;
        }
        #back-btn:hover {
            background-color: #2997ff;
            color: #ffffff;
        }

        /* 🎙️ 음성 인식 버튼 */
        #speech-btn {
            width: 50px;
            height: 40px;
            padding: 0;
            font-size: 14px;
            background-color: #0a192f;
            color: #38bdf8;
            border: 1px solid #38bdf8;
            font-weight: bold;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(56, 189, 248, 0.15);
        }
        #speech-btn:hover {
            background-color: #38bdf8;
            color: #000000;
        }

        /* 🔊 스피커 토글 버튼 */
        #speak-btn {
            width: 50px;
            height: 40px;
            padding: 0;
            font-size: 14px;
            background-color: #0a192f;
            color: #a855f7;
            border: 1px solid #a855f7;
            font-weight: bold;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(168, 85, 247, 0.15);
            cursor: pointer;
        }

        /* 🎵 구글 검색 스타일의 말할 때 춤추는 점 4개 컨테이너 */
        .google-dots-container {
            display: none;
            gap: 4px;
            justify-content: center;
            align-items: center;
            height: 30px;
            width: 50px;
            margin-top: 5px;
        }
        .google-dots-container.active {
            display: flex;
        }

        /* 동글동글한 점 설정 */
        .google-dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            transition: transform 0.05s ease-out;
        }
        .google-dot:nth-child(1) { background-color: #4285f4; }
        .google-dot:nth-child(2) { background-color: #ea4335; }
        .google-dot:nth-child(3) { background-color: #fbbc05; }
        .google-dot:nth-child(4) { background-color: #34a853; }
                /* 👉 [거대한 메모장 구역 마스터 컨트롤러] */
        .right-content {
            margin-left: 80px;
            height: 100vh;
            padding: 20px;
            box-sizing: border-box;
            display: flex; /* 좌우로 쪼개기 위한 상자 설정 */
            gap: 20px;     /* 두 상자 사이의 여백 간격 */
        }

        /* 📝 1번방: [사용자 메모 공간] 전체의 3/5 (60%) 크기 배정 */
        .user-memo-container {
            flex: 3;
            height: calc(100vh - 40px);
        }

        /* 🤖 2번방: [자비스 AI 요약 공간] 전체의 2/5 (40%) 크기 배정 */
        .ai-memo-container {
            flex: 2;
            height: calc(100vh - 40px);
        }

        /* 📥 두 영역 안에 들어가는 실제 글상자 공통 스타일 디자인 */
        textarea {
            width: 100%;
            height: 100%;
            background-color: #1c1c1e;
            color: #ffffff;
            font-size: 18px;
            padding: 25px;
            outline: none;
            resize: none;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "Segoe UI", Roboto, sans-serif;
        }

        /* 👈 왼쪽 사용자 메모장 테두리는 시원한 파란색 */
        #memo-box {
            border: 2px solid #2997ff;
            border-radius: 16px;
        }

        /* 👉 오른쪽 자비스 요약창 테두리는 미래지향적인 보라색 */
        #ai-box {
            border: 2px solid #a855f7;
            border-radius: 16px;
            background-color: #121214; /* 요약창은 살짝 다르게 톤다운 */
        }
    </style>
</HEAD>
<BODY>
    <!-- 1층 무대: 초기 정중앙 화면 -->
    <div id="main-view" class="main-container">
        <h1>안녕하세요</h1>
        <button id="memo-btn" onclick="openmemo()">메모창 열기</button>
    </div>

    <!-- 2층 무대: 반듯하게 3:2로 쪼개진 분할 작업 공간 -->
    <div id="workspace-view" class="memo-layout-workspace">
        <div class="left-sidebar">
            <button id="back-btn" onclick="closememo()">◀</button>
            <button id="speech-btn" onclick="startSpeech()">🎙️</button>
            <button id="speak-btn" onclick="toggleAudioSwitch()">🔊 켜짐</button>
            
            <!-- 🎵 🔊 버튼 밑에 고정 배치된 점 4개 구조 -->
            <div id="google-dots" class="google-dots-container">
                <div class="google-dot"></div>
                <div class="google-dot"></div>
                <div class="google-dot"></div>
                <div class="google-dot"></div>
            </div>
        </div>
        
        <!-- 좌우 3:2 분할 상자 배치 -->
        <div class="right-content">
            <div class="user-memo-container">
                <!-- 사용자가 손으로 쓰는 파란색 메모장 -->
                <textarea id="memo-box" oninput="savememo()" placeholder="이곳은 사용자 전용 메모장입니다..."></textarea>
            </div>
            <div class="ai-memo-container">
                <!-- 자비스가 요약본만 차곡차곡 던져주는 보라색 요약창 -->
                <textarea id="ai-box" oninput="savememo()" placeholder="자비스 AI 요약 대기 중..." readonly></textarea>
            </div>
        </div>
    </div>
        <script>
        var SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        var recognition = new SpeechRecognition();
        
        // 🎯 [다국어 지원] 한국어와 영어를 동시에 정밀하게 인식하도록 다중 언어 환경 셋업
        recognition.lang = 'ko-KR'; 
        recognition.interimResults = true; 
        recognition.maxAlternatives = 1;
        recognition.continuous = true; 

        var isListening = false;
        var isAudioOn = true;

        // 🎙️ 오디오 분석 장치 전용 보관소
        var audioContext;
        var analyser;
        var micStream;
        var lastHeights = new Array(0, 0, 0, 0); 

        recognition.onresult = function(event) {
            var currentResultIndex = event.resultIndex;
            var speechResult = event.results[currentResultIndex].transcript.trim();
            var aiBox = document.getElementById('ai-box');

            // 🎯 [단순화 완료] 복잡한 호출어 검사 단계를 전부 패스하고, 말이 확정되는 순간 즉시 작동합니다.
            if (event.results[currentResultIndex].isFinal) {
                // 불필요한 말버릇 필터링
                var summarizedText = speechResult
                    .replace(/말이야|했거든|진짜로|그니까|진짜|약간/g, "")
                    .trim();
                
                if (summarizedText.length > 0) {
                    var fullNewText = (aiBox.value === "") ? summarizedText : "\n" + summarizedText;
                    
                    // 🎯 [출력 위치 고정] 왼쪽 메모장은 터치 노터치! 오직 오른쪽 보라색 요약창에만 실시간 타이핑을 칩니다.
                    var charIndex = 0;
                    function typeWriter() {
                        if (charIndex < fullNewText.length) {
                            aiBox.value += fullNewText.charAt(charIndex);
                            charIndex++;
                            savememo(); 
                            setTimeout(typeWriter, 50);
                        }
                    }
                    typeWriter();
                }
            }
        };

        // 브라우저 마이크 자동 꺼짐을 방지하는 심폐소생술 시스템
        recognition.onend = function() {
            if (isListening) {
                try { recognition.start(); } catch(e) {}
            } else {
                document.getElementById('speech-btn').innerText = "🎙️";
                document.getElementById('speech-btn').style.backgroundColor = "#0a192f";
                document.getElementById('google-dots').classList.remove('active');
                stopMicAnalysis();
            }
        };

        function startSpeech() {
            if (isListening) {
                isListening = false;
                recognition.stop();
            } else {
                try {
                    window.speechSynthesis.cancel();
                    recognition.start();
                    isListening = true;
                    document.getElementById('speech-btn').innerText = "🎧";
                    document.getElementById('speech-btn').style.backgroundColor = "#ea4335";
                    
                    // 마이크가 켜짐과 동시에 구글 점 4개 파동 작동 시작
                    document.getElementById('google-dots').classList.add('active');
                    startMicAnalysis();
                } catch(e) {
                    recognition.stop();
                }
            }
        }

        // 📊 앙증맞은 엇박자 파동 가동 엔진 (최대 2~3px 내외 연산)
        function startMicAnalysis() {
            if (audioContext) return; 
            navigator.mediaDevices.getUserMedia({ audio: true }).then(function(stream) {
                micStream = stream;
                audioContext = new (window.AudioContext || window.webkitAudioContext)();
                analyser = audioContext.createAnalyser();
                
                var source = audioContext.createMediaStreamSource(stream);
                source.connect(analyser);
                
                analyser.fftSize = 64; 
                analyser.smoothingTimeConstant = 0.8; 

                var bufferLength = analyser.frequencyBinCount;
                var dataArray = new Uint8Array(bufferLength);
                var dots = document.querySelectorAll('.google-dot');

                function drawVolume() {
                    if (!isListening) return; 
                    analyser.getByteFrequencyData(dataArray);

                    var targetIndices = new Array(1, 4, 8, 12); 

                    dots.forEach(function(dot, index) {
                        var rawValue = dataArray[targetIndices[index]];
                        var targetHeight = (rawValue > 35) ? (rawValue / 45.0) : 0;
                        if (targetHeight > 4) targetHeight = 3.5; 
                        
                        var smoothHeight = lastHeights[index] + (targetHeight - lastHeights[index]) * 0.25;
                        lastHeights[index] = smoothHeight;

                        dot.style.transform = `translateY(-${smoothHeight}px)`;
                    });

                    requestAnimationFrame(drawVolume);
                }
                drawVolume();
            }).catch(function(err) {
                console.log("마이크 분석 실패:", err);
            });
        }

        function stopMicAnalysis() {
            if (micStream) { micStream.getTracks().forEach(track => track.stop()); micStream = null; }
            if (audioContext) { audioContext.close(); audioContext = null; }
            lastHeights = new Array(0, 0, 0, 0);
            document.querySelectorAll('.google-dot').forEach(dot => dot.style.transform = 'translateY(0)');
        }

        function toggleAudioSwitch() {
            var speakBtn = document.getElementById('speak-btn');
            if (isAudioOn) {
                isAudioOn = false;
                speakBtn.innerText = "🔇 꺼짐"; speakBtn.style.color = "#86868b";
                speakBtn.style.border = "1px solid #86868b"; speakBtn.style.boxShadow = "none";
            } else {
                isAudioOn = true;
                speakBtn.innerText = "🔊 켜짐"; speakBtn.style.color = "#a855f7";
                speakBtn.style.border = "1px solid #a855f7";
                speakBtn.style.boxShadow = "0 4px 15px rgba(168, 85, 247, 0.15)";
            }
        }

        // 🌟 독립 분할된 사용자 및 AI 저장고 자동 복원 시스템
        window.onload = function() {
            var savedUserText = localStorage.getItem('mySecretMemo_User');
            var savedAiText = localStorage.getItem('mySecretMemo_Ai');
            
            if (savedUserText) { document.getElementById('memo-box').value = savedUserText; }
            if (savedAiText) { document.getElementById('ai-box').value = savedAiText; }
            
            // 🎯 최초 로드 시 마이크를 자동으로 대기 모드로 켭니다.
            startSpeech(); 
        }

        function savememo() {
            var userValue = document.getElementById('memo-box').value;
            var aiValue = document.getElementById('ai-box').value;
            
            localStorage.setItem('mySecretMemo_User', userValue);
            localStorage.setItem('mySecretMemo_Ai', aiValue);
        }

        function openmemo() {
            document.getElementById('main-view').style.display = 'none';
            document.getElementById('workspace-view').style.display = 'block';
        }

        function closememo() {
            document.getElementById('workspace-view').style.display = 'none';
            document.getElementById('main-view').style.display = 'flex';
            isListening = false;
            recognition.stop();
        }
    </script>
</BODY>
</HTML>


