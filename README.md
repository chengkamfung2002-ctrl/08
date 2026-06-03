<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Circle | 智能自学账户中心</title>
    <style>
        :root {
            --bg-base: #f1f5f9;
            --bg-card: #ffffff;
            --text-main: #0f172a;
            --text-muted: #64748b;
            --primary: #6366f1;
            --primary-hover: #4f46e5;
            --success: #10b981;
            --border: #e2e8f0;
            --shadow: 0 4px 6px -1px rgba(0,0,0,0.05), 0 2px 4px -1px rgba(0,0,0,0.03);
            --radius-lg: 16px;
            --radius-md: 8px;
        }
        [data-theme="dark"] {
            --bg-base: #0f172a;
            --bg-card: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --primary: #818cf8;
            --primary-hover: #6366f1;
            --border: #334155;
        }
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: system-ui, -apple-system, sans-serif;
            transition: background-color 0.3s, border-color 0.3s;
        }
        body {
            background-color: var(--bg-base);
            color: var(--text-main);
            display: flex;
            min-height: 100vh;
        }
        aside {
            width: 260px;
            background-color: var(--bg-card);
            border-right: 1px solid var(--border);
            padding: 2rem 1.5rem;
            display: flex;
            flex-direction: column;
            position: fixed;
            height: 100vh;
        }
        .brand {
            font-size: 1.35rem;
            font-weight: 800;
            color: var(--primary);
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        .nav-item {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            padding: 0.75rem 1rem;
            border-radius: var(--radius-md);
            cursor: pointer;
            color: var(--text-muted);
            font-weight: 600;
            background: transparent;
            border: none;
            width: 100%;
            text-align: left;
            font-size: 0.95rem;
            margin-bottom: 0.5rem;
        }
        .nav-item:hover, .nav-item.active {
            background-color: rgba(99, 102, 241, 0.1);
            color: var(--primary);
        }
        main {
            margin-left: 260px;
            flex: 1;
            padding: 3rem;
            max-width: 1000px;
            width: calc(100% - 260px);
        }
        .card {
            background-color: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: var(--radius-lg);
            padding: 2.5rem;
            box-shadow: var(--shadow);
            margin-bottom: 2rem;
        }
        .account-header {
            display: flex;
            align-items: center;
            gap: 2rem;
            border-bottom: 1px solid var(--border);
            padding-bottom: 2rem;
            margin-bottom: 2rem;
        }
        .avatar-circle {
            width: 90px;
            height: 90px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), #ec4899);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2.5rem;
            font-weight: 700;
            box-shadow: 0 4px 14px rgba(99, 102, 241, 0.4);
            cursor: pointer;
        }
        .profile-info h2 {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 0.25rem;
        }
        .badge-level {
            background-color: var(--primary);
            color: white;
            padding: 0.2rem 0.6rem;
            border-radius: 50px;
            font-size: 0.75rem;
            font-weight: 700;
        }
        .content-panel { display: none; }
        .content-panel.active { display: block; }
        .form-group { margin-bottom: 1.25rem; }
        .form-group label { display: block; margin-bottom: 0.5rem; font-weight: 600; font-size: 0.9rem; }
        .form-group input { width: 100%; padding: 0.75rem 1rem; border-radius: var(--radius-md); border: 1px solid var(--border); background-color: var(--bg-base); color: var(--text-main); }
        .timer-display { font-size: 4rem; font-weight: 800; text-align: center; margin: 1.5rem 0; font-family: monospace; color: var(--primary); }
        .flex-center { display: flex; justify-content: center; gap: 1rem; }
        .btn { background-color: var(--primary); color: white; border: none; padding: 0.65rem 1.25rem; border-radius: var(--radius-md); font-weight: 600; cursor: pointer; }
        .btn:hover { background-color: var(--primary-hover); }
        .todo-item { display: flex; justify-content: space-between; align-items: center; padding: 0.75rem 1rem; background-color: var(--bg-base); border-radius: var(--radius-md); margin-top: 0.75rem; }
    </style>
</head>
<body>
    <aside>
        <div class="brand">🎓 Study Circle</div>
        <button class="nav-item active" onclick="switchPanel('account')">👤 <span>账户主页</span></button>
        <button class="nav-item" onclick="switchPanel('tasks')">📝 <span>学习清单</span></button>
        <button class="nav-item" onclick="switchPanel('timer')">⏱️ <span>专注时钟</span></button>
        <button class="nav-item" style="margin-top: auto;" onclick="toggleTheme()">🌗 <span>切换主题</span></button>
    </aside>
    <main>
        <div class="card">
            <div class="account-header">
                <div class="avatar-circle" id="avatar-view" onclick="changeEmoji()">🎯</div>
                <div class="profile-info">
                    <h2 id="username-view">极客学者</h2>
                    <p style="color: var(--text-muted); margin-bottom: 0.5rem;">UID: 20260603 | 欢迎回到自学圈</p>
                    <span class="badge-level">🏆 钻石自律星</span>
                </div>
            </div>
        </div>
        <div id="account" class="content-panel active">
            <div class="card">
                <h3 style="margin-bottom: 1.5rem;">⚙️ 账户信息修改</h3>
                <div class="form-group">
                    <label>更改您的学习昵称</label>
                    <input type="text" id="username-input" value="极客学者" oninput="updateProfile()">
                </div>
                <p style="color: var(--text-muted); font-size: 0.9rem;">提示：点击上方的圆形头像框，可以随机切换属于你的今日学习状态图标！</p>
            </div>
        </div>
        <div id="tasks" class="content-panel">
            <div class="card">
                <h3 style="margin-bottom: 1.25rem;">📝 今日学习任务卡</h3>
                <div style="display: flex; gap: 0.5rem; margin-bottom: 1.5rem;">
                    <input type="text" id="todo-input" placeholder="输入你想完成的学习任务..." style="flex: 1; padding: 0.75rem; border-radius: var(--radius-md); border: 1px solid var(--border); background-color: var(--bg-base); color: var(--text-main);">
                    <button class="btn" onclick="addTodo()">添加</button>
                </div>
                <div id="todo-list">
                    <div class="todo-item">
                        <span>📖 阅读商业策略导论 30 页</span>
                        <button class="btn" style="background-color:#ef4444; padding: 0.4rem 0.8rem;" onclick="this.parentElement.remove()">完成</button>
                    </div>
                </div>
            </div>
        </div>
        <div id="timer" class="content-panel">
            <div class="card" style="text-align: center;">
                <h3>⏱️ 艾宾浩斯深度专注时钟</h3>
                <p style="color: var(--text-muted); margin-top: 0.25rem;">标准 25 分钟高效学习周期</p>
                <div class="timer-display" id="time-string">25:00</div>
                <div class="flex-center">
                    <button class="btn" id="start-btn" onclick="toggleTimer()">启动倒计时</button>
                    <button class="btn" style="background-color: var(--text-muted);" onclick="resetTimer()">重置</button>
                </div>
            </div>
        </div>
    </main>
    <script>
        function switchPanel(panelId) {
            document.querySelectorAll('.content-panel').forEach(p => p.classList.remove('active'));
            document.getElementById(panelId).classList.add('active');
            document.querySelectorAll('.nav-item').forEach(btn => {
                btn.classList.remove('active');
                if(btn.getAttribute('onclick').includes(panelId)) btn.classList.add('active');
            });
        }
        function updateProfile() {
            const inputVal = document.getElementById('username-input').value;
            document.getElementById('username-view').innerText = inputVal || '匿名学者';
        }
        const emojis = ['🎯', '🚀', '🧠', '📚', '💻', '💡', '🔥', '🌟', '📝'];
        function changeEmoji() {
            const randomEmoji = emojis[Math.floor(Math.random() * emojis.length)];
            document.getElementById('avatar-view').innerText = randomEmoji;
        }
        function addTodo() {
            const input = document.getElementById('todo-input');
            if(!input.value.trim()) return;
            const list = document.getElementById('todo-list');
            const item = document.createElement('div');
            item.className = 'todo-item';
            item.innerHTML = `<span>${input.value}</span><button class="btn" style="background-color:#ef4444; padding: 0.4rem 0.8rem;" onclick="this.parentElement.remove()">完成</button>`;
            list.appendChild(item);
            input.value = '';
        }
        let timer = null; let timeLeft = 25 * 60; let isRunning = false;
        function toggleTimer() {
            const btn = document.getElementById('start-btn');
            if(isRunning) {
                clearInterval(timer); btn.innerText = '恢复倒计时'; isRunning = false;
            } else {
                isRunning = true; btn.innerText = '暂停专注';
                timer = setInterval(() => {
                    if(timeLeft > 0) { timeLeft--; updateTimerUI(); } else { clearInterval(timer); alert('🎉 太棒了！您已成功完成一个专注周期！'); resetTimer(); }
                }, 1000);
            }
        }
        function resetTimer() { clearInterval(timer); timeLeft = 25 * 60; isRunning = false; document.getElementById('start-btn').innerText = '启动倒计时'; updateTimerUI(); }
        function updateTimerUI() {
            const minutes = Math.floor(timeLeft / 60).toString().padStart(2, '0');
            const seconds = (timeLeft % 60).toString().padStart(2, '0');
            document.getElementById('time-string').innerText = `${minutes}:${seconds}`;
        }
        function toggleTheme() {
            const doc = document.documentElement;
            if(doc.getAttribute('data-theme') === 'dark') { doc.removeAttribute('data-theme'); } else { doc.setAttribute('data-theme', 'dark'); }
        }
    </script>
</body>
</html>
