<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="theme-color" content="#0a0a14">
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='0.9em' font-size='90'>⚔️</text></svg>">
    <title>Life RPG v2.6</title>
    <style>
        :root {
            --bg: #0a0a14;
            --card: #111122;
            --border: #1e1e3a;
            --text: #e0e0e0;
            --subtext: #888;
            --gold: #ffd700;
            --accent: #3d5afe;
            --xp-fill: #ffd700, #ff8c00;
            --water-bg: #1a1a30;
            --water-fill: #1565c0;
        }
        .theme-light { --bg: #f0f0f5; --card: #ffffff; --border: #ddd; --text: #222; --subtext: #666; --gold: #c79100; --accent: #3d5afe; --water-bg: #e0e0e0; --water-fill: #42a5f5; }
        .theme-forest { --bg: #0a1a0a; --card: #112211; --border: #1e3a1e; --text: #d0e0d0; --subtext: #6a8a6a; --gold: #7fff00; --accent: #2e7d32; --water-bg: #1a2a1a; --water-fill: #2e7d32; }
        .theme-fire { --bg: #1a0a0a; --card: #221111; --border: #3a1e1e; --text: #e0d0d0; --subtext: #996666; --gold: #ff4500; --accent: #c62828; --water-bg: #2a1a1a; --water-fill: #c62828; }
        .theme-ocean { --bg: #0a0a1a; --card: #111122; --border: #1a2a4a; --text: #c0d0e0; --subtext: #6688aa; --gold: #00bcd4; --accent: #0288d1; --water-bg: #0d1b2a; --water-fill: #0288d1; }
        .theme-sakura { --bg: #1a0f14; --card: #221118; --border: #3a1a2a; --text: #e0d0d8; --subtext: #996678; --gold: #ff80ab; --accent: #c2185b; --water-bg: #2a1a22; --water-fill: #c2185b; }
        .theme-minimal { --bg: #111; --card: #1a1a1a; --border: #333; --text: #ccc; --subtext: #777; --gold: #fff; --accent: #666; --water-bg: #1a1a1a; --water-fill: #888; }
        .theme-retro { --bg: #0a0a00; --card: #111100; --border: #333300; --text: #ccff00; --subtext: #888800; --gold: #ffcc00; --accent: #ff6600; --water-bg: #1a1a00; --water-fill: #ffcc00; }

        *{margin:0;padding:0;box-sizing:border-box}
        body{
            font-family:'Segoe UI',system-ui,-apple-system,sans-serif;
            background:var(--bg);
            color:var(--text);
            min-height:100vh;
            padding:6px;
            -webkit-tap-highlight-color:transparent;
            user-select:none;
            -webkit-user-select:none;
            transition:background 0.4s,color 0.4s;
        }
        .app{max-width:500px;margin:0 auto;padding-bottom:70px}
        h1{text-align:center;font-size:19px;margin:4px 0;color:var(--gold)}
        .subtitle{text-align:center;font-size:9px;color:var(--subtext);margin-bottom:8px}

        .settings-bar{display:flex;justify-content:center;gap:5px;margin-bottom:6px;flex-wrap:wrap}
        .theme-dot{width:22px;height:22px;border-radius:50%;cursor:pointer;border:2px solid transparent;transition:all 0.2s}
        .theme-dot.active{border-color:var(--gold);transform:scale(1.2)}
        .theme-dot.dark{background:#0a0a14}
        .theme-dot.light{background:#f0f0f5}
        .theme-dot.forest{background:#0a1a0a}
        .theme-dot.fire{background:#1a0a0a}
        .theme-dot.ocean{background:#0a0a1a}
        .theme-dot.sakura{background:#1a0f14}
        .theme-dot.minimal{background:#111}
        .theme-dot.retro{background:#0a0a00}

        .level-card{
            background:var(--card);border-radius:16px;padding:12px;margin-bottom:8px;
            text-align:center;border:1px solid var(--border);
        }
        .big-level{font-size:34px;font-weight:900;color:var(--gold)}
        .big-title{font-size:9px;color:var(--subtext);letter-spacing:2px;text-transform:uppercase}
        .xp-bar-bg{background:var(--water-bg);border-radius:20px;height:12px;margin:6px 0 4px;overflow:hidden;border:1px solid var(--border)}
        .xp-bar-fill{height:100%;background:linear-gradient(90deg,var(--gold),#ff8c00);border-radius:20px;transition:width 0.4s}
        .xp-text{font-size:9px;color:var(--subtext)}

        .stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-bottom:10px}
        .stat-card{
            background:var(--card);border-radius:12px;padding:10px 6px;
            text-align:center;border:1px solid var(--border);
        }
        .stat-icon{font-size:24px}
        .stat-name{font-size:9px;color:var(--subtext);text-transform:uppercase}
        .stat-level{font-weight:700;color:var(--gold);font-size:13px}
        .stat-xp{font-size:8px;color:var(--subtext)}
        .stat-card .xp-bar-bg{height:6px;margin:3px 0 0}

        .section-title{
            font-size:10px;font-weight:700;color:var(--subtext);margin:12px 0 5px;
            text-transform:uppercase;letter-spacing:2px;
            display:flex;align-items:center;gap:4px;
        }

        .task{
            background:var(--card);border-radius:12px;padding:10px 12px;
            margin-bottom:5px;display:flex;align-items:center;
            justify-content:space-between;border:1px solid var(--border);
            transition:all 0.2s;
        }
        .task.completed{opacity:0.5;border-color:#1a3a1a}
        .task-info{flex:1;min-width:0;padding-right:6px}
        .task-name{font-size:11px;font-weight:500;line-height:1.2}
        .task-reward{font-size:8px;color:var(--gold);margin-top:1px}
        .task-counter{font-size:9px;color:#4fc3f7;margin-top:1px}

        .btn{
            padding:7px 10px;border-radius:20px;border:none;
            font-weight:700;font-size:10px;cursor:pointer;
            transition:all 0.15s;white-space:nowrap;
            min-width:36px;text-align:center;margin-left:4px;
        }
        .btn:active{transform:scale(0.92)}
        .btn-done{background:#1a5c2a;color:#fff;border:1px solid #2e7d32}
        .btn-undo{background:#444;color:#aaa;border:1px solid #555}
        .btn-over{background:#1a3a5c;color:#fff;border:1px solid #1565c0}
        .btn-refresh{background:#5d3a1a;color:#fff;border:1px solid #8d6e2a;font-size:9px;padding:6px 10px}

        .water-section{background:var(--card);border-radius:12px;padding:12px;margin:6px 0;border:1px solid var(--border)}
        .water-title{font-size:12px;font-weight:600;margin-bottom:6px;display:flex;justify-content:space-between;align-items:center}
        .water-glasses{display:flex;gap:4px;flex-wrap:wrap;justify-content:center;margin:8px 0}
        .glass{
            width:34px;height:42px;border-radius:6px 6px 12px 12px;
            background:var(--water-bg);border:2px solid var(--border);
            display:flex;align-items:flex-end;justify-content:center;
            font-size:9px;color:var(--subtext);cursor:pointer;
            transition:all 0.2s;
        }
        .glass.filled{background:var(--water-fill);border-color:var(--gold);color:#fff}

        .daily-bonus{background:var(--card);border:1px solid #3d2a6e}

        .sleep-section{background:var(--card);border-radius:12px;padding:12px;margin:8px 0;text-align:center;border:1px solid var(--border)}
        .sleep-status{font-size:12px;margin-bottom:6px}
        .btn-sleep,.btn-wake{width:100%;padding:10px;font-size:13px;border-radius:25px;font-weight:700;border:none;cursor:pointer}
        .btn-sleep{background:linear-gradient(135deg,#1a237e,#283593);color:#fff}
        .btn-wake{background:linear-gradient(135deg,#e65100,#ff8f00);color:#fff}
        .penalty-warn{color:#ff5252;font-size:9px;margin-top:4px}

        .tab-bar{display:flex;gap:3px;margin-bottom:8px;flex-wrap:wrap}
        .tab{
            flex:1;min-width:50px;padding:6px 4px;text-align:center;border-radius:15px;
            font-size:9px;font-weight:600;cursor:pointer;
            background:var(--card);border:1px solid var(--border);color:var(--subtext);
        }
        .tab.active{background:var(--accent);border-color:var(--accent);color:#fff}

        .modal-overlay{
            position:fixed;top:0;left:0;right:0;bottom:0;
            background:rgba(0,0,0,0.85);z-index:100;
            display:flex;align-items:center;justify-content:center;
            padding:15px;
        }
        .modal{
            background:var(--card);border-radius:16px;padding:16px;
            max-width:420px;width:100%;max-height:80vh;
            overflow-y:auto;border:1px solid var(--border);
        }
        .modal h3{color:var(--gold);margin-bottom:10px;font-size:15px}
        .modal input,.modal select{
            width:100%;padding:8px;margin:4px 0;border-radius:8px;
            border:1px solid var(--border);background:var(--bg);color:var(--text);font-size:12px;
        }
        .modal-btns{text-align:center;margin-top:10px}

        .calendar-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;margin:6px 0}
        .cal-day{
            aspect-ratio:1;border-radius:6px;display:flex;
            align-items:center;justify-content:center;
            font-size:10px;font-weight:600;cursor:pointer;
            background:var(--card);border:1px solid var(--border);
        }
        .cal-day.today{border:2px solid var(--gold)}
        .cal-day.green{background:#1a3a1a;border-color:#2e7d32}
        .cal-day.yellow{background:#3a3510;border-color:#f9a825}
        .cal-day.red{background:#3a1010;border-color:#c62828}
        .cal-header{text-align:center;font-size:9px;color:var(--subtext);padding:3px 0}

        .achievement{
            background:var(--card);border-radius:10px;padding:8px 12px;margin:4px 0;
            border:1px solid var(--border);display:flex;align-items:center;gap:8px;
        }
        .ach-icon{font-size:24px}
        .ach-info{flex:1}
        .ach-name{font-size:11px;font-weight:600;color:var(--gold)}
        .ach-desc{font-size:9px;color:var(--subtext)}
        .ach-locked{opacity:0.35;filter:grayscale(1)}

        /* Паутина */
        .web-container{position:relative;width:100%;max-width:300px;height:300px;margin:10px auto}
        .web-node{
            position:absolute;width:30px;height:30px;border-radius:50%;
            background:var(--card);border:2px solid var(--border);
            display:flex;align-items:center;justify-content:center;
            font-size:16px;transform:translate(-50%,-50%);
            transition:all 0.3s;
        }
        .web-node.unlocked{background:var(--accent);border-color:var(--gold);box-shadow:0 0 10px var(--gold)}
        .web-branch{position:absolute;height:2px;background:var(--border);transform-origin:left center;transition:all 0.3s}
        .web-branch.active{background:var(--gold)}

        .xp-popup{
            position:fixed;pointer-events:none;font-weight:900;
            font-size:18px;color:var(--gold);text-shadow:0 0 10px var(--gold);
            animation:floatUp 1s ease-out forwards;z-index:200;
        }
        @keyframes floatUp{
            0%{opacity:1;transform:translateY(0) scale(1)}
            100%{opacity:0;transform:translateY(-50px) scale(1.3)}
        }

        .btn-add{background:#6a1b9a;color:#fff;width:100%;padding:8px;border-radius:20px;font-size:11px;margin:4px 0}
        .streak{text-align:center;font-size:11px;color:var(--gold);margin:3px 0}

        .timer-display{font-size:36px;font-weight:900;text-align:center;color:var(--gold);margin:10px 0}
        .timer-btns{display:flex;gap:6px;justify-content:center;flex-wrap:wrap}
    </style>
</head>
<body>
    <div class="app" id="app"></div>
    <script>
        // ==================== LIFE RPG v2.6 ====================
        let data = JSON.parse(localStorage.getItem('lifeRpgv26')) || {
            health: { xp: 0, level: 1 },
            sport: { xp: 0, level: 1 },
            mind: { xp: 0, level: 1 },
            hygiene: { xp: 0, level: 1 },
            globalXp: 0,
            globalLevel: 1,
            sleepState: null,
            sleepDate: null,
            sleepDay: '',
            penaltyApplied: false,
            waterCount: 0,
            waterDone: false,
            waterOverBonus: 0,
            todayMandatory: {},
            todayDaily: {},
            customTasks: [],
            history: {},
            achievements: [],
            rewardsEarned: [],
            dailyTasksDate: '',
            streak: 0,
            lastActiveDate: '',
            theme: 'dark',
            dailyRefreshUsed: false,
        };

        const XP_PER_LEVEL = 100;
        const GLOBAL_XP_PER_LEVEL = 400;
        const WATER_GOAL = 1500;
        const GLASS_SIZE = 250;

        const mandatoryTasksDef = [
            { id: 'abs', name: '🏋️ Тренировка на пресс', reward: 25, stat: 'sport', maxOver: 3 },
            { id: 'hygiene', name: '🪥 Чистка зубов + умывание', reward: 20, stat: 'hygiene', maxOver: 2 },
        ];

        const dailyTasksPool = [
            // Здоровье
            { name: '🥗 Съесть овощной салат', reward: 25, stat: 'health', maxOver: 2 },
            { name: '🍬 День без сладкого', reward: 35, stat: 'health', maxOver: 1 },
            { name: '🍵 Выпить зелёный чай', reward: 15, stat: 'health', maxOver: 4 },
            { name: '🍎 Съесть 2 фрукта', reward: 15, stat: 'health', maxOver: 3 },
            { name: '💊 Принять витамины', reward: 15, stat: 'health', maxOver: 2 },
            { name: '🥜 Горсть орехов', reward: 10, stat: 'health', maxOver: 3 },
            { name: '🥤 Смузи/фреш', reward: 20, stat: 'health', maxOver: 2 },
            { name: '🚫 Без фастфуда', reward: 30, stat: 'health', maxOver: 1 },
            { name: '🥑 Полезный завтрак', reward: 20, stat: 'health', maxOver: 2 },
            { name: '🍵 Травяной чай', reward: 10, stat: 'health', maxOver: 4 },
            // Спорт
            { name: '⏱️ Планка 1 мин', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🚶 Прогулка 20 мин', reward: 30, stat: 'sport', maxOver: 2 },
            { name: '🤸 Растяжка 10 мин', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🦵 50 приседаний', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '☀️ Зарядка 10 мин', reward: 20, stat: 'sport', maxOver: 2 },
            { name: '🧘‍♂️ 10000 шагов', reward: 35, stat: 'sport', maxOver: 1 },
            { name: '💪 Отжимания 20 раз', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🕺 Танцы 10 мин', reward: 25, stat: 'sport', maxOver: 2 },
            { name: '🧗 Подъём по лестнице', reward: 20, stat: 'sport', maxOver: 3 },
            { name: '🏃 Пробежка 15 мин', reward: 30, stat: 'sport', maxOver: 2 },
            // Самопознание
            { name: '📖 Читать 15 страниц', reward: 25, stat: 'mind', maxOver: 3 },
            { name: '🧘 Медитация 5 мин', reward: 20, stat: 'mind', maxOver: 3 },
            { name: '🙏 3 благодарности', reward: 20, stat: 'mind', maxOver: 2 },
            { name: '📝 Дневник мыслей', reward: 20, stat: 'mind', maxOver: 2 },
            { name: '📵 Час без телефона', reward: 25, stat: 'mind', maxOver: 3 },
            { name: '🫁 Дыхательная практика', reward: 20, stat: 'mind', maxOver: 3 },
            { name: '📚 Учить новое слово', reward: 15, stat: 'mind', maxOver: 5 },
            { name: '🎧 Подкаст 20 мин', reward: 20, stat: 'mind', maxOver: 2 },
            { name: '📿 Аффирмации', reward: 20, stat: 'mind', maxOver: 3 },
            { name: '🧩 Головоломка', reward: 15, stat: 'mind', maxOver: 3 },
            // Гигиена
            { name: '🚿 Контрастный душ', reward: 30, stat: 'hygiene', maxOver: 2 },
            { name: '🧴 Маска для лица', reward: 20, stat: 'hygiene', maxOver: 2 },
            { name: '🛏️ Сменить бельё', reward: 25, stat: 'hygiene', maxOver: 1 },
            { name: '🧹 Уборка 15 мин', reward: 20, stat: 'hygiene', maxOver: 2 },
            { name: '🧼 Скраб для лица', reward: 20, stat: 'hygiene', maxOver: 1 },
            { name: '🧴 Увлажнить кожу', reward: 15, stat: 'hygiene', maxOver: 3 },
            { name: '🪥 Ирригатор/зубная нить', reward: 15, stat: 'hygiene', maxOver: 2 },
            { name: '👕 Сортировка одежды', reward: 15, stat: 'hygiene', maxOver: 2 },
            { name: '🪞 Уход за бровями', reward: 15, stat: 'hygiene', maxOver: 1 },
            { name: '🧽 Протереть пыль', reward: 15, stat: 'hygiene', maxOver: 2 },
        ];

        // Паутина достижений: 1,3,5,7,12,18,25,30
        const webDays = [1, 3, 5, 7, 12, 18, 25, 30];
        const webBranches = [
            { id: 'health_web', name: '❤️ Здоровье', stat: 'health', tasks: ['🥗 Овощной салат', '🍬 Без сладкого', '💊 Витамины', '🍎 2 фрукта'] },
            { id: 'sport_web', name: '💪 Спорт', stat: 'sport', tasks: ['🏋️ Пресс', '⏱️ Планка', '🦵 Приседания', '💪 Отжимания'] },
            { id: 'mind_web', name: '🧠 Самопознание', stat: 'mind', tasks: ['📖 Чтение', '🧘 Медитация', '📝 Дневник', '🙏 Благодарности'] },
            { id: 'hygiene_web', name: '✨ Гигиена', stat: 'hygiene', tasks: ['🪥 Зубы+умыв.', '🚿 Контраст.душ', '🧴 Маска', '🧹 Уборка'] },
            { id: 'custom_web', name: '✏️ Свои задания', stat: null, tasks: [] },
        ];

        function save() { localStorage.setItem('lifeRpgv26', JSON.stringify(data)); }
        function getToday() { return new Date().toDateString(); }
        function getDateKey(d) { return d || new Date().toISOString().split('T')[0]; }
        function getStatName(s) {
            return { health: '❤️ Здоровье', sport: '💪 Спорт', mind: '🧠 Самопознание', hygiene: '✨ Гигиена' }[s] || s;
        }

        function addXp(stat, amount) {
            if (!data[stat]) return;
            data[stat].xp += amount;
            data.globalXp += amount;
            while (data[stat].xp >= data[stat].level * XP_PER_LEVEL) {
                data[stat].xp -= data[stat].level * XP_PER_LEVEL;
                data[stat].level++;
            }
            while (data.globalXp >= data.globalLevel * GLOBAL_XP_PER_LEVEL) {
                data.globalXp -= data.globalLevel * GLOBAL_XP_PER_LEVEL;
                data.globalLevel++;
                grantReward(data.globalLevel % 5 === 0 ? 'big' : 'medium');
            }
            checkWebAchievements();
            save();
        }

        function spawnXpPopup(x, y, text) {
            const el = document.createElement('div');
            el.className = 'xp-popup';
            el.textContent = text;
            el.style.left = x + 'px';
            el.style.top = y + 'px';
            document.body.appendChild(el);
            setTimeout(() => el.remove(), 1000);
        }

        function generateDailyTasks() {
            let shuffled = [...dailyTasksPool].sort(() => Math.random() - 0.5);
            return shuffled.slice(0, 6).map((t, i) => ({ ...t, id: 'daily_' + i }));
        }

        function getCachedDailyTasks() {
            let cached = localStorage.getItem('dailyTasksCachev26');
            if (!cached) { cached = JSON.stringify(generateDailyTasks());
                localStorage.setItem('dailyTasksCachev26', cached); }
            return JSON.parse(cached);
        }

        function refreshDailyTasks() {
            if (data.dailyRefreshUsed) return;
            data.dailyRefreshUsed = true;
            localStorage.setItem('dailyTasksCachev26', JSON.stringify(generateDailyTasks()));
            save();
            render();
        }

        function checkNewDay() {
            const today = getToday();
            if (data.lastActiveDate && data.lastActiveDate !== today) {
                const yesterday = new Date();
                yesterday.setDate(yesterday.getDate() - 1);
                const yKey = yesterday.toISOString().split('T')[0];
                const yh = data.history[yKey];
                if (yh) {
                    const allDone = mandatoryTasksDef.every(t => (yh.mandatory[t.id] || 0) >= 1);
                    if (allDone && yh.water >= 6) data.streak++;
                    else data.streak = 0;
                } else data.streak = 0;
            }
            if (data.dailyTasksDate !== today) {
                if (data.dailyTasksDate) {
                    const oldKey = new Date(data.dailyTasksDate).toISOString().split('T')[0];
                    if (!data.history[oldKey]) data.history[oldKey] = { mandatory: { ...data.todayMandatory },
                        daily: { ...data.todayDaily }, water: data.waterCount, sleep: data.sleepState,
                        waterOverBonus: data.waterOverBonus };
                }
                data.dailyTasksDate = today;
                data.todayMandatory = {};
                data.todayDaily = {};
                data.waterCount = 0;
                data.waterDone = false;
                data.waterOverBonus = 0;
                data.penaltyApplied = false;
                data.sleepState = null;
                data.sleepDay = '';
                data.dailyRefreshUsed = false;
                localStorage.setItem('dailyTasksCachev26', JSON.stringify(generateDailyTasks()));
            }
            data.lastActiveDate = today;
            save();
        }

        function completeTask(taskId, isMandatory, btnElement) {
            if (isMandatory) {
                const task = mandatoryTasksDef.find(t => t.id === taskId);
                if (!task) return;
                const cur = data.todayMandatory[taskId] || 0;
                if (cur >= task.maxOver) return;
                data.todayMandatory[taskId] = cur + 1;
                addXp(task.stat, task.reward);
                const dk = getDateKey();
                if (!data.history[dk]) data.history[dk] = { mandatory: {}, daily: {}, water: 0, sleep: null,
                    waterOverBonus: 0 };
                data.history[dk].mandatory[taskId] = (data.history[dk].mandatory[taskId] || 0) + 1;
            } else {
                const tasks = getCachedDailyTasks();
                const task = tasks.find(t => t.id === taskId);
                if (!task) return;
                const cur = data.todayDaily[taskId] || 0;
                if (cur >= task.maxOver) return;
                data.todayDaily[taskId] = cur + 1;
                addXp(task.stat, task.reward);
                const dk = getDateKey();
                if (!data.history[dk]) data.history[dk] = { mandatory: {}, daily: {}, water: 0, sleep: null,
                    waterOverBonus: 0 };
                data.history[dk].daily[taskId] = (data.history[dk].daily[taskId] || 0) + 1;
            }
            if (btnElement) {
                const rect = btnElement.getBoundingClientRect();
                spawnXpPopup(rect.left + rect.width / 2 - 20, rect.top - 10, '+XP');
            }
            save();
            render();
        }

        function undoTask(taskId, isMandatory) {
            if (isMandatory) {
                const cur = data.todayMandatory[taskId] || 0;
                if (cur <= 0) return;
                const task = mandatoryTasksDef.find(t => t.id === taskId);
                if (!task) return;
                data.todayMandatory[taskId] = cur - 1;
                if (data.todayMandatory[taskId] <= 0) delete data.todayMandatory[taskId];
                addXp(task.stat, -task.reward);
            } else {
                const cur = data.todayDaily[taskId] || 0;
                if (cur <= 0) return;
                const tasks = getCachedDailyTasks();
                const task = tasks.find(t => t.id === taskId);
                if (!task) return;
                data.todayDaily[taskId] = cur - 1;
                if (data.todayDaily[taskId] <= 0) delete data.todayDaily[taskId];
                addXp(task.stat, -task.reward);
            }
            save();
            render();
        }

        function drinkGlass() {
            if (data.waterCount >= 10) return;
            data.waterCount++;
            if (data.waterCount >= 6 && !data.waterDone) { data.waterDone = true;
                addXp('health', 20); }
            if (data.waterCount > 6) { data.waterOverBonus += 5;
                addXp('health', 5); }
            const dk = getDateKey();
            if (!data.history[dk]) data.history[dk] = { mandatory: {}, daily: {}, water: 0, sleep: null,
                waterOverBonus: 0 };
            data.history[dk].water = data.waterCount;
            save();
            render();
        }

        function undoGlass() {
            if (data.waterCount <= 0) return;
            if (data.waterCount > 6) { data.waterOverBonus = Math.max(0, data.waterOverBonus - 5);
                addXp('health', -5); }
            if (data.waterCount === 6 && data.waterDone) { data.waterDone = false;
                addXp('health', -20); }
            data.waterCount--;
            const dk = getDateKey();
            if (data.history[dk]) data.history[dk].water = data.waterCount;
            save();
            render();
        }

        function goSleep() {
            data.sleepState = 'asleep';
            data.sleepDate = new Date().toISOString();
            data.sleepDay = getDateKey();
            data.penaltyApplied = false;
            const dk = getDateKey();
            if (!data.history[dk]) data.history[dk] = { mandatory: {}, daily: {}, water: 0, sleep: null,
                waterOverBonus: 0 };
            data.history[dk].sleep = 'asleep';
            save();
            render();
        }

        function wakeUp() {
            if (data.sleepState === 'asleep') {
                const oldKey = data.sleepDay || getDateKey();
                if (!data.history[oldKey]) data.history[oldKey] = { mandatory: { ...data.todayMandatory },
                    daily: { ...data.todayDaily }, water: data.waterCount, sleep: 'awake', waterOverBonus: data
                        .waterOverBonus };
                else data.history[oldKey].sleep = 'awake';
                data.todayMandatory = {};
                data.todayDaily = {};
                data.waterCount = 0;
                data.waterDone = false;
                data.waterOverBonus = 0;
                data.penaltyApplied = false;
                data.sleepState = 'awake';
                data.dailyTasksDate = getToday();
                data.sleepDay = '';
                data.dailyRefreshUsed = false;
                localStorage.setItem('dailyTasksCachev26', JSON.stringify(generateDailyTasks()));
                save();
                render();
            }
        }

        function checkSleepPenalty() {
            const h = new Date().getHours();
            if (h >= 2 && h < 8 && data.sleepState === null && !data.penaltyApplied) {
                for (let s of ['health', 'sport', 'mind', 'hygiene']) {
                    let p = Math.floor(data[s].xp * 0.1);
                    if (p > 0) { data[s].xp = Math.max(0, data[s].xp - p);
                        data.globalXp = Math.max(0, data.globalXp - p); }
                }
                data.penaltyApplied = true;
                save();
                render();
            }
        }

        function checkStreakWarning() {
            const h = new Date().getHours();
            if (h === 1 && data.streak >= 1 && data.sleepState === null) {
                const allDone = mandatoryTasksDef.every(t => (data.todayMandatory[t.id] || 0) >= 1);
                if (!allDone || data.waterCount < 6) {
                    if ('Notification' in window && Notification.permission === 'granted') {
                        new Notification('⚠️ Life RPG', { body: 'Стрик под угрозой! Выполни задания до 02:00!',
                            icon: '🔥' });
                    }
                }
            }
        }

        function grantReward(tier) {
            const pool = {
                small: ['🍫 Вкусняшка', '☕ Любимый напиток', '🎵 Альбом', '📺 Серия сериала'],
                medium: ['🍕 Доставка еды', '🚶 Прогулка', '📖 Книга', '🎬 Кино'],
                big: ['🎁 Покупка до 2000₽', '🧖 SPA-день', '🍔 Читмил', '🚴 Поездка'],
                legendary: ['✈️ Путешествие', '🎪 Концерт', '💻 Гаджет', '🏨 Отель']
            } [tier] || ['🎁 Награда'];
            const reward = pool[Math.floor(Math.random() * pool.length)];
            data.rewardsEarned.push({ date: getDateKey(), reward, tier });
            save();
            showRewardModal(reward, tier);
        }

        function showRewardModal(reward, tier) {
            const overlay = document.createElement('div');
            overlay.className = 'modal-overlay';
            overlay.innerHTML = `
                <div class="modal"><div style="text-align:center">
                    <div style="font-size:40px">🎁</div>
                    <div style="font-size:14px;color:var(--gold);margin:8px 0">${tier==='legendary'?'ЛЕГЕНДАРНАЯ':tier==='big'?'Крупная':'Средняя'} награда!</div>
                    <div style="font-size:16px;color:var(--text)">${reward}</div>
                    <button class="btn btn-done" style="margin-top:10px" onclick="this.closest('.modal-overlay').remove()">👍 Принято!</button>
                </div></div>`;
            document.body.appendChild(overlay);
            overlay.addEventListener('click', function(e) { if (e.target === overlay) overlay.remove(); });
        }

        function checkWebAchievements() {
            for (let branch of webBranches) {
                for (let d of webDays) {
                    const achId = `${branch.id}_${d}`;
                    if (data.achievements.includes(achId)) continue;
                    // Проверка: считать дни выполнения заданий этой ветки
                    let daysDone = 0;
                    let consecutive = 0;
                    let maxConsecutive = 0;
                    // Упрощённо: смотрим историю
                    const keys = Object.keys(data.history).sort();
                    for (let key of keys) {
                        const h = data.history[key];
                        let relevant = false;
                        if (branch.id === 'custom_web') {
                            relevant = h.daily && Object.keys(h.daily).some(k => k.startsWith('custom_'));
                        } else if (branch.stat) {
                            if (branch.id === 'health_web') relevant = (h.water || 0) >= 6;
                            else if (branch.id === 'sport_web') relevant = (h.mandatory['abs'] || 0) >= 1;
                            else if (branch.id === 'mind_web') relevant = (h.daily && Object.values(h.daily).some(v => v >
                                0));
                            else if (branch.id === 'hygiene_web') relevant = (h.mandatory['hygiene'] || 0) >= 1;
                        }
                        if (relevant) { consecutive++;
                            maxConsecutive = Math.max(maxConsecutive, consecutive); } else { consecutive = 0; }
                    }
                    if (maxConsecutive >= d) {
                        data.achievements.push(achId);
                        if (d >= 25) grantReward('legendary');
                        else if (d >= 18) grantReward('big');
                        else if (d >= 12) grantReward('medium');
                    }
                }
            }
            save();
        }

        function setTheme(theme) {
            data.theme = theme;
            document.body.className = theme === 'dark' ? '' : 'theme-' + theme;
            save();
            render();
        }

        // ============ ТАЙМЕР ============
        let timerInterval = null;
        let timerSeconds = 0;

        function startTimer(seconds) {
            stopTimer();
            timerSeconds = seconds;
            const overlay = document.createElement('div');
            overlay.className = 'modal-overlay';
            overlay.id = 'timerModal';
            overlay.innerHTML = `
                <div class="modal" style="text-align:center">
                    <h3>⏱️ Таймер медитации</h3>
                    <div class="timer-display" id="timerDisplay">${formatTime(timerSeconds)}</div>
                    <div class="timer-btns">
                        <button class="btn btn-undo" onclick="stopTimer();document.getElementById('timerModal').remove()">❌</button>
                        <button class="btn btn-done" id="timerPauseBtn" onclick="pauseTimer()">⏸️</button>
                    </div>
                </div>`;
            document.body.appendChild(overlay);
            timerInterval = setInterval(() => {
                timerSeconds--;
                const display = document.getElementById('timerDisplay');
                if (display) display.textContent = formatTime(timerSeconds);
                if (timerSeconds <= 0) {
                    stopTimer();
                    if ('vibrate' in navigator) navigator.vibrate([200, 100, 200]);
                    document.getElementById('timerModal')?.remove();
                    // Автозачёт медитации
                    const tasks = getCachedDailyTasks();
                    const medTask = tasks.find(t => t.name.includes('Медитация'));
                    if (medTask && (data.todayDaily[medTask.id] || 0) < medTask.maxOver) {
                        data.todayDaily[medTask.id] = (data.todayDaily[medTask.id] || 0) + 1;
                        addXp('mind', medTask.reward);
                        save();
                        render();
                    }
                }
            }, 1000);
        }

        function pauseTimer() {
            if (timerInterval) { stopTimer();
                document.getElementById('timerPauseBtn').textContent = '▶️';
                document.getElementById('timerPauseBtn').onclick = resumeTimer; } else resumeTimer();
        }

        function resumeTimer() {
            if (timerSeconds <= 0) return;
            document.getElementById('timerPauseBtn').textContent = '⏸️';
            document.getElementById('timerPauseBtn').onclick = pauseTimer;
            timerInterval = setInterval(() => {
                timerSeconds--;
                const display = document.getElementById('timerDisplay');
                if (display) display.textContent = formatTime(timerSeconds);
                if (timerSeconds <= 0) {
                    stopTimer();
                    if ('vibrate' in navigator) navigator.vibrate([200, 100, 200]);
                    document.getElementById('timerModal')?.remove();
                    const tasks = getCachedDailyTasks();
                    const medTask = tasks.find(t => t.name.includes('Медитация'));
                    if (medTask && (data.todayDaily[medTask.id] || 0) < medTask.maxOver) {
                        data.todayDaily[medTask.id] = (data.todayDaily[medTask.id] || 0) + 1;
                        addXp('mind', medTask.reward);
                        save();
                        render();
                    }
                }
            }, 1000);
        }

        function stopTimer() { if (timerInterval) { clearInterval(timerInterval);
                timerInterval = null; } }

        function formatTime(s) { const m = Math.floor(s / 60); const sec = s % 60; return `${m}:${String(sec).padStart(2,'0')}`; }

        // ============ РЕНДЕР ============
        let currentTab = 'main';

        function render() {
            const app = document.getElementById('app');
            checkNewDay();
            checkSleepPenalty();
            checkStreakWarning();

            let html = `
                <h1>⚔️ Life RPG</h1>
                <div class="subtitle">v2.6</div>
                <div class="settings-bar">
                    ${['dark','light','forest','fire','ocean','sakura','minimal','retro'].map(t => `
                        <div class="theme-dot ${t}${data.theme===t?' active':''}" onclick="setTheme('${t}')"></div>
                    `).join('')}
                </div>
                <div class="tab-bar">
                    <div class="tab${currentTab==='main'?' active':''}" onclick="currentTab='main';render()">🎮 Игра</div>
                    <div class="tab${currentTab==='timer'?' active':''}" onclick="currentTab='timer';render()">⏱️ Таймер</div>
                    <div class="tab${currentTab==='web'?' active':''}" onclick="currentTab='web';render()">🕸️ Паутина</div>
                    <div class="tab${currentTab==='history'?' active':''}" onclick="currentTab='history';renderHistoryCalendar()">📆</div>
                    <div class="tab${currentTab==='rewards'?' active':''}" onclick="currentTab='rewards';render()">🎁</div>
                </div>
            `;

            if (currentTab === 'main') html += renderMainTab();
            else if (currentTab === 'timer') html += renderTimerTab();
            else if (currentTab === 'web') html += renderWebTab();
            else if (currentTab === 'rewards') html += renderRewardsTab();

            app.innerHTML = html;

            if ('Notification' in window && Notification.permission === 'default') {
                Notification.requestPermission();
            }
        }

        function renderMainTab() {
            let h = '';
            let gxpFor = data.globalLevel * GLOBAL_XP_PER_LEVEL;
            h += `
                <div class="level-card">
                    <div class="big-title">Общий уровень</div>
                    <div class="big-level">Ур. ${data.globalLevel}</div>
                    <div class="xp-bar-bg"><div class="xp-bar-fill" style="width:${Math.min(100,(data.globalXp/gxpFor)*100)}%"></div></div>
                    <div class="xp-text">${data.globalXp}/${gxpFor} XP</div>
                    ${data.streak>0?`<div class="streak">🔥 Стрик: ${data.streak} дн.</div>`:''}
                </div>`;
            h += '<div class="stats-grid">';
            for (let s of ['health', 'sport', 'mind', 'hygiene']) {
                let ic = { health: '❤️', sport: '💪', mind: '🧠', hygiene: '✨' };
                let nm = { health: 'Здоровье', sport: 'Спорт', mind: 'Самопознание', hygiene: 'Гигиена' };
                let xpFor = data[s].level * XP_PER_LEVEL;
                h += `<div class="stat-card"><div class="stat-icon">${ic[s]}</div><div class="stat-name">${nm[s]}</div><div class="stat-level">Ур.${data[s].level}</div><div class="stat-xp">${data[s].xp}/${xpFor}</div><div class="xp-bar-bg"><div class="xp-bar-fill" style="width:${Math.min(100,(data[s].xp/xpFor)*100)}%"></div></div></div>`;
            }
            h += '</div>';

            const wml = data.waterCount * GLASS_SIZE;
            const wp = Math.min(100, (wml / WATER_GOAL) * 100);
            h +=
                `<div class="section-title">💧 Вода</div><div class="water-section"><div class="water-title"><span>${wml}/${WATER_GOAL} мл</span><span>${data.waterDone?'✅':'⏳'}</span></div><div class="xp-bar-bg"><div class="xp-bar-fill" style="width:${wp}%;background:var(--water-fill)"></div></div><div class="water-glasses">${Array.from({length:10},(_,i)=>`<div class="glass${i<data.waterCount?' filled':''}" onclick="drinkGlass()">${i<data.waterCount?'💧':i+1}</div>`).join('')}</div>${data.waterCount>0?`<button class="btn btn-undo" style="display:block;margin:4px auto" onclick="undoGlass()">↩</button>`:''}${data.waterOverBonus>0?`<div style="color:var(--gold);text-align:center;font-size:9px">+${data.waterOverBonus} бонус XP</div>`:''}</div>`;

            h += '<div class="section-title">📋 Обязательные</div>';
            for (let t of mandatoryTasksDef) {
                let c = data.todayMandatory[t.id] || 0;
                let d = c >= 1;
                h +=
                    `<div class="task${d?' completed':''}"><div class="task-info"><div class="task-name">${t.name}</div><div class="task-reward">+${t.reward} XP (x${t.maxOver})</div>${c>1?`<div class="task-counter">${c}/${t.maxOver}</div>`:''}</div><div>${c>0?`<button class="btn btn-undo" onclick="undoTask('${t.id}',true)">↩</button>`:''}${c<t.maxOver?`<button class="btn ${d?'btn-over':'btn-done'}" onclick="completeTask('${t.id}',true,this)">${d?'+1':'✓'}</button>`:'<span style="font-size:9px;color:var(--subtext)">MAX</span>'}</div></div>`;
            }

            let daily = getCachedDailyTasks();
            h +=
                '<div class="section-title">🎯 Доп. задания (6) <button class="btn btn-refresh" onclick="refreshDailyTasks()" style="margin-left:auto">🔄 Обновить</button></div>';
            if (data.dailyRefreshUsed) h +=
                '<div style="font-size:8px;color:var(--subtext);text-align:center">Обновление использовано на сегодня</div>';
            for (let t of daily) {
                let c = data.todayDaily[t.id] || 0;
                let d = c >= 1;
                h +=
                    `<div class="task daily-bonus${d?' completed':''}"><div class="task-info"><div class="task-name">${t.name}</div><div class="task-reward">+${t.reward} XP (x${t.maxOver})</div>${c>1?`<div class="task-counter">${c}/${t.maxOver}</div>`:''}</div><div>${c>0?`<button class="btn btn-undo" onclick="undoTask('${t.id}',false)">↩</button>`:''}${c<t.maxOver?`<button class="btn ${d?'btn-over':'btn-done'}" onclick="completeTask('${t.id}',false,this)">${d?'+1':'✓'}</button>`:'<span style="font-size:9px;color:var(--subtext)">MAX</span>'}</div></div>`;
            }

            h += '<div class="section-title">🌙 Сон</div><div class="sleep-section">';
            let ss, bs = 'block',
                bw = 'none',
                pw = '';
            if (data.sleepState === 'asleep') { ss = '😴 Спите...';
                bs = 'none';
                bw = 'block'; } else if (data.sleepState === 'awake') ss = '☀️ Бодрствуете';
            else { ss = '⚠️ Не отмечен'; if (new Date().getHours() >= 2 && new Date().getHours() < 8) pw =
                    '<div class="penalty-warn">ШТРАФ -10%!</div>';
                else pw = '<div class="penalty-warn">Отметь до 02:00</div>'; }
            h +=
                `<div class="sleep-status">${ss}</div>${pw}<button class="btn btn-sleep" style="display:${bs}" onclick="goSleep()">😴 Я лёг спать</button><button class="btn btn-wake" style="display:${bw}" onclick="wakeUp()">☀️ Я проснулся</button></div>`;
            return h;
        }

        function renderTimerTab() {
            return `
                <div style="text-align:center;padding:20px">
                    <h3 style="color:var(--gold)">⏱️ Таймер медитации</h3>
                    <div class="timer-btns" style="margin:15px 0">
                        ${[1,3,5,10,15,20].map(m=>`<button class="btn btn-done" onclick="startTimer(${m*60})">${m} мин</button>`).join('')}
                    </div>
                    <button class="btn btn-undo" style="margin-top:10px" onclick="currentTab='main';render()">← Назад</button>
                </div>`;
        }

        function renderWebTab() {
            let h = '<h3 style="color:var(--gold);text-align:center">🕸️ Паутина достижений</h3>';
            h +=
                '<div style="text-align:center;font-size:10px;color:var(--subtext)">Непрерывные дни выполнения заданий ветки</div>';
            h += '<div style="display:flex;flex-wrap:wrap;justify-content:center;gap:8px;margin:10px 0">';
            for (let branch of webBranches) {
                let maxD = 0;
                for (let d of webDays) { if (data.achievements.includes(`${branch.id}_${d}`)) maxD = d; }
                h +=
                    `<div style="background:var(--card);border-radius:10px;padding:10px;text-align:center;border:1px solid var(--border);min-width:80px"><div>${branch.name}</div><div style="color:var(--gold);font-size:18px">${maxD} дн.</div><div style="font-size:8px;color:var(--subtext)">макс. серия</div></div>`;
            }
            h += '</div>';
            h += '<div style="font-size:9px;color:var(--subtext);text-align:center">Цепочка: 1→3→5→7→12→18→25→30 дней</div>';
            h +=
                `<button class="btn btn-undo" style="display:block;margin:10px auto" onclick="currentTab='main';render()">← Назад</button>`;
            return h;
        }

        function renderRewardsTab() {
            let h = '<h3 style="color:var(--gold);text-align:center">🎁 Награды</h3>';
            if (data.rewardsEarned.length === 0) h +=
                '<div style="text-align:center;color:var(--subtext)">Пока нет наград</div>';
            else for (let r of [...data.rewardsEarned].reverse()) h +=
                `<div class="achievement"><div class="ach-icon">🎁</div><div class="ach-info"><div class="ach-name">${r.reward}</div><div class="ach-desc">${r.date} · ${r.tier}</div></div></div>`;
            h +=
                `<button class="btn btn-undo" style="display:block;margin:10px auto" onclick="currentTab='main';render()">← Назад</button>`;
            return h;
        }

        function renderHistoryCalendar(monthOffset = 0) {
            const app = document.getElementById('app');
            const now = new Date();
            const tm = new Date(now.getFullYear(), now.getMonth() + monthOffset, 1);
            const y = tm.getFullYear(),
                m = tm.getMonth();
            const todayKey = getDateKey();
            const mn = ['Янв', 'Фев', 'Мар', 'Апр', 'Май', 'Июн', 'Июл', 'Авг', 'Сен', 'Окт', 'Ноя', 'Дек'];
            let h =
                `<h1>📆 История</h1><div style="display:flex;justify-content:space-between;align-items:center;margin:8px 0"><button class="btn btn-undo" onclick="renderHistoryCalendar(${monthOffset-1})">←</button><span>${mn[m]} ${y}</span><button class="btn btn-undo" onclick="renderHistoryCalendar(${monthOffset+1})">→</button></div>`;
            h += '<div class="calendar-grid">';
            for (let d of ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']) h +=
                `<div class="cal-header">${d}</div>`;
            const fd = new Date(y, m, 1);
            let sd = fd.getDay();
            if (sd === 0) sd = 7;
            sd--;
            const pm = new Date(y, m, 0);
            for (let i = sd - 1; i >= 0; i--) { const d = pm.getDate() - i; h +=
                    `<div class="cal-day other-month">${d}</div>`; }
            const dim = new Date(y, m + 1, 0).getDate();
            for (let d = 1; d <= dim; d++) {
                const dk = `${y}-${String(m+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                const hd = data.history[dk];
                let color = '';
                if (hd) {
                    const md = mandatoryTasksDef.every(t => (hd.mandatory[t.id] || 0) >= 1);
                    if (md && hd.water >= 6) color = 'green';
                    else if (md || hd.water >= 4) color = 'yellow';
                    else color = 'red';
                }
                h +=
                    `<div class="cal-day ${color} ${dk===todayKey?'today':''}" onclick="showDayDetail('${dk}')">${d}</div>`;
            }
            h += '</div>';
            h +=
                '<div style="display:flex;gap:8px;justify-content:center;font-size:9px"><span>🟢Всё</span><span>🟡Часть</span><span>🔴Нет</span></div>';
            h +=
                `<button class="btn btn-undo" style="display:block;margin:10px auto" onclick="currentTab='main';render()">← Назад</button>`;
            app.innerHTML = h;
            currentTab = 'history';
        }

        function showDayDetail(dateKey) {
            const hd = data.history[dateKey] || { mandatory: {}, daily: {}, water: 0, sleep: null };
            const isToday = dateKey === getDateKey();
            let c = `<div class="modal-overlay" id="ddm"><div class="modal"><h3>📆 ${dateKey}</h3><div>💧 Вода: ${hd.water||0} ст.</div>`;
            c += '<div style="margin:6px 0">📋 Обязательные:';
            for (let t of mandatoryTasksDef) c +=
                `<div>${t.name}: ${hd.mandatory[t.id]||0}/${t.maxOver}</div>`;
            c += '</div>';
            c +=
                `<div>🌙 Сон: ${hd.sleep||'нет'}</div><button class="btn btn-undo" style="margin-top:8px" onclick="document.getElementById('ddm').remove()">Закрыть</button></div></div>`;
            document.body.insertAdjacentHTML('beforeend', c);
            document.getElementById('ddm').addEventListener('click', function(e) { if (e.target === this) this.remove(); });
        }

        // ============ ИНИТ ============
        function init() {
            if (data.theme && data.theme !== 'dark') document.body.className = 'theme-' + data.theme;
            checkNewDay();
            render();
            setInterval(() => { checkSleepPenalty();
                checkStreakWarning(); }, 60000);
            if ('Notification' in window && Notification.permission === 'default') Notification.requestPermission();
        }
        init();
    </script>
</body>
</html>
