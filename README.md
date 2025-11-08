<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>慧の专属业绩计算器</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * { 
            margin: 0; 
            padding: 0; 
            box-sizing: border-box; 
            font-family: "Comic Sans MS", "Segoe UI", "Microsoft YaHei", Arial, sans-serif; 
        }
        body { 
            background: linear-gradient(135deg, #ffcce6 0%, #ffb3d9 100%);
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow-x: hidden;
        }
        
        /* Hello Kitty装饰元素 */
        .kitty-decoration {
            position: absolute;
            z-index: 1;
            pointer-events: none;
        }
        .kitty-decoration.top-left {
            top: 10px;
            left: 10px;
            font-size: 40px;
            transform: rotate(-15deg);
        }
        .kitty-decoration.top-right {
            top: 10px;
            right: 10px;
            font-size: 40px;
            transform: rotate(15deg);
        }
        .kitty-decoration.bottom-left {
            bottom: 10px;
            left: 10px;
            font-size: 40px;
            transform: rotate(15deg);
        }
        .kitty-decoration.bottom-right {
            bottom: 10px;
            right: 10px;
            font-size: 40px;
            transform: rotate(-15deg);
        }
        .bow {
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 30px;
            z-index: 10;
        }
        
        .container { 
            max-width: 700px; 
            width: 100%;
            margin: 20px auto; 
            background: #fff; 
            border-radius: 25px; 
            box-shadow: 0 10px 30px rgba(255, 105, 180, 0.3); 
            padding: 35px;
            position: relative;
            overflow: hidden;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
            border: 5px solid #ff66b2;
            z-index: 2;
        }
        .container:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(255, 105, 180, 0.4);
        }
        .title { 
            font-size: 28px; 
            font-weight: 700; 
            color: #ff66b2; 
            text-align: center; 
            margin-bottom: 30px; 
            padding-bottom: 20px; 
            border-bottom: 2px dotted #ffb3d9;
            position: relative;
            text-shadow: 2px 2px 0px #ffe6f2;
        }
        .title:after {
            content: '';
            position: absolute;
            bottom: -2px;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 5px;
            background: #ff66b2;
            border-radius: 5px;
        }
        .form-group { 
            margin-bottom: 25px; 
            position: relative;
        }
        label { 
            display: block; 
            margin-bottom: 10px; 
            color: #ff66b2; 
            font-weight: 600; 
            font-size: 16px; 
            transition: color 0.3s;
        }
        input { 
            width: 100%; 
            padding: 14px 16px; 
            border: 2px solid #ffb3d9; 
            border-radius: 15px; 
            font-size: 16px; 
            outline: none; 
            transition: all 0.3s; 
            background: #fff9fc;
        }
        input:focus { 
            border-color: #ff66b2; 
            box-shadow: 0 0 0 3px rgba(255, 102, 178, 0.2); 
            background: #fff;
        }
        .radio-group { 
            display: flex; 
            gap: 20px; 
            margin-bottom: 10px; 
            flex-wrap: wrap;
        }
        .radio-item { 
            display: flex; 
            align-items: center; 
            gap: 8px; 
            color: #ff66b2; 
            cursor: pointer;
            padding: 8px 12px;
            border-radius: 15px;
            transition: background 0.2s;
            background: #fff0f5;
            border: 2px solid #ffb3d9;
        }
        .radio-item:hover {
            background: #ffe6f2;
        }
        .radio-item input[type="radio"] {
            width: auto;
            margin-right: 5px;
            accent-color: #ff66b2;
        }
        button { 
            width: 100%; 
            padding: 16px; 
            background: linear-gradient(to right, #ff66b2, #ff3385); 
            color: #fff; 
            border: none; 
            border-radius: 15px; 
            font-size: 18px; 
            font-weight: 600; 
            cursor: pointer; 
            transition: all 0.3s; 
            margin: 15px 0 30px; 
            box-shadow: 0 4px 12px rgba(255, 102, 178, 0.4);
            position: relative;
            overflow: hidden;
        }
        button:hover { 
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(255, 102, 178, 0.6);
        }
        button:active {
            transform: translateY(0);
        }
        .result-card { 
            background: linear-gradient(135deg, #ffe6f2 0%, #ffd9eb 100%);
            border-radius: 20px; 
            padding: 30px; 
            border-left: 5px solid #ff66b2;
            box-shadow: 0 4px 12px rgba(0,0,0,0.05);
            animation: fadeIn 0.5s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .result-title { 
            font-size: 20px; 
            font-weight: 700; 
            color: #ff66b2; 
            margin-bottom: 25px; 
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .result-title:before {
            content: '';
            display: block;
            width: 6px;
            height: 20px;
            background: #ff66b2;
            border-radius: 3px;
        }
        .result-item { 
            display: flex; 
            justify-content: space-between; 
            margin: 14px 0; 
            padding: 10px 0; 
            border-bottom: 1px dashed #ffb3d9; 
        }
        .result-label { 
            color: #ff66b2; 
            font-size: 15px; 
        }
        .result-value { 
            color: #ff3385; 
            font-weight: 700; 
            font-size: 16px; 
        }
        .calc-process { 
            margin-top: 25px; 
            padding: 20px; 
            background: #fff; 
            border-radius: 15px; 
            border: 2px dashed #ffb3d9; 
            box-shadow: 0 2px 8px rgba(0,0,0,0.03);
        }
        .process-title { 
            font-size: 15px; 
            color: #ff66b2; 
            margin-bottom: 12px; 
            font-weight: 600; 
        }
        .process-step { 
            font-size: 14px; 
            color: #ff3385; 
            line-height: 1.8; 
            margin-bottom: 8px; 
            padding-left: 15px;
            position: relative;
        }
        .process-step:before {
            content: '•';
            position: absolute;
            left: 0;
            color: #ff66b2;
        }
        .tips { 
            color: #ff66b2; 
            font-size: 13px; 
            margin-top: 20px; 
            line-height: 1.6; 
            text-align: center; 
            padding: 15px;
            background: #fff0f5;
            border-radius: 15px;
            border: 2px dotted #ffb3d9;
        }
        
        /* 音乐播放器样式 - Hello Kitty主题 */
        .music-player {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #fff;
            border-radius: 50px;
            box-shadow: 0 5px 20px rgba(255, 105, 180, 0.3);
            padding: 12px 20px;
            display: flex;
            align-items: center;
            gap: 15px;
            z-index: 100;
            transition: all 0.3s ease;
            width: auto;
            max-width: 500px;
            border: 3px solid #ff66b2;
        }
        .music-player:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(255, 105, 180, 0.4);
        }
        .music-info {
            display: flex;
            align-items: center;
            gap: 15px;
            flex: 1;
            min-width: 0;
        }
        .song-info {
            font-size: 14px;
            color: #ff66b2;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            min-width: 120px;
            max-width: 180px;
            font-weight: 600;
        }
        .music-controls {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .music-btn {
            background: #ff66b2;
            color: white;
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 3px 8px rgba(255, 102, 178, 0.4);
            flex-shrink: 0;
        }
        .music-btn:hover {
            background: #ff3385;
            transform: scale(1.05);
        }
        .music-btn:active {
            transform: scale(0.95);
        }
        .volume-control {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-left: 10px;
        }
        
        .volume-slider {
            width: 80px;
            accent-color: #ff66b2;
        }
        
        /* 响应式设计 */
        @media (max-width: 768px) {
            .container {
                padding: 25px 20px;
                margin: 10px;
            }
            .title {
                font-size: 22px;
            }
            .radio-group {
                flex-direction: column;
                gap: 10px;
            }
            .music-player {
                position: relative;
                top: auto;
                right: auto;
                margin: 0 auto 20px;
                width: 100%;
                max-width: 400px;
                justify-content: center;
                flex-wrap: wrap;
                border-radius: 25px;
                padding: 15px;
            }
            .music-info {
                justify-content: center;
                width: 100%;
                margin-bottom: 10px;
            }
            .song-info {
                max-width: 200px;
            }
            .music-controls {
                justify-content: center;
                width: 100%;
            }
            .kitty-decoration {
                display: none;
            }
        }
        
        /* 输入验证样式 */
        input:invalid {
            border-color: #ff4757;
        }
        input:valid {
            border-color: #ff66b2;
        }
        .validation-message {
            color: #ff4757;
            font-size: 12px;
            margin-top: 5px;
            display: none;
        }
        input:invalid + .validation-message {
            display: block;
        }
        
        /* Hello Kitty 头像 */
        .kitty-face {
            position: absolute;
            bottom: -30px;
            right: -30px;
            width: 150px;
            height: 150px;
            background: white;
            border-radius: 50%;
            z-index: -1;
            box-shadow: 0 0 0 10px #ff66b2;
        }
        .kitty-face:before,
        .kitty-face:after {
            content: '';
            position: absolute;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 0 8px #ff66b2;
        }
        .kitty-face:before {
            width: 60px;
            height: 60px;
            top: -30px;
            left: 20px;
        }
        .kitty-face:after {
            width: 60px;
            height: 60px;
            top: -30px;
            right: 20px;
        }
        
        /* 粉色爱心动画 */
        .floating-hearts {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: 1;
            pointer-events: none;
        }
        .heart {
            position: absolute;
            color: #ff66b2;
            font-size: 20px;
            opacity: 0.7;
            animation: float 6s ease-in-out infinite;
        }
        @keyframes float {
            0% { transform: translateY(0) rotate(0deg); opacity: 0.7; }
            50% { transform: translateY(-20px) rotate(10deg); opacity: 1; }
            100% { transform: translateY(0) rotate(0deg); opacity: 0.7; }
        }
    </style>
</head>
<body>
    <!-- Hello Kitty装饰元素 -->
    <div class="kitty-decoration top-left">🐱</div>
    <div class="kitty-decoration top-right">🐱</div>
    <div class="kitty-decoration bottom-left">🐱</div>
    <div class="kitty-decoration bottom-right">🐱</div>
    
    <!-- 粉色爱心动画 -->
    <div class="floating-hearts" id="floatingHearts"></div>

    <!-- 音乐播放器 -->
    <div class="music-player">
        <div class="music-info">
            <div class="song-info" id="currentSong">Hello Kitty音乐播放中...</div>
            <div class="volume-control">
                <span style="color: #ff66b2;">🔊</span>
                <input type="range" min="0" max="1" step="0.1" value="0.7" class="volume-slider" id="volumeSlider">
            </div>
        </div>
        <div class="music-controls">
            <button class="music-btn" id="prevBtn" title="上一首">⏮</button>
            <button class="music-btn" id="playBtn" title="播放/暂停">▶</button>
            <button class="music-btn" id="nextBtn" title="下一首">⏭</button>
        </div>
    </div>

    <div class="container">
        <!-- Hello Kitty蝴蝶结 -->
        <div class="bow">🎀</div>
        
        <!-- Hello Kitty头像 -->
        <div class="kitty-face"></div>
        
        <div class="title">慧の业绩计算器</div>

        <!-- 输入区域 -->
        <div class="form-group">
            <label for="date">📅 输入今天的日期喵~（例：10.26）</label>
            <input type="text" id="date" placeholder="请输入今日日期" required>
            <div class="validation-message">请输入有效日期</div>
        </div>

        <div class="form-group">
            <label for="name">👤 是小慧！</label>
            <input type="text" id="name" placeholder="记得输入小慧喵~" required>
            <div class="validation-message">请输入姓名</div>
        </div>

        <div class="form-group">
            <label>⏰ 休息时长设置</label>
            <div class="radio-group">
                <div class="radio-item">
                    <input type="radio" name="restType" id="restShort" checked>
                    <label for="restShort">两次打卡选这个慧慧</label>
                </div>
                <div class="radio-item">
                    <input type="radio" name="restType" id="restLong">
                    <label for="restLong">四次打卡选这个慧慧</label>
                </div>
            </div>
        </div>

        <div class="form-group">
            <label for="workStart">💼 慧の上班业绩（实收）</label>
            <input type="number" id="workStart" placeholder="请输入上班时实收业绩" step="0.01" min="0" required>
            <div class="validation-message">请输入有效金额</div>
        </div>

        <div class="form-group" id="restOutGroup" style="display: none;">
            <label for="restOut">🍰 慧慧可以去休息啦！（去休息业绩）</label>
            <input type="number" id="restOut" placeholder="请输入去休息时实收业绩" step="0.01" min="0">
        </div>

        <div class="form-group" id="restInGroup" style="display: none;">
            <label for="restIn">🎀 慧慧休息回来啦！我又可以见到慧慧了ovo（休息回来业绩实收）</label>
            <input type="number" id="restIn" placeholder="请输入休息回来时实收业绩" step="0.01" min="0">
        </div>

        <div class="form-group">
            <label for="workEnd">🏠 慧慧の下班业绩（好好休息、别感冒）</label>
            <input type="number" id="workEnd" placeholder="请输入下班时实收业绩" step="0.01" min="0" required>
            <div class="validation-message">请输入有效金额</div>
        </div>

        <button onclick="calculatePerformance()">✨ 计算今日业绩</button>

        <!-- 结果展示区域 -->
        <div class="result-card" id="resultCard" style="display: none;">
            <div class="result-title">📊 业绩结算结果</div>
            <div class="result-item">
                <span class="result-label">日期：</span>
                <span class="result-value" id="resultDate">-</span>
            </div>
            <div class="result-item">
                <span class="result-label">姓名：</span>
                <span class="result-value" id="resultName">-</span>
            </div>
            <div class="result-item">
                <span class="result-label">上班业绩：</span>
                <span class="result-value" id="resultStart">-</span>
            </div>
            <div class="result-item" id="resultRestOut" style="display: none;">
                <span class="result-label">去休息业绩：</span>
                <span class="result-value">-</span>
            </div>
            <div class="result-item" id="resultRestIn" style="display: none;">
                <span class="result-label">休息回来业绩：</span>
                <span class="result-value">-</span>
            </div>
            <div class="result-item">
                <span class="result-label">下班业绩：</span>
                <span class="result-value" id="resultEnd">-</span>
            </div>
            <div class="result-item" style="border-bottom: none; margin-top: 20px;">
                <span class="result-label">在岗总业绩：</span>
                <span class="result-value" id="totalPerformance" style="font-size: 18px;">-</span>
            </div>

            <div class="calc-process" id="calcProcess">
                <div class="process-title">📝 计算过程：</div>
                <div id="processContent"></div>
            </div>
        </div>

        <div class="tips">
            💖 提示：业绩要填写实收金额哟慧慧，功将根据数据实时计算；输错了重新输入即可，这个暂时还没有数据存储功能，就是得每天算，不能看以前算过的
        </div>
    </div>

    <script>
        // 休息类型切换显示
        document.getElementById('restShort').addEventListener('change', function() {
            document.getElementById('restOutGroup').style.display = 'none';
            document.getElementById('restInGroup').style.display = 'none';
            document.getElementById('resultRestOut').style.display = 'none';
            document.getElementById('resultRestIn').style.display = 'none';
        });

        document.getElementById('restLong').addEventListener('change', function() {
            document.getElementById('restOutGroup').style.display = 'block';
            document.getElementById('restInGroup').style.display = 'block';
            document.getElementById('resultRestOut').style.display = 'flex';
            document.getElementById('resultRestIn').style.display = 'flex';
        });

        // 计算业绩核心函数
        function calculatePerformance() {
            // 获取输入值
            const date = document.getElementById('date').value.trim();
            const name = document.getElementById('name').value.trim();
            const workStart = parseFloat(document.getElementById('workStart').value) || 0;
            const workEnd = parseFloat(document.getElementById('workEnd').value) || 0;
            const restOut = parseFloat(document.getElementById('restOut').value) || 0;
            const restIn = parseFloat(document.getElementById('restIn').value) || 0;
            const isLongRest = document.getElementById('restLong').checked;

            // 基础校验
            if (!date || !name) {
                alert('请填写日期和名字哟慧慧');
                return;
            }
            if (isLongRest && (isNaN(restOut) || isNaN(restIn))) {
                alert('休息＞40分钟时，需填写去休息和休息回来的业绩！');
                return;
            }

            // 计算总业绩
            let total = 0;
            let processHtml = '';

            if (isLongRest) {
                // 长休息计算逻辑
                const restPerformance = restIn - restOut;
                total = (workEnd - workStart - restPerformance).toFixed(2);
                processHtml = `
                    <div class="process-step">1. 休息期间业绩：休息回来业绩 - 去休息业绩 = ${restIn.toFixed(2)} - ${restOut.toFixed(2)} = ${restPerformance.toFixed(2)}</div>
                    <div class="process-step">2. 在岗总业绩 = 下班业绩 - 上班业绩 - 休息期间业绩</div>
                    <div class="process-step">3. 代入计算：${workEnd.toFixed(2)} - ${workStart.toFixed(2)} - ${restPerformance.toFixed(2)} = ${total}</div>
                `;
                // 填充结果
                document.querySelector('#resultRestOut .result-value').textContent = restOut.toFixed(2);
                document.querySelector('#resultRestIn .result-value').textContent = restIn.toFixed(2);
            } else {
                // 短休息计算逻辑
                total = (workEnd - workStart).toFixed(2);
                processHtml = `
                    <div class="process-step">在岗总业绩 = 下班业绩 - 上班业绩</div>
                    <div class="process-step">代入计算：${workEnd.toFixed(2)} - ${workStart.toFixed(2)} = ${total}</div>
                `;
            }

            // 填充结果区域
            document.getElementById('resultDate').textContent = date;
            document.getElementById('resultName').textContent = name;
            document.getElementById('resultStart').textContent = workStart.toFixed(2);
            document.getElementById('resultEnd').textContent = workEnd.toFixed(2);
            document.getElementById('totalPerformance').textContent = total;
            document.getElementById('processContent').innerHTML = processHtml;

            // 显示结果卡片
            document.getElementById('resultCard').style.display = 'block';
            
            // 添加庆祝效果
            createHearts(10);
        }

        // 音乐播放器功能
        const musicList = [
            { name: "恋人", url: "http://t5eufjc1c.hn-bkt.clouddn.com/%E6%81%8B%E4%BA%BA.mp3" },
            { name: "须尽欢", url: "http://t5eufjc1c.hn-bkt.clouddn.com/%E9%A1%BB%E5%B0%BD%E6%AC%A2.mp3" },
            { name: "鸟之诗", url: "http://t5eufjc1c.hn-bkt.clouddn.com/%E4%BD%A0%E4%B8%8D%E8%A6%81%E5%86%8D%E5%90%B9%E4%BA%86%EF%BC%81.mp3" },
            { name: "悄悄做个梦给你", url: "http://t5eufjc1c.hn-bkt.clouddn.com/%E6%82%84%E6%82%84%E5%81%9A%E4%B8%AA%E6%A2%A6%E7%BB%99%E4%BD%A0.mp3" }
        ];

        const audioPlayer = new Audio();
        let currentTrackIndex = 0;
        let isPlaying = false;

        // 随机选择一首歌
        function randomTrack() {
            currentTrackIndex = Math.floor(Math.random() * musicList.length);
            audioPlayer.src = musicList[currentTrackIndex].url;
            document.getElementById('currentSong').textContent = musicList[currentTrackIndex].name;
        }

        // 播放/暂停
        document.getElementById('playBtn').addEventListener('click', function() {
            if (isPlaying) {
                audioPlayer.pause();
                this.innerHTML = '▶';
            } else {
                if (!audioPlayer.src) randomTrack();
                audioPlayer.play();
                this.innerHTML = '⏸';
            }
            isPlaying = !isPlaying;
        });

        // 下一首
        document.getElementById('nextBtn').addEventListener('click', function() {
            currentTrackIndex = (currentTrackIndex + 1) % musicList.length;
            audioPlayer.src = musicList[currentTrackIndex].url;
            document.getElementById('currentSong').textContent = musicList[currentTrackIndex].name;
            if (isPlaying) {
                audioPlayer.play();
            }
        });

        // 上一首
        document.getElementById('prevBtn').addEventListener('click', function() {
            currentTrackIndex = (currentTrackIndex - 1 + musicList.length) % musicList.length;
            audioPlayer.src = musicList[currentTrackIndex].url;
            document.getElementById('currentSong').textContent = musicList[currentTrackIndex].name;
            if (isPlaying) {
                audioPlayer.play();
            }
        });

        // 音量控制
        document.getElementById('volumeSlider').addEventListener('input', function() {
            audioPlayer.volume = this.value;
        });

        // 歌曲结束时自动播放下一首
        audioPlayer.addEventListener('ended', function() {
            document.getElementById('nextBtn').click();
        });

        // 页面加载时随机选择一首歌
        window.addEventListener('load', function() {
            randomTrack();
            // 设置初始音量
            audioPlayer.volume = document.getElementById('volumeSlider').value;
            
            // 创建漂浮爱心
            createHearts(15);
        });

        // 创建漂浮爱心
        function createHearts(count) {
            const container = document.getElementById('floatingHearts');
            for (let i = 0; i < count; i++) {
                const heart = document.createElement('div');
                heart.className = 'heart';
                heart.innerHTML = '💖';
                heart.style.left = Math.random() * 100 + 'vw';
                heart.style.animationDelay = Math.random() * 5 + 's';
                heart.style.fontSize = (Math.random() * 20 + 15) + 'px';
                container.appendChild(heart);
                
                // 移除爱心元素
                setTimeout(() => {
                    heart.remove();
                }, 6000);
            }
        }
    </script>
</body>
</html>
