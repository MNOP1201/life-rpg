<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="mobile-web-app-capable" content="yes">
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='0.9em' font-size='90'>⚔️</text></svg>">
    <title>Life RPG v2.5</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{
            font-family:'Segoe UI',system-ui,-apple-system,sans-serif;
            background:#0a0a14;
            color:#e0e0e0;
            min-height:100vh;
            padding:8px;
            -webkit-tap-highlight-color:transparent;
            user-select:none;
            -webkit-user-select:none;
        }
        .app{max-width:480px;margin:0 auto;padding-bottom:50px}
        h1{text-align:center;font-size:22px;margin:6px 0 2px;color:#ffd700;text-shadow:0 0 20px rgba(255,215,0,0.3)}
        .subtitle{text-align:center;font-size:10px;color:#555;margin-bottom:10px}
        
        .level-card{
            background:linear-gradient(135deg,#14142a,#1a1040);
            border-radius:18px;padding:16px;margin-bottom:10px;
            text-align:center;border:1px solid #2a2a4a;
            box-shadow:0 4px 20px rgba(0,0,0,0.4);
        }
        .big-level{font-size:40px;font-weight:900;color:#ffd700}
        .big-title{font-size:10px;color:#777;letter-spacing:2px;text-transform:uppercase}
        .xp-bar-bg{background:#1a1a30;border-radius:20px;height:14px;margin:8px 0 4px;overflow:hidden;border:1px solid #333}
        .xp-bar-fill{height:100%;background:linear-gradient(90deg,#ffd700,#ff8c00);border-radius:20px;transition:width 0.4s ease;box-shadow:0 0 10px rgba(255,215,0,0.3)}
        .xp-text{font-size:10px;color:#777}
        
        .stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px}
        .stat-card{
            background:#111122;border-radius:14px;padding:12px 8px;
            text-align:center;border:1px solid #222244;
        }
        .stat-icon{font-size:28px;margin-bottom:2px}
        .stat-name{font-size:10px;color:#888;text-transform:uppercase;letter-spacing:0.5px}
        .stat-level{font-weight:700;color:#ffd700;font-size:14px}
        .stat-xp{font-size:9px;color:#555;margin:2px 0}
        .stat-card .xp-bar-bg{height:7px;margin:4px 0 0}
        
        .section-title{
            font-size:11px;font-weight:700;color:#666;margin:14px 0 6px;
            text-transform:uppercase;letter-spacing:2px;
            display:flex;align-items:center;gap:6px;
        }
        
        .task{
            background:#111122;border-radius:14px;padding:11px 13px;
            margin-bottom:6px;display:flex;align-items:center;
            justify-content:space-between;border:1px solid #1e1e3a;
        }
        .task.completed{opacity:0.55;border-color:#1a3a1a;background:#0d0d1a}
        .task-info{flex:1;min-width:0;padding-right:8px}
        .task-name{font-size:12px;font-weight:500;line-height:1.2}
        .task-reward{font-size:9px;color:#ffaa00;margin-top:2px}
        .task-counter{font-size:10px;color:#4fc3f7;margin-top:1px}
        
        .btn{
            padding:8px 12px;border-radius:25px;border:none;
            font-weight:700;font-size:11px;cursor:pointer;
            transition:all 0.15s;white-space:nowrap;
            min-width:40px;text-align:center;margin-left:6px;
        }
        .btn:active{transform:scale(0.93)}
        .btn-done{background:#1a5c2a;color:#fff;border:1px solid #2e7d32}
        .btn-undo{background:#333;color:#aaa;border:1px solid #444}
        .btn-over{background:#1a3a5c;color:#fff;border:1px solid #1565c0}
        
        .water-section{background:#111122;border-radius:14px;padding:14px;margin:8px 0;border:1px solid #1e1e3a}
        .water-title{font-size:13px;font-weight:600;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center}
        .water-glasses{display:flex;gap:6px;flex-wrap:wrap;justify-content:center;margin:10px 0}
        .glass{
            width:42px;height:50px;border-radius:8px 8px 14px 14px;
            background:#1a1a30;border:2px solid #333;
            display:flex;align-items:flex-end;justify-content:center;
            font-size:10px;color:#888;cursor:pointer;
            transition:all 0.2s;position:relative;overflow:hidden;
        }
        .glass.filled{background:#1565c0;border-color:#42a5f5;color:#fff}
        .glass .water-fill{position:absolute;bottom:0;left:0;right:0;background:#42a5f5;transition:height 0.3s}
        .water-progress{font-size:11px;text-align:center;color:#4fc3f7;margin:6px 0}
        .water-bonus{font-size:10px;color:#ffd700;text-align:center}
        
        .daily-bonus{background:linear-gradient(135deg,#111122,#191030);border:1px solid #3d2a6e}
        
        .sleep-section{background:#111122;border-radius:14px;padding:14px;margin:10px 0;text-align:center;border:1px solid #1e1e3a}
        .sleep-status{font-size:13px;margin-bottom:8px;font-weight:500}
        .btn-sleep,.btn-wake{width:100%;padding:12px;font-size:14px;border-radius:30px;font-weight:700;letter-spacing:1px;border:none;cursor:pointer}
        .btn-sleep{background:linear-gradient(135deg,#1a237e,#283593);color:#fff}
        .btn-wake{background:linear-gradient(135deg,#e65100,#ff8f00);color:#fff}
        .penalty-warn{color:#ff5252;font-size:10px;margin-top:6px}
        
        .log{background:#080812;border-radius:10px;padding:8px 12px;max-height:120px;overflow-y:auto;font-size:9px;color:#555;margin-top:6px;border:1px solid #1a1a2a}
        .log-item{margin:2px 0;font-family:monospace}
        
        .modal-overlay{
            position:fixed;top:0;left:0;right:0;bottom:0;
            background:rgba(0,0,0,0.85);z-index:100;
            display:flex;align-items:center;justify-content:center;
            padding:20px;
        }
        .modal{
            background:#1a1a2e;border-radius:18px;padding:20px;
            max-width:420px;width:100%;max-height:80vh;
            overflow-y:auto;border:1px solid #333;
        }
        .modal h3{color:#ffd700;margin-bottom:12px;font-size:16px}
        .modal input,.modal select{
            width:100%;padding:10px;margin:6px 0;border-radius:10px;
            border:1px solid #333;background:#111;color:#fff;font-size:13px;
        }
        .modal .btn{margin:4px;width:auto;display:inline-block}
        .modal-btns{text-align:center;margin-top:12px}
        
        .calendar-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:3px;margin:8px 0}
        .cal-day{
            aspect-ratio:1;border-radius:8px;display:flex;
            align-items:center;justify-content:center;
            font-size:11px;font-weight:600;cursor:pointer;
            background:#111122;border:1px solid #222;
        }
        .cal-day.today{border:2px solid #ffd700}
        .cal-day.green{background:#1a3a1a;border-color:#2e7d32}
        .cal-day.yellow{background:#3a3510;border-color:#f9a825}
        .cal-day.red{background:#3a1010;border-color:#c62828}
        .cal-day.other-month{opacity:0.3}
        .cal-header{text-align:center;font-size:11px;color:#888;padding:4px 0}
        
        .achievement{
            background:linear-gradient(135deg,#1a1a2e,#1a1030);
            border-radius:12px;padding:10px 14px;margin:6px 0;
            border:1px solid #3d2a6e;display:flex;align-items:center;gap:10px;
        }
        .ach-icon{font-size:28px}
        .ach-info{flex:1}
        .ach-name{font-size:12px;font-weight:600;color:#ffd700}
        .ach-desc{font-size:10px;color:#888}
        .ach-locked{opacity:0.4;filter:grayscale(1)}
        
        .reward-card{
            background:linear-gradient(135deg,#1a2a1a,#1a3020);
            border-radius:14px;padding:14px;margin:8px 0;
            border:2px solid #ffd700;text-align:center;
            animation:glow 2s infinite;
        }
        @keyframes glow{
            0%,100%{box-shadow:0 0 10px rgba(255,215,0,0.3)}
            50%{box-shadow:0 0 25px rgba(255,215,0,0.6)}
        }
        .reward-card .reward-icon{font-size:40px}
        .reward-card .reward-text{font-size:14px;font-weight:700;color:#ffd700;margin:6px 0}
        .reward-card .reward-sub{font-size:10px;color:#aaa}
        
        .tab-bar{display:flex;gap:4px;margin-bottom:10px}
        .tab{
            flex:1;padding:8px;text-align:center;border-radius:20px;
            font-size:11px;font-weight:600;cursor:pointer;
            background:#111122;border:1px solid #222;color:#888;
        }
        .tab.active{background:#1a237e;border-color:#3d5afe;color:#fff}
        
        .xp-popup{
            position:fixed;pointer-events:none;font-weight:900;
            font-size:18px;color:#ffd700;text-shadow:0 0 10px #ffd700;
            animation:floatUp 1s ease-out forwards;z-index:200;
        }
        @keyframes floatUp{
            0%{opacity:1;transform:translateY(0) scale(1)}
            100%{opacity:0;transform:translateY(-60px) scale(1.4)}
        }
        
        .btn-add{background:#6a1b9a;color:#fff;border:1px solid #9c27b0;width:100%;padding:10px;border-radius:25px;font-size:12px;margin:6px 0}
        
        .streak{text-align:center;font-size:12px;color:#ffd700;margin:4px 0}
    </style>
</head>
<body>
    <div class="app" id="app"></div>
    <script>
            // ==================== LIFE RPG v2.5 ====================
        
        // ============ ХРАНИЛИЩЕ ДАННЫХ ============
        let data = JSON.parse(localStorage.getItem('lifeRpgv25')) || {
            health: { xp: 0, level: 1 },
            sport: { xp: 0, level: 1 },
            mind: { xp: 0, level: 1 },
            hygiene: { xp: 0, level: 1 },
            globalXp: 0,
            globalLevel: 1,
            sleepState: null, // null | 'asleep' | 'awake'
            sleepDate: null,
            sleepDay: '', // дата когда легли
            penaltyApplied: false,
            waterCount: 0, // стаканы 250мл
            waterDone: false,
            waterOverBonus: 0,
            todayMandatory: {}, // { taskId: count }
            todayDaily: {}, // { taskId: count }
            customTasks: [], // [{ id, name, stat, xp, date, maxOver }]
            history: {}, // { 'YYYY-MM-DD': { mandatory:{}, daily:{}, water:0, sleep:null } }
            achievements: [], // ['ach_id', ...]
            rewardsEarned: [], // [{ date, reward }]
            dailyTasksDate: '',
            log: [],
            streak: 0,
            lastActiveDate: ''
        };

        const XP_PER_LEVEL = 100;
        const GLOBAL_XP_PER_LEVEL = 400;
        const WATER_GOAL = 1500; // мл
        const GLASS_SIZE = 250; // мл

        // ============ ПУЛ ЗАДАНИЙ ============
        const mandatoryTasksDef = [
            { id: 'abs', name: '🏋️ Тренировка на пресс', reward: 25, stat: 'sport', maxOver: 3 },
            { id: 'hygiene', name: '🪥 Чистка зубов + умывание', reward: 20, stat: 'hygiene', maxOver: 2 },
        ];

        const dailyTasksPool = [
            { name: '📖 Прочитать 15 страниц', reward: 25, stat: 'mind', maxOver: 3 },
            { name: '🧘 Медитация 5 мин', reward: 20, stat: 'mind', maxOver: 3 },
            { name: '🙏 3 благодарности', reward: 20, stat: 'mind', maxOver: 2 },
            { name: '⏱️ Планка 1 мин', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🚶 Прогулка 20 мин', reward: 30, stat: 'sport', maxOver: 2 },
            { name: '🤸 Растяжка 10 мин', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🥗 Овощной салат', reward: 25, stat: 'health', maxOver: 2 },
            { name: '🍬 День без сладкого', reward: 35, stat: 'health', maxOver: 1 },
            { name: '🍵 Зелёный чай', reward: 15, stat: 'health', maxOver: 4 },
            { name: '🚿 Контрастный душ', reward: 30, stat: 'hygiene', maxOver: 2 },
            { name: '🧴 Маска для лица', reward: 20, stat: 'hygiene', maxOver: 2 },
            { name: '🛏️ Сменить бельё', reward: 25, stat: 'hygiene', maxOver: 1 },
            { name: '🪟 Проветрить 10 мин', reward: 15, stat: 'health', maxOver: 3 },
            { name: '📝 Дневник мыслей', reward: 20, stat: 'mind', maxOver: 2 },
            { name: '🦵 50 приседаний', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🧹 Уборка 15 мин', reward: 20, stat: 'hygiene', maxOver: 2 },
            { name: '🍎 Съесть фрукт', reward: 15, stat: 'health', maxOver: 4 },
            { name: '📵 Час без телефона', reward: 25, stat: 'mind', maxOver: 3 },
            { name: '☀️ Зарядка 10 мин', reward: 20, stat: 'sport', maxOver: 2 },
            { name: '🫁 Дыхательная практика', reward: 20, stat: 'mind', maxOver: 3 },
            { name: '💊 Витамины', reward: 15, stat: 'health', maxOver: 2 },
            { name: '🚰 Стакан воды с лимоном', reward: 10, stat: 'health', maxOver: 3 },
            { name: '🧘‍♂️ 10000 шагов', reward: 35, stat: 'sport', maxOver: 1 },
            { name: '📚 Учить новое слово', reward: 15, stat: 'mind', maxOver: 5 },
            { name: '🎧 Подкаст 20 мин', reward: 20, stat: 'mind', maxOver: 2 },
            { name: '🧼 Скраб для лица', reward: 20, stat: 'hygiene', maxOver: 1 },
            { name: '💪 Отжимания 20 раз', reward: 25, stat: 'sport', maxOver: 3 },
            { name: '🥜 Горсть орехов', reward: 10, stat: 'health', maxOver: 3 },
            { name: '🚫 Без кофеина весь день', reward: 30, stat: 'health', maxOver: 1 },
            { name: '🧴 Увлажнить кожу', reward: 15, stat: 'hygiene', maxOver: 2 },
            { name: '📿 Аффирмации', reward: 20, stat: 'mind', maxOver: 3 },
            { name: '🕺 Танцы 10 мин', reward: 25, stat: 'sport', maxOver: 2 },
            { name: '🍽️ Не переедать', reward: 25, stat: 'health', maxOver: 1 },
        ];

        // ============ АЧИВКИ ============
        const achievementsDef = [
            { id: 'first_day', name: '🌱 Первый шаг', desc: 'Выполнить все обязательные задания за день', icon: '🌱' },
            { id: 'streak_3', name: '🔥 Огонь 3 дня', desc: 'Выполнять всё 3 дня подряд', icon: '🔥' },
            { id: 'streak_7', name: '💎 Неделя силы', desc: 'Выполнять всё 7 дней подряд', icon: '💎' },
            { id: 'streak_30', name: '👑 Месяц легенды', desc: 'Выполнять всё 30 дней подряд', icon: '👑' },
            { id: 'water_master', name: '💧 Водный мастер', desc: 'Выпить 2000+ мл воды за день 5 раз', icon: '💧' },
            { id: 'overachiever', name: '⭐ Сверхусилие', desc: 'Перевыполнить 3 задания за день', icon: '⭐' },
            { id: 'sport_5', name: '💪 Спортсмен', desc: 'Достичь уровня 5 в Спорте', icon: '💪' },
            { id: 'health_5', name: '❤️ ЗОЖник', desc: 'Достичь уровня 5 в Здоровье', icon: '❤️' },
            { id: 'mind_5', name: '🧠 Мудрец', desc: 'Достичь уровня 5 в Самопознании', icon: '🧠' },
            { id: 'hygiene_5', name: '✨ Сияние', desc: 'Достичь уровня 5 в Гигиене', icon: '✨' },
            { id: 'global_10', name: '🏆 Элита', desc: 'Достичь общего уровня 10', icon: '🏆' },
            { id: 'custom_creator', name: '✏️ Творец', desc: 'Создать 5 своих заданий', icon: '✏️' },
        ];

        // ============ НАГРАДЫ РЕАЛЬНОЙ ЖИЗНИ ============
        const rewardsPool = {
            small: [
                '🍫 Съесть вкусняшку без угрызений совести',
                '☕ Сделать любимый напиток и насладиться',
                '🎵 Послушать любимый альбом целиком',
                '📺 Посмотреть 1 серию любимого сериала',
                '🛀 Поваляться в ванне 20 минут',
                '📱 30 минут соцсетей в награду',
                '🎮 30 минут игры',
            ],
            medium: [
                '🍕 Заказать доставку еды',
                '🚶 Прогулка в любимом месте',
                '📖 Купить книгу из вишлиста',
                '🎬 Посмотреть фильм в кино',
                '☕ Сходить в любимую кофейню',
                '🛍️ Купить мелочь для себя (до 500₽)',
                '🌳 Провести час на природе',
            ],
            big: [
                '🎁 Купить желанную вещь (до 2000₽)',
                '🧖 SPA-день дома',
                '🍔 Полный читмил-день',
                '🚴 Поездка на природу на полдня',
                '🎸 Купить что-то для хобби',
                '📵 Цифровой детокс на полдня',
                '👕 Обновить гардероб (1 вещь)',
            ],
            legendary: [
                '✈️ Мини-путешествие на выходные',
                '🎪 Билет на концерт/фестиваль',
                '💻 Крупная покупка из вишлиста',
                '🏨 Ночевка в отеле с завтраком',
                '🎨 Мастер-класс или курс',
                '🎉 Устроить вечеринку для друзей',
                '⌚ Купить гаджет мечты',
            ]
        };

        // ============ УТИЛИТЫ ============
        function getToday() { return new Date().toDateString(); }
        function getDateKey(d) { return d ? d : new Date().toISOString().split('T')[0]; }
        function getStatName(s) {
            return { health: '❤️ Здоровье', sport: '💪 Спорт', mind: '🧠 Самопознание', hygiene: '✨ Гигиена' }[s] || s;
        }

        function save() {
            localStorage.setItem('lifeRpgv25', JSON.stringify(data));
        }

        function addLog(msg) {
            const time = new Date().toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit' });
            data.log.unshift(`[${time}] ${msg}`);
            if (data.log.length > 100) data.log.pop();
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

        // ============ ГЕНЕРАЦИЯ ДОП ЗАДАНИЙ ============
        function generateDailyTasks() {
            let shuffled = [...dailyTasksPool].sort(() => Math.random() - 0.5);
            return shuffled.slice(0, 4).map((t, i) => ({ ...t, id: 'daily_' + i }));
        }

        function getCachedDailyTasks() {
            let cached = localStorage.getItem('dailyTasksCachev25');
            if (!cached) {
                cached = JSON.stringify(generateDailyTasks());
                localStorage.setItem('dailyTasksCachev25', cached);
            }
            return JSON.parse(cached);
        }

        // ============ ПРОВЕРКА НОВОГО ДНЯ ============
        function checkNewDay() {
            const today = getToday();
            const dateKey = getDateKey();

            if (data.lastActiveDate && data.lastActiveDate !== today) {
                // Проверяем стрик
                const yesterday = new Date();
                yesterday.setDate(yesterday.getDate() - 1);
                const yesterdayKey = yesterday.toISOString().split('T')[0];
                const yesterdayHistory = data.history[yesterdayKey];
                if (yesterdayHistory) {
                    const allMandatoryDone = mandatoryTasksDef.every(t => (yesterdayHistory.mandatory[t.id] || 0) >= 1);
                    const waterDone = (yesterdayHistory.water || 0) >= 6;
                    if (allMandatoryDone && waterDone && yesterdayHistory.daily && Object.keys(yesterdayHistory.daily).length >= 4) {
                        data.streak++;
                        addLog(`🔥 Стрик: ${data.streak} дней!`);
                        checkAchievements();
                    } else {
                        data.streak = 0;
                    }
                } else {
                    data.streak = 0;
                }
            }

            if (data.dailyTasksDate !== today) {
                // Сохраняем вчерашний день в историю
                if (data.dailyTasksDate) {
                    const oldKey = new Date(data.dailyTasksDate).toISOString().split('T')[0];
                    if (!data.history[oldKey]) {
                        data.history[oldKey] = {
                            mandatory: { ...data.todayMandatory },
                            daily: { ...data.todayDaily },
                            water: data.waterCount,
                            sleep: data.sleepState,
                            waterOverBonus: data.waterOverBonus,
                        };
                    }
                }

                // Сбрасываем день
                data.dailyTasksDate = today;
                data.todayMandatory = {};
                data.todayDaily = {};
                data.waterCount = 0;
                data.waterDone = false;
                data.waterOverBonus = 0;
                data.penaltyApplied = false;
                data.sleepState = null;
                data.sleepDate = null;
                data.sleepDay = '';
                localStorage.setItem('dailyTasksCachev25', JSON.stringify(generateDailyTasks()));
                addLog('🌅 Новый день! Задания обновлены.');
            }

            data.lastActiveDate = today;
            save();
        }

        // ============ НАЧИСЛЕНИЕ XP ============
        function addXp(stat, amount) {
            if (!data[stat]) return;
            data[stat].xp += amount;
            data.globalXp += amount;
            addLog(`+${amount} XP → ${getStatName(stat)}`);

            while (data[stat].xp >= data[stat].level * XP_PER_LEVEL) {
                data[stat].xp -= data[stat].level * XP_PER_LEVEL;
                data[stat].level++;
                addLog(`🎉 ${getStatName(stat)} → Ур.${data[stat].level}!`);
                checkAchievements();
            }
            while (data.globalXp >= data.globalLevel * GLOBAL_XP_PER_LEVEL) {
                data.globalXp -= data.globalLevel * GLOBAL_XP_PER_LEVEL;
                data.globalLevel++;
                addLog(`👑 ОБЩИЙ УРОВЕНЬ ${data.globalLevel}!`);
                checkAchievements();
                // Награда за уровень
                if (data.globalLevel % 5 === 0) {
                    grantReward('big');
                } else if (data.globalLevel % 3 === 0) {
                    grantReward('medium');
                }
            }
            save();
        }

        // ============ ВЫПОЛНЕНИЕ ЗАДАНИЯ ============
        function completeTask(taskId, isMandatory, btnElement) {
            if (isMandatory) {
                const task = mandatoryTasksDef.find(t => t.id === taskId);
                if (!task) return;
                const current = data.todayMandatory[taskId] || 0;
                if (current >= task.maxOver) return;
                data.todayMandatory[taskId] = current + 1;
                addXp(task.stat, task.reward);
                if (current + 1 > 1) addLog(`🔄 Перевыполнение: ${task.name} x${current + 1}`);
                // Сохраняем в историю
                const dateKey = getDateKey();
                if (!data.history[dateKey]) data.history[dateKey] = { mandatory: {}, daily: {}, water: 0, sleep: null, waterOverBonus: 0 };
                data.history[dateKey].mandatory[taskId] = (data.history[dateKey].mandatory[taskId] || 0) + 1;
            } else {
                const tasks = getCachedDailyTasks();
                const task = tasks.find(t => t.id === taskId);
                if (!task) return;
                const current = data.todayDaily[taskId] || 0;
                if (current >= task.maxOver) return;
                data.todayDaily[taskId] = current + 1;
                addXp(task.stat, task.reward);
                if (current + 1 > 1) addLog(`🔄 Перевыполнение: ${task.name} x${current + 1}`);
                const dateKey = getDateKey();
                if (!data.history[dateKey]) data.history[dateKey] = { mandatory: {}, daily: {}, water: 0, sleep: null, waterOverBonus: 0 };
                data.history[dateKey].daily[taskId] = (data.history[dateKey].daily[taskId] || 0) + 1;
            }

            if (btnElement) {
                const rect = btnElement.getBoundingClientRect();
                spawnXpPopup(rect.left + rect.width / 2 - 20, rect.top - 10, '+XP');
            }
            checkAchievements();
            save();
            render();
        }

        function undoTask(taskId, isMandatory) {
            if (isMandatory) {
                const current = data.todayMandatory[taskId] || 0;
                if (current <= 0) return;
                const task = mandatoryTasksDef.find(t => t.id === taskId);
                if (!task) return;
                data.todayMandatory[taskId] = current - 1;
                if (data.todayMandatory[taskId] <= 0) delete data.todayMandatory[taskId];
                addXp(task.stat, -task.reward);
                const dateKey = getDateKey();
                if (data.history[dateKey]) {
                    data.history[dateKey].mandatory[taskId] = Math.max(0, (data.history[dateKey].mandatory[taskId] || 0) - 1);
                }
            } else {
                const current = data.todayDaily[taskId] || 0;
                if (current <= 0) return;
                const tasks = getCachedDailyTasks();
                const task = tasks.find(t => t.id === taskId);
                if (!task) return;
                data.todayDaily[taskId] = current - 1;
                if (data.todayDaily[taskId] <= 0) delete data.todayDaily[taskId];
                addXp(task.stat, -task.reward);
                const dateKey = getDateKey();
                if (data.history[dateKey]) {
                    data.history[dateKey].daily[taskId] = Math.max(0, (data.history[dateKey].daily[taskId] || 0) - 1);
                }
            }
            save();
            render();
        }

        // ============ ВОДА ============
        function drinkGlass() {
            if (data.waterCount >= 10) return; // максимум 10 стаканов
            data.waterCount++;
            if (data.waterCount >= 6) {
                if (!data.waterDone) {
                    data.waterDone = true;
                    addXp('health', 20);
                    addLog('💧 Норма воды выполнена! +20 XP');
                }
                if (data.waterCount > 6) {
                    data.waterOverBonus += 5;
                    addXp('health', 5);
                    addLog(`💧 Бонусный стакан! +5 XP (всего стаканов: ${data.waterCount})`);
                }
            }
            // Сохраняем в историю
            const dateKey = getDateKey();
            if (!data.history[dateKey]) data.history[dateKey] = { mandatory: {}, daily: {}, water: 0, sleep: null, waterOverBonus: 0 };
            data.history[dateKey].water = data.waterCount;
            data.history[dateKey].waterOverBonus = data.waterOverBonus;
            save();
            render();
        }

        function undoGlass() {
            if (data.waterCount <= 0) return;
            if (data.waterCount > 6) {
                data.waterOverBonus = Math.max(0, data.waterOverBonus - 5);
                addXp('health', -5);
            }
            if (data.waterCount === 6 && data.waterDone) {
                data.waterDone = false;
                addXp('health', -20);
            }
            data.waterCount--;
            const dateKey = getDateKey();
            if (data.history[dateKey]) {
                data.history[dateKey].water = data.waterCount;
                data.history[dateKey].waterOverBonus = data.waterOverBonus;
            }
            save();
            render();
        }

        // ============ СОН ============
        function goSleep() {
            data.sleepState = 'asleep';
            data.sleepDate = new Date().toISOString();
            data.sleepDay = getDateKey();
            data.penaltyApplied = false;
            addLog('😴 Отход ко сну отмечен');
            const dateKey = getDateKey();
            if (!data.history[dateKey]) data.history[dateKey] = { mandatory: {}, daily: {}, water: 0, sleep: null, waterOverBonus: 0 };
            data.history[dateKey].sleep = 'asleep';
            save();
            render();
        }

        function wakeUp() {
            if (data.sleepState === 'asleep') {
                data.sleepState = 'awake';
                addLog('☀️ Подъём! Новый день!');

                // Сохраняем текущий день в историю
                const oldKey = data.sleepDay || getDateKey();
                if (!data.history[oldKey]) {
                    data.history[oldKey] = {
                        mandatory: { ...data.todayMandatory },
                        daily: { ...data.todayDaily },
                        water: data.waterCount,
                        sleep: 'awake',
                        waterOverBonus: data.waterOverBonus,
                    };
                } else {
                    data.history[oldKey].sleep = 'awake';
                }

                // Сбрасываем всё
                data.todayMandatory = {};
                data.todayDaily = {};
                data.waterCount = 0;
                data.waterDone = false;
                data.waterOverBonus = 0;
                data.penaltyApplied = false;
                data.sleepState = 'awake';
                data.dailyTasksDate = getToday();
                data.sleepDay = '';
                localStorage.setItem('dailyTasksCachev25', JSON.stringify(generateDailyTasks()));
                addLog('🌅 Задания обновлены после пробуждения');
                checkAchievements();
                save();
                render();
            }
        }

        function checkSleepPenalty() {
            const h = new Date().getHours();
            if (h >= 2 && h < 8 && data.sleepState === null && !data.penaltyApplied) {
                for (let s of ['health', 'sport', 'mind', 'hygiene']) {
                    let p = Math.floor(data[s].xp * 0.1);
                    if (p > 0) {
                        data[s].xp = Math.max(0, data[s].xp - p);
                        data.globalXp = Math.max(0, data.globalXp - p);
                    }
                }
                data.penaltyApplied = true;
                addLog('⚠️ ШТРАФ -10%: сон не отмечен после 02:00!');
                save();
                render();
            }
        }

        // ============ ДОСТИЖЕНИЯ ============
        function checkAchievements() {
            const newAch = [];

            // Первый день
            const allMandDone = mandatoryTasksDef.every(t => (data.todayMandatory[t.id] || 0) >= 1);
            if (allMandDone && data.waterCount >= 6 && !data.achievements.includes('first_day')) {
                data.achievements.push('first_day');
                newAch.push('first_day');
            }

            // Стрики
            if (data.streak >= 3 && !data.achievements.includes('streak_3')) {
                data.achievements.push('streak_3');
                newAch.push('streak_3');
            }
            if (data.streak >= 7 && !data.achievements.includes('streak_7')) {
                data.achievements.push('streak_7');
                newAch.push('streak_7');
                grantReward('medium');
            }
            if (data.streak >= 30 && !data.achievements.includes('streak_30')) {
                data.achievements.push('streak_30');
                newAch.push('streak_30');
                grantReward('legendary');
            }

            // Уровни
            if (data.sport.level >= 5 && !data.achievements.includes('sport_5')) { data.achievements.push('sport_5');
                newAch.push('sport_5'); }
            if (data.health.level >= 5 && !data.achievements.includes('health_5')) { data.achievements.push('health_5');
                newAch.push('health_5'); }
            if (data.mind.level >= 5 && !data.achievements.includes('mind_5')) { data.achievements.push('mind_5');
                newAch.push('mind_5'); }
            if (data.hygiene.level >= 5 && !data.achievements.includes('hygiene_5')) { data.achievements.push('hygiene_5');
                newAch.push('hygiene_5'); }
            if (data.globalLevel >= 10 && !data.achievements.includes('global_10')) { data.achievements.push('global_10');
                newAch.push('global_10');
                grantReward('big'); }

            // Водный мастер
            let waterMasterDays = 0;
            for (let key in data.history) {
                if (data.history[key].water >= 8) waterMasterDays++;
            }
            if (waterMasterDays >= 5 && !data.achievements.includes('water_master')) {
                data.achievements.push('water_master');
                newAch.push('water_master');
            }

            // Сверхусилие
            let overCount = 0;
            for (let tid in data.todayMandatory) { if (data.todayMandatory[tid] > 1) overCount++; }
            for (let tid in data.todayDaily) { if (data.todayDaily[tid] > 1) overCount++; }
            if (overCount >= 3 && !data.achievements.includes('overachiever')) {
                data.achievements.push('overachiever');
                newAch.push('overachiever');
            }

            // Творец
            if (data.customTasks.length >= 5 && !data.achievements.includes('custom_creator')) {
                data.achievements.push('custom_creator');
                newAch.push('custom_creator');
            }

            for (let achId of newAch) {
                const ach = achievementsDef.find(a => a.id === achId);
                if (ach) addLog(`🏆 Ачивка: ${ach.icon} ${ach.name}!`);
            }

            if (newAch.length > 0) save();
        }

        // ============ НАГРАДЫ ============
        function grantReward(tier) {
            const pool = rewardsPool[tier];
            if (!pool) return;
            const reward = pool[Math.floor(Math.random() * pool.length)];
            data.rewardsEarned.push({ date: getDateKey(), reward, tier });
            addLog(`🎁 Награда (${tier}): ${reward}`);
            save();
            // Показываем модалку с наградой
            showRewardModal(reward, tier);
        }

        function showRewardModal(reward, tier) {
            const tierNames = { small: 'Малая', medium: 'Средняя', big: 'Крупная', legendary: 'ЛЕГЕНДАРНАЯ' };
            const overlay = document.createElement('div');
            overlay.className = 'modal-overlay';
            overlay.innerHTML = `
                <div class="modal">
                    <div class="reward-card">
                        <div class="reward-icon">🎁</div>
                        <div class="reward-text">${tierNames[tier]} награда!</div>
                        <div style="font-size:16px;margin:8px 0;color:#fff">${reward}</div>
                        <div class="reward-sub">Ты заслужил! Выполни это для себя</div>
                    </div>
                    <div class="modal-btns">
                        <button class="btn btn-done" onclick="this.closest('.modal-overlay').remove()">👍 Принято!</button>
                    </div>
                </div>
            `;
            document.body.appendChild(overlay);
            overlay.addEventListener('click', function(e) { if (e.target === overlay) overlay.remove(); });
        }

        // ============ СВОИ ЗАДАНИЯ ============
        function addCustomTask(name, stat, xp, date, maxOver) {
            const task = {
                id: 'custom_' + Date.now(),
                name,
                stat,
                reward: parseInt(xp),
                date: date || getDateKey(),
                maxOver: parseInt(maxOver) || 3,
            };
            data.customTasks.push(task);
            addLog(`✏️ Добавлено задание: ${name}`);
            save();
            render();
        }

        function completeCustomTask(taskId) {
            const task = data.customTasks.find(t => t.id === taskId);
            if (!task) return;
            const key = 'custom_' + taskId;
            const current = data.todayDaily[key] || 0;
            if (current >= task.maxOver) return;
            data.todayDaily[key] = current + 1;
            addXp(task.stat, task.reward);
            if (current + 1 > 1) addLog(`🔄 Перевыполнение своего задания: ${task.name} x${current + 1}`);
            checkAchievements();
            save();
            render();
        }

        function deleteCustomTask(taskId) {
            data.customTasks = data.customTasks.filter(t => t.id !== taskId);
            save();
            render();
        }

        // ============ КАЛЕНДАРЬ И ИСТОРИЯ ============
        function getHistoryForDate(dateKey) {
            return data.history[dateKey] || { mandatory: {}, daily: {}, water: 0, sleep: null, waterOverBonus: 0 };
        }

        function editHistoryDate(dateKey, type, taskId, value) {
            if (!data.history[dateKey]) {
                data.history[dateKey] = { mandatory: {}, daily: {}, water: 0, sleep: null, waterOverBonus: 0 };
            }
            if (type === 'mandatory') {
                data.history[dateKey].mandatory[taskId] = Math.max(0, value);
            } else if (type === 'daily') {
                data.history[dateKey].daily[taskId] = Math.max(0, value);
            } else if (type === 'water') {
                data.history[dateKey].water = Math.max(0, Math.min(10, value));
            }
            addLog(`📝 История ${dateKey} обновлена`);
            save();
            renderHistoryCalendar();
        }

        function getDayColor(dateKey) {
            const h = data.history[dateKey];
            if (!h) return '';
            const mandDone = mandatoryTasksDef.every(t => (h.mandatory[t.id] || 0) >= 1);
            const waterDone = (h.water || 0) >= 6;
            const dailyDone = h.daily && Object.keys(h.daily).length >= 4;
            if (mandDone && waterDone && dailyDone) return 'green';
            if (mandDone || waterDone || (h.daily && Object.keys(h.daily).length > 0)) return 'yellow';
            return 'red';
        }

        // ============ УВЕДОМЛЕНИЯ О ВОДЕ ============
        let waterNotificationInterval = null;

        function startWaterNotifications() {
            stopWaterNotifications();
            if (!('Notification' in window)) return;
            if (Notification.permission === 'granted') {
                scheduleWaterCheck();
            } else if (Notification.permission !== 'denied') {
                Notification.requestPermission().then(perm => {
                    if (perm === 'granted') scheduleWaterCheck();
                });
            }
        }

        function scheduleWaterCheck() {
            waterNotificationInterval = setInterval(() => {
                const now = new Date();
                const hour = now.getHours();
                if (hour >= 14 && hour <= 22 && data.waterCount < 6 && data.sleepState !== 'asleep') {
                    if (Notification.permission === 'granted') {
                        new Notification('💧 Life RPG', {
                            body: `Выпей стакан воды! Осталось: ${6 - data.waterCount} ст. до нормы`,
                            icon: '💧',
                            silent: true,
                        });
                    }
                }
                if (data.waterCount >= 6) stopWaterNotifications();
            }, 3600000); // каждый час
        }

        function stopWaterNotifications() {
            if (waterNotificationInterval) {
                clearInterval(waterNotificationInterval);
                waterNotificationInterval = null;
            }
        }

        // ============ РЕНДЕР ============
        let currentTab = 'main'; // main | history | achievements | rewards | custom

        function render() {
            const app = document.getElementById('app');
            checkNewDay();
            checkSleepPenalty();

            // Таббар
            let html = `
                <h1>⚔️ Life RPG</h1>
                <div class="subtitle">Прокачка в реальной жизни v2.5</div>
                <div class="tab-bar">
                    <div class="tab${currentTab === 'main' ? ' active' : ''}" onclick="currentTab='main';render()">🎮 Игра</div>
                    <div class="tab${currentTab === 'history' ? ' active' : ''}" onclick="currentTab='history';renderHistoryCalendar()">📆 История</div>
                    <div class="tab${currentTab === 'achievements' ? ' active' : ''}" onclick="currentTab='achievements';render()">🏆 Ачивки</div>
                    <div class="tab${currentTab === 'rewards' ? ' active' : ''}" onclick="currentTab='rewards';render()">🎁 Награды</div>
                    <div class="tab${currentTab === 'custom' ? ' active' : ''}" onclick="currentTab='custom';render()">✏️ Своё</div>
                </div>
            `;

            if (currentTab === 'main') {
                html += renderMainTab();
            } else if (currentTab === 'achievements') {
                html += renderAchievementsTab();
            } else if (currentTab === 'rewards') {
                html += renderRewardsTab();
            } else if (currentTab === 'custom') {
                html += renderCustomTab();
            }

            app.innerHTML = html;
            if (currentTab === 'main') startWaterNotifications();
        }

        function renderMainTab() {
            let h = '';

            // Глобальный уровень
            let gxpFor = data.globalLevel * GLOBAL_XP_PER_LEVEL;
            h += `
                <div class="level-card">
                    <div class="big-title">Общий уровень</div>
                    <div class="big-level">Ур. ${data.globalLevel}</div>
                    <div class="xp-bar-bg"><div class="xp-bar-fill" style="width:${Math.min(100,(data.globalXp/gxpFor)*100)}%"></div></div>
                    <div class="xp-text">${data.globalXp} / ${gxpFor} XP</div>
                    ${data.streak > 0 ? `<div class="streak">🔥 Стрик: ${data.streak} дн.</div>` : ''}
                </div>
            `;

            // 4 шкалы
            h += '<div class="stats-grid">';
            for (let s of ['health', 'sport', 'mind', 'hygiene']) {
                let icons = { health: '❤️', sport: '💪', mind: '🧠', hygiene: '✨' };
                let names = { health: 'Здоровье', sport: 'Спорт', mind: 'Самопознание', hygiene: 'Гигиена' };
                let xpFor = data[s].level * XP_PER_LEVEL;
                h += `
                    <div class="stat-card">
                        <div class="stat-icon">${icons[s]}</div>
                        <div class="stat-name">${names[s]}</div>
                        <div class="stat-level">Ур.${data[s].level}</div>
                        <div class="stat-xp">${data[s].xp}/${xpFor} XP</div>
                        <div class="xp-bar-bg"><div class="xp-bar-fill" style="width:${Math.min(100,(data[s].xp/xpFor)*100)}%"></div></div>
                    </div>
                `;
            }
            h += '</div>';

            // ВОДА
            const waterMl = data.waterCount * GLASS_SIZE;
            const waterPercent = Math.min(100, (waterMl / WATER_GOAL) * 100);
            h += `
                <div class="section-title">💧 Вода</div>
                <div class="water-section">
                    <div class="water-title">
                        <span>${waterMl} мл / ${WATER_GOAL} мл</span>
                        <span>${data.waterDone ? '✅ Норма!' : '⏳'}</span>
                    </div>
                    <div class="xp-bar-bg"><div class="xp-bar-fill" style="width:${waterPercent}%;background:linear-gradient(90deg,#1565c0,#42a5f5)"></div></div>
                    <div class="water-glasses">
                        ${Array.from({ length: 10 }, (_, i) => `
                            <div class="glass${i < data.waterCount ? ' filled' : ''}" onclick="drinkGlass()" title="${(i+1)*GLASS_SIZE} мл">
                                ${i < data.waterCount ? '💧' : (i+1)}
                            </div>
                        `).join('')}
                    </div>
                    ${data.waterCount > 0 ? `<button class="btn btn-undo" style="display:block;margin:6px auto" onclick="undoGlass()">↩ Отменить стакан</button>` : ''}
                    ${data.waterOverBonus > 0 ? `<div class="water-bonus">+${data.waterOverBonus} бонус XP за перевыполнение!</div>` : ''}
                    <div class="water-progress">Нажимай на стакан, чтобы отметить 250 мл</div>
                </div>
            `;

            // ОБЯЗАТЕЛЬНЫЕ
            h += '<div class="section-title">📋 Обязательные</div>';
            for (let t of mandatoryTasksDef) {
                let count = data.todayMandatory[t.id] || 0;
                let done = count >= 1;
                let canOver = count < t.maxOver;
                h += `
                    <div class="task${done ? ' completed' : ''}">
                        <div class="task-info">
                            <div class="task-name">${t.name}</div>
                            <div class="task-reward">+${t.reward} XP → ${getStatName(t.stat)}</div>
                            ${count > 1 ? `<div class="task-counter">Выполнено: ${count}/${t.maxOver}</div>` : ''}
                        </div>
                        <div>
                            ${count > 0 ? `<button class="btn btn-undo" onclick="undoTask('${t.id}',true)">↩</button>` : ''}
                            ${canOver ? `<button class="btn ${done ? 'btn-over' : 'btn-done'}" onclick="completeTask('${t.id}',true,this)">${done ? `+1` : '✓'}</button>` : '<span style="font-size:10px;color:#666">MAX</span>'}
                        </div>
                    </div>
                `;
            }

            // ДОП ЗАДАНИЯ
            let daily = getCachedDailyTasks();
            h += '<div class="section-title">🎯 Доп. задания</div>';
            for (let t of daily) {
                let count = data.todayDaily[t.id] || 0;
                let done = count >= 1;
                let canOver = count < t.maxOver;
                h += `
                    <div class="task daily-bonus${done ? ' completed' : ''}">
                        <div class="task-info">
                            <div class="task-name">${t.name}</div>
                            <div class="task-reward">+${t.reward} XP → ${getStatName(t.stat)} (x${t.maxOver})</div>
                            ${count > 1 ? `<div class="task-counter">Выполнено: ${count}/${t.maxOver}</div>` : ''}
                        </div>
                        <div>
                            ${count > 0 ? `<button class="btn btn-undo" onclick="undoTask('${t.id}',false)">↩</button>` : ''}
                            ${canOver ? `<button class="btn ${done ? 'btn-over' : 'btn-done'}" onclick="completeTask('${t.id}',false,this)">${done ? `+1` : '✓'}</button>` : '<span style="font-size:10px;color:#666">MAX</span>'}
                        </div>
                    </div>
                `;
            }

            // Свои задания (те что на сегодня)
            let todayCustom = data.customTasks.filter(t => t.date === getDateKey() || !t.date);
            if (todayCustom.length > 0) {
                h += '<div class="section-title">✏️ Свои задания</div>';
                for (let t of todayCustom) {
                    let key = 'custom_' + t.id;
                    let count = data.todayDaily[key] || 0;
                    let done = count >= 1;
                    let canOver = count < t.maxOver;
                    h += `
                        <div class="task${done ? ' completed' : ''}">
                            <div class="task-info">
                                <div class="task-name">✏️ ${t.name}</div>
                                <div class="task-reward">+${t.reward} XP → ${getStatName(t.stat)}</div>
                                ${count > 1 ? `<div class="task-counter">Выполнено: ${count}/${t.maxOver}</div>` : ''}
                            </div>
                            <div>
                                ${count > 0 ? `<button class="btn btn-undo" onclick="undoTask('${key}',false)">↩</button>` : ''}
                                ${canOver ? `<button class="btn ${done ? 'btn-over' : 'btn-done'}" onclick="completeCustomTask('${t.id}')">${done ? `+1` : '✓'}</button>` : '<span style="font-size:10px;color:#666">MAX</span>'}
                            </div>
                        </div>
                    `;
                }
            }

            // СОН
            h += '<div class="section-title">🌙 Сон</div>';
            h += '<div class="sleep-section">';
            let ss = '';
            let bs = 'block';
            let bw = 'none';
            let pw = '';
            if (data.sleepState === 'asleep') {
                ss = '😴 Вы спите...';
                bs = 'none';
                bw = 'block';
            } else if (data.sleepState === 'awake') {
                ss = '☀️ Вы бодрствуете';
            } else {
                ss = '⚠️ Сон не отмечен';
                const now = new Date();
                pw = (now.getHours() >= 2 && now.getHours() < 8) ?
                    '<div class="penalty-warn">🔴 ШТРАФ -10% после 02:00!</div>' :
                    '<div class="penalty-warn">Не забудь отметить сон до 02:00</div>';
            }
            h += `<div class="sleep-status">${ss}</div>${pw}`;
            h += `<button class="btn btn-sleep" style="display:${bs}" onclick="goSleep()">😴 Я лёг спать</button>`;
            h += `<button class="btn btn-wake" style="display:${bw}" onclick="wakeUp()">☀️ Я проснулся</button>`;
            h += '</div>';

            // ЛОГ
            h += '<div class="section-title">📜 Лог</div>';
            h += '<div class="log">';
            h += data.log.slice(0, 20).map(l => `<div class="log-item">${l}</div>`).join('');
            h += '</div>';

            return h;
        }

        function renderAchievementsTab() {
            let h = '<div class="section-title">🏆 Ачивки</div>';
            for (let ach of achievementsDef) {
                let unlocked = data.achievements.includes(ach.id);
                h += `
                    <div class="achievement${unlocked ? '' : ' ach-locked'}">
                        <div class="ach-icon">${unlocked ? ach.icon : '🔒'}</div>
                        <div class="ach-info">
                            <div class="ach-name">${ach.name}</div>
                            <div class="ach-desc">${ach.desc}</div>
                        </div>
                        ${unlocked ? '<span style="color:#ffd700">✅</span>' : ''}
                    </div>
                `;
            }
            h += `<button class="btn btn-done" style="width:100%;margin-top:10px" onclick="currentTab='main';render()">← Назад к игре</button>`;
            return h;
        }

        function renderRewardsTab() {
            let h = '<div class="section-title">🎁 Заработанные награды</div>';
            if (data.rewardsEarned.length === 0) {
                h += '<div style="text-align:center;color:#666;padding:20px">Пока нет наград. Выполняй задания и повышай уровни!</div>';
            } else {
                for (let r of [...data.rewardsEarned].reverse()) {
                    h += `
                        <div class="achievement">
                            <div class="ach-icon">🎁</div>
                            <div class="ach-info">
                                <div class="ach-name">${r.reward}</div>
                                <div class="ach-desc">${r.date} · ${r.tier}</div>
                            </div>
                        </div>
                    `;
                }
            }
            h += `<button class="btn btn-done" style="width:100%;margin-top:10px" onclick="currentTab='main';render()">← Назад к игре</button>`;
            return h;
        }

        function renderCustomTab() {
            let h = '<div class="section-title">✏️ Мои задания</div>';

            // Форма добавления
            h += `
                <div class="task" style="flex-direction:column;align-items:stretch">
                    <input id="customName" placeholder="Название задания" style="width:100%;padding:10px;margin:4px 0;border-radius:10px;border:1px solid #333;background:#111;color:#fff">
                    <select id="customStat" style="width:100%;padding:10px;margin:4px 0;border-radius:10px;border:1px solid #333;background:#111;color:#fff">
                        <option value="health">❤️ Здоровье</option>
                        <option value="sport">💪 Спорт</option>
                        <option value="mind">🧠 Самопознание</option>
                        <option value="hygiene">✨ Гигиена</option>
                    </select>
                    <input id="customXp" type="number" value="20" placeholder="XP за выполнение" style="width:100%;padding:10px;margin:4px 0;border-radius:10px;border:1px solid #333;background:#111;color:#fff">
                    <input id="customDate" type="date" placeholder="Дата (пусто = сегодня)" style="width:100%;padding:10px;margin:4px 0;border-radius:10px;border:1px solid #333;background:#111;color:#fff">
                    <input id="customMaxOver" type="number" value="3" placeholder="Макс перевыполнений" style="width:100%;padding:10px;margin:4px 0;border-radius:10px;border:1px solid #333;background:#111;color:#fff">
                    <button class="btn btn-add" onclick="addCustomFromForm()">➕ Добавить задание</button>
                </div>
            `;

            // Список своих заданий
            if (data.customTasks.length > 0) {
                h += '<div class="section-title">📝 Созданные задания</div>';
                for (let t of data.customTasks) {
                    h += `
                        <div class="task">
                            <div class="task-info">
                                <div class="task-name">✏️ ${t.name}</div>
                                <div class="task-reward">+${t.reward} XP → ${getStatName(t.stat)} · ${t.date || 'Сегодня'} · x${t.maxOver}</div>
                            </div>
                            <button class="btn btn-undo" onclick="deleteCustomTask('${t.id}')">🗑️</button>
                        </div>
                    `;
                }
            }

            h += `<button class="btn btn-done" style="width:100%;margin-top:10px" onclick="currentTab='main';render()">← Назад к игре</button>`;
            return h;
        }

        function addCustomFromForm() {
            const name = document.getElementById('customName').value.trim();
            const stat = document.getElementById('customStat').value;
            const xp = document.getElementById('customXp').value;
            const date = document.getElementById('customDate').value;
            const maxOver = document.getElementById('customMaxOver').value;
            if (!name) { alert('Введи название!'); return; }
            addCustomTask(name, stat, xp, date, maxOver);
            render();
        }

        // ============ ИСТОРИЯ (КАЛЕНДАРЬ) ============
        function renderHistoryCalendar(monthOffset = 0) {
            const app = document.getElementById('app');
            const now = new Date();
            const targetMonth = new Date(now.getFullYear(), now.getMonth() + monthOffset, 1);
            const year = targetMonth.getFullYear();
            const month = targetMonth.getMonth();
            const todayKey = getDateKey();
            const monthNames = ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь',
            'Октябрь', 'Ноябрь', 'Декабрь'];

            let h = `
                <h1>⚔️ Life RPG</h1>
                <div class="subtitle">История</div>
                <div style="display:flex;justify-content:space-between;align-items:center;margin:10px 0">
                    <button class="btn btn-undo" onclick="renderHistoryCalendar(${monthOffset-1})">←</button>
                    <span style="font-weight:600">${monthNames[month]} ${year}</span>
                    <button class="btn btn-undo" onclick="renderHistoryCalendar(${monthOffset+1})">→</button>
                </div>
            `;

            // Дни недели
            const dayNames = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'];
            h += '<div class="calendar-grid">';
            for (let d of dayNames) {
                h += `<div class="cal-header">${d}</div>`;
            }

            // Первый день месяца
            const firstDay = new Date(year, month, 1);
            let startDow = firstDay.getDay();
            if (startDow === 0) startDow = 7;
            startDow--;

            // Дни предыдущего месяца
            const prevMonth = new Date(year, month, 0);
            for (let i = startDow - 1; i >= 0; i--) {
                const d = prevMonth.getDate() - i;
                const dk = `${year}-${String(month).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                h +=
                    `<div class="cal-day other-month" onclick="showDayDetail('${dk}')">${d}</div>`;
            }

            // Текущий месяц
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            for (let d = 1; d <= daysInMonth; d++) {
                const dk = `${year}-${String(month+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                const color = getDayColor(dk);
                const isToday = dk === todayKey;
                h += `
                    <div class="cal-day ${color} ${isToday ? 'today' : ''}" onclick="showDayDetail('${dk}')">
                        ${d}
                    </div>
                `;
            }

            // Оставшиеся дни
            const remaining = 7 - ((startDow + daysInMonth) % 7);
            if (remaining < 7) {
                for (let d = 1; d <= remaining; d++) {
                    const nextMonth = month + 2 > 12 ? 1 : month + 2;
                    const nextYear = month + 2 > 12 ? year + 1 : year;
                    const dk =
                        `${nextYear}-${String(nextMonth).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                    h +=
                        `<div class="cal-day other-month" onclick="showDayDetail('${dk}')">${d}</div>`;
                }
            }
            h += '</div>';

            // Легенда
            h += `
                <div style="display:flex;gap:10px;justify-content:center;font-size:10px;margin:8px 0">
                    <span>🟢 Всё сделано</span>
                    <span>🟡 Частично</span>
                    <span>🔴 Пропуск</span>
                    <span>⭐ Сегодня</span>
                </div>
            `;

            h +=
                `<button class="btn btn-done" style="width:100%" onclick="currentTab='main';render()">← Назад к игре</button>`;

            app.innerHTML = h;
            currentTab = 'history';
        }

        function showDayDetail(dateKey) {
            const hdata = getHistoryForDate(dateKey);
            const todayKey = getDateKey();
            const isToday = dateKey === todayKey;

            let content = `
                <div class="modal-overlay" id="dayDetailModal">
                    <div class="modal">
                        <h3>📆 ${dateKey}</h3>
                        <div class="section-title">💧 Вода: ${hdata.water || 0} ст. (${(hdata.water||0)*GLASS_SIZE} мл)</div>
                        ${isToday ? '' : `<div style="margin:6px 0">
                            <input type="number" id="editWater" value="${hdata.water||0}" min="0" max="10" style="width:80px;padding:8px;border-radius:8px;border:1px solid #333;background:#111;color:#fff">
                            <button class="btn btn-done" onclick="editHistoryDate('${dateKey}','water','',parseInt(document.getElementById('editWater').value))">💾</button>
                        </div>`}
                        <div class="section-title">📋 Обязательные</div>
            `;

            for (let t of mandatoryTasksDef) {
                let val = hdata.mandatory[t.id] || 0;
                content += `
                    <div class="task${val>0?' completed':''}">
                        <span>${t.name}</span>
                        <span>${val}/${t.maxOver}</span>
                        ${!isToday ? `<input type="number" id="edit_m_${t.id}" value="${val}" min="0" max="${t.maxOver}" style="width:50px;padding:4px;border-radius:6px;border:1px solid #333;background:#111;color:#fff;margin-left:6px">
                        <button class="btn btn-done" onclick="editHistoryDate('${dateKey}','mandatory','${t.id}',parseInt(document.getElementById('edit_m_${t.id}').value))">💾</button>` : ''}
                    </div>
                `;
            }

            content += '<div class="section-title">🎯 Доп. задания</div>';
            if (hdata.daily && Object.keys(hdata.daily).length > 0) {
                for (let tid in hdata.daily) {
                    let val = hdata.daily[tid];
                    content += `
                        <div class="task${val>0?' completed':''}">
                            <span>${tid}</span>
                            <span>x${val}</span>
                            ${!isToday ? `<input type="number" id="edit_d_${tid}" value="${val}" min="0" max="5" style="width:50px;padding:4px;border-radius:6px;border:1px solid #333;background:#111;color:#fff;margin-left:6px">
                            <button class="btn btn-done" onclick="editHistoryDate('${dateKey}','daily','${tid}',parseInt(document.getElementById('edit_d_${tid}').value))">💾</button>` : ''}
                        </div>
                    `;
                }
            } else {
                content += '<div style="color:#666;font-size:11px">Нет данных</div>';
            }

            content += `
                        <div class="section-title">🌙 Сон: ${hdata.sleep || 'Не отмечен'}</div>
                        <div class="modal-btns">
                            <button class="btn btn-undo" onclick="document.getElementById('dayDetailModal').remove()">Закрыть</button>
                        </div>
                    </div>
                </div>
            `;

            document.body.insertAdjacentHTML('beforeend', content);
            const modal = document.getElementById('dayDetailModal');
            modal.addEventListener('click', function(e) { if (e.target === modal) modal.remove(); });
        }

        // ============ ИНИЦИАЛИЗАЦИЯ ============
        function init() {
            checkNewDay();
            checkSleepPenalty();
            render();
            startWaterNotifications();

            setInterval(() => {
                checkSleepPenalty();
                if (currentTab === 'main') {
                    checkNewDay();
                    render();
                }
            }, 60000);
        }

        // Запуск
        init();
        console.log('⚔️ Life RPG v2.5 запущен!');
        console.log('💧 Вода: счётчик стаканов 250мл');
        console.log('🔄 Перевыполнение заданий');
        console.log('🔔 Уведомления о воде с 14:00');
        console.log('✏️ Свои задания');
        console.log('🏆 Ачивки и награды');
        console.log('📆 Календарь с историей');
    </script>
</body>
</html>
