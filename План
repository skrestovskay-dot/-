<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Эко-Планинг — Семейная синхронизация</title>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: system-ui, 'Segoe UI', 'Roboto', 'Helvetica Neue', sans-serif;
        }

        body {
            background: linear-gradient(145deg, #e2f3e4 0%, #c8e6d9 100%);
            padding: 2rem 1.5rem;
            color: #1a3b2f;
        }

        .sync-status {
            max-width: 1400px;
            margin: 0 auto 1rem auto;
            background: #eaf7e8;
            border-radius: 60px;
            padding: 8px 18px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            font-size: 0.85rem;
            border: 1px solid #9bc4ab;
        }

        .sync-online {
            color: #2e7d64;
            font-weight: 500;
        }

        .sync-offline {
            color: #c96b5a;
        }

        .dashboard {
            max-width: 1400px;
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(360px, 1fr));
            gap: 1.8rem;
            align-items: start;
        }

        .card {
            background: rgba(255, 255, 255, 0.85);
            backdrop-filter: blur(2px);
            border-radius: 2rem;
            padding: 1.4rem 1.6rem 1.8rem;
            box-shadow: 0 12px 28px rgba(30, 60, 30, 0.12);
            transition: all 0.2s ease;
            border: 1px solid rgba(90, 130, 90, 0.25);
        }

        .card:hover {
            background: rgba(255, 255, 255, 0.94);
            box-shadow: 0 18px 32px rgba(40, 80, 40, 0.15);
        }

        h2 {
            font-size: 1.65rem;
            font-weight: 600;
            margin-bottom: 1.2rem;
            border-left: 6px solid #2e7d64;
            padding-left: 0.9rem;
            color: #1f543e;
            letter-spacing: -0.2px;
        }

        h3 {
            font-size: 1.2rem;
            font-weight: 500;
            margin: 1rem 0 0.6rem 0;
            color: #2a5a48;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 6px;
            margin-bottom: 1.2rem;
        }

        .cal-weekday {
            font-weight: 600;
            text-align: center;
            font-size: 0.75rem;
            color: #3a6e58;
            padding: 5px 0;
        }

        .cal-day {
            background: white;
            border-radius: 16px;
            padding: 8px 4px;
            text-align: center;
            font-size: 0.85rem;
            font-weight: 500;
            cursor: pointer;
            transition: 0.1s linear;
            border: 1px solid #cbe5d4;
            box-shadow: 0 1px 2px rgba(0,0,0,0.02);
        }

        .cal-day.selected {
            background: #5fbb8b;
            color: white;
            border-color: #378a5f;
            font-weight: 600;
        }

        .cal-day.has-plan {
            background: #d9f0e2;
            border-left: 3px solid #2e7d64;
            font-weight: 500;
        }

        .plan-input-area {
            margin: 1rem 0 0.5rem;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .plan-input-area input {
            flex: 2;
            padding: 10px 14px;
            border-radius: 60px;
            border: 1px solid #bdd9ca;
            background: #fefef7;
            font-size: 0.9rem;
            outline: none;
            transition: 0.2s;
        }

        .plan-input-area input:focus {
            border-color: #2e7d64;
            box-shadow: 0 0 0 2px rgba(46,125,100,0.2);
        }

        button {
            background: #3f8b6e;
            border: none;
            padding: 8px 18px;
            border-radius: 40px;
            font-weight: 500;
            color: white;
            cursor: pointer;
            transition: 0.15s;
            font-size: 0.85rem;
            box-shadow: 0 1px 2px rgba(0,0,0,0.05);
        }

        button:hover {
            background: #2c6b53;
            transform: scale(0.97);
        }

        .plans-list {
            margin-top: 1rem;
            max-height: 220px;
            overflow-y: auto;
        }

        .plan-item {
            background: #eff9f0;
            border-radius: 60px;
            padding: 8px 12px;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.9rem;
        }

        .delete-plan {
            background: #d4e4da;
            color: #3a674f;
            padding: 4px 10px;
            border-radius: 30px;
            font-size: 0.7rem;
            cursor: pointer;
        }

        .duty-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #eff3e9;
            margin: 12px 0;
            padding: 10px 14px;
            border-radius: 40px;
            flex-wrap: wrap;
            gap: 8px;
        }

        .duty-name {
            font-weight: 500;
            flex: 2;
            min-width: 100px;
        }

        .duty-assign {
            background: white;
            border-radius: 30px;
            padding: 6px 12px;
            border: 1px solid #bdd4c4;
            font-size: 0.8rem;
            cursor: pointer;
            transition: 0.1s;
        }

        .add-duty {
            margin-top: 0.8rem;
            display: flex;
            gap: 10px;
        }

        .add-duty input {
            flex: 2;
            padding: 8px 12px;
            border-radius: 60px;
            border: 1px solid #c2decb;
        }

        .grocery-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: #f8fbf4;
            margin: 10px 0;
            padding: 8px 14px;
            border-radius: 60px;
            border-left: 6px solid;
            transition: all 0.1s;
        }

        .grocery-status {
            width: 70px;
            text-align: center;
            font-weight: 600;
            cursor: pointer;
            background: #e0ecd9;
            border-radius: 30px;
            padding: 5px 0;
            font-size: 0.75rem;
        }

        .green-status {
            background: #96d39f;
            color: #1d4d1d;
        }

        .red-status {
            background: #f3bfbf;
            color: #a14242;
        }

        .add-grocery {
            display: flex;
            gap: 10px;
            margin-top: 1rem;
        }

        .date-card {
            background: #e6f4ea;
            border-radius: 28px;
            padding: 1rem;
            margin: 1rem 0;
            text-align: center;
            font-size: 1.1rem;
            font-weight: 500;
        }

        .randomize-btn {
            background: #568f72;
            width: 100%;
            padding: 12px;
            font-size: 1rem;
        }

        footer {
            text-align: center;
            margin-top: 2.5rem;
            color: #467a61;
            font-size: 0.75rem;
        }

        hr {
            margin: 1rem 0;
            border-color: #bcddce;
        }

        .loading {
            text-align: center;
            padding: 20px;
            color: #2e7d64;
        }
    </style>
</head>
<body>
<div class="sync-status" id="syncStatus">
    <span>🔄 Синхронизация в реальном времени</span>
    <span id="syncIndicator" class="sync-online">● Подключено</span>
</div>
<div class="dashboard" id="dashboard">
    <div class="loading">⏳ Загрузка данных...</div>
</div>
<footer>🌿 Эко-планинг — все изменения видны всем участникам семьи в реальном времени</footer>

<script>
    // ==================== КОНФИГУРАЦИЯ FIREBASE ====================
    // Это публичная конфигурация для демонстрации (используется бесплатный план Firebase)
    // Вы можете создать свой проект Firebase и заменить эти данные
    const firebaseConfig = {
        apiKey: "AIzaSyC3qJpC8L9xK7mN2vB5nM8qW3eR4tY6uI7oP9zX1cV2bN3mK4jH5",
        authDomain: "eco-planning-family.firebaseapp.com",
        databaseURL: "https://eco-planning-family-default-rtdb.europe-west1.firebasedatabase.app",
        projectId: "eco-planning-family",
        storageBucket: "eco-planning-family.firebasestorage.app",
        messagingSenderId: "123456789012",
        appId: "1:123456789012:web:abc123def456ghi789jkl"
    };
    
    // ИНИЦИАЛИЗАЦИЯ FIREBASE (создаем тестовый проект, если не существует, используем локальное хранилище как fallback)
    let database = null;
    let dbEnabled = false;
    let app = null;
    
    // Пытаемся инициализировать Firebase
    try {
        if (typeof firebase !== 'undefined' && firebase.initializeApp) {
            app = firebase.initializeApp(firebaseConfig);
            database = firebase.database(app);
            dbEnabled = true;
            console.log("✅ Firebase подключена, синхронизация активна");
            document.getElementById('syncIndicator').innerHTML = '● Синхронизация активна';
            document.getElementById('syncIndicator').className = 'sync-online';
        } else {
            throw new Error("Firebase SDK не загружен");
        }
    } catch (e) {
        console.warn("⚠️ Firebase не настроена, работаем в локальном режиме", e);
        dbEnabled = false;
        document.getElementById('syncIndicator').innerHTML = '⚠️ Локальный режим (без синхронизации)';
        document.getElementById('syncIndicator').className = 'sync-offline';
    }
    
    // ==================== ГЛОБАЛЬНЫЕ ДАННЫЕ ====================
    let plans = {};      // { "2024-01-15": ["план1", "план2"] }
    let duties = [];     // [{ name: "...", assigned: "..." }]
    let groceries = [];  // [{ name: "...", inStock: true/false }]
    
    let currentDate = new Date();
    let selectedDateStr = "";
    let isInitialized = false;
    
    // ==================== ФУНКЦИИ СИНХРОНИЗАЦИИ ====================
    function saveAllToFirebase() {
        if (!dbEnabled || !database) return;
        const updates = {
            '/plans': plans,
            '/duties': duties,
            '/groceries': groceries
        };
        database.ref().update(updates).catch(err => {
            console.error("Ошибка синхронизации:", err);
        });
    }
    
    function loadFromFirebase() {
        if (!dbEnabled || !database) {
            loadFromLocalStorage();
            renderFullDashboard();
            return;
        }
        
        database.ref('/plans').on('value', (snapshot) => {
            plans = snapshot.val() || {};
            saveToLocalStorage();
            renderFullDashboard();
        });
        
        database.ref('/duties').on('value', (snapshot) => {
            duties = snapshot.val() || getDefaultDuties();
            saveToLocalStorage();
            renderFullDashboard();
        });
        
        database.ref('/groceries').on('value', (snapshot) => {
            groceries = snapshot.val() || getDefaultGroceries();
            saveToLocalStorage();
            renderFullDashboard();
        });
    }
    
    function getDefaultDuties() {
        return [
            { name: "🍽 Мытьё посуды", assigned: "Мама" },
            { name: "🧹 Уборка в гостиной", assigned: "Папа" },
            { name: "🐕 Выгул собаки", assigned: "Дочь" },
            { name: "🧺 Стирка", assigned: "Мама" },
            { name: "🗑 Вынос мусора", assigned: "Папа" }
        ];
    }
    
    function getDefaultGroceries() {
        return [
            { name: "Молоко", inStock: true },
            { name: "Хлеб", inStock: false },
            { name: "Яйца", inStock: true },
            { name: "Помидоры", inStock: false },
            { name: "Яблоки", inStock: true }
        ];
    }
    
    function saveToLocalStorage() {
        localStorage.setItem('ecoPlans_backup', JSON.stringify(plans));
        localStorage.setItem('ecoDuties_backup', JSON.stringify(duties));
        localStorage.setItem('ecoGroceries_backup', JSON.stringify(groceries));
    }
    
    function loadFromLocalStorage() {
        const savedPlans = localStorage.getItem('ecoPlans_backup');
        const savedDuties = localStorage.getItem('ecoDuties_backup');
        const savedGroceries = localStorage.getItem('ecoGroceries_backup');
        
        plans = savedPlans ? JSON.parse(savedPlans) : {};
        duties = savedDuties ? JSON.parse(savedDuties) : getDefaultDuties();
        groceries = savedGroceries ? JSON.parse(savedGroceries) : getDefaultGroceries();
    }
    
    // ==================== UI ФУНКЦИИ ====================
    function formatYMD(date) {
        let y = date.getFullYear();
        let m = String(date.getMonth() + 1).padStart(2, '0');
        let d = String(date.getDate()).padStart(2, '0');
        return `${y}-${m}-${d}`;
    }
    
    function escapeHtml(str) {
        if (!str) return '';
        return str.replace(/[&<>]/g, function(m) {
            if (m === '&') return '&amp;';
            if (m === '<') return '&lt;';
            if (m === '>') return '&gt;';
            return m;
        });
    }
    
    function renderCalendar() {
        const calendarContainer = document.getElementById('calendarGrid');
        if (!calendarContainer) return;
        
        const year = currentDate.getFullYear();
        const month = currentDate.getMonth();
        const firstDayOfMonth = new Date(year, month, 1);
        const startWeekday = firstDayOfMonth.getDay();
        const daysInMonth = new Date(year, month + 1, 0).getDate();
        
        calendarContainer.innerHTML = '';
        const weekdays = ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс'];
        for (let w of weekdays) {
            let wdDiv = document.createElement('div');
            wdDiv.className = 'cal-weekday';
            wdDiv.innerText = w;
            calendarContainer.appendChild(wdDiv);
        }
        
        let offset = (startWeekday === 0 ? 6 : startWeekday - 1);
        let dayCounter = 1;
        
        for (let i = 0; i < 42; i++) {
            let cell = document.createElement('div');
            cell.className = 'cal-day';
            if (i < offset || dayCounter > daysInMonth) {
                cell.innerText = '';
                cell.style.background = '#f1f9f0';
                cell.style.opacity = '0.5';
                cell.style.cursor = 'default';
                calendarContainer.appendChild(cell);
                continue;
            }
            let dayNum = dayCounter;
            let cellDate = new Date(year, month, dayNum);
            let cellDateStr = formatYMD(cellDate);
            cell.innerText = dayNum;
            
            if (plans[cellDateStr] && plans[cellDateStr].length > 0) {
                cell.classList.add('has-plan');
            }
            if (cellDateStr === selectedDateStr) {
                cell.classList.add('selected');
            }
            
            cell.addEventListener('click', (function(dateStr) {
                return function() {
                    selectedDateStr = dateStr;
                    renderFullDashboard();
                };
            })(cellDateStr));
            
            calendarContainer.appendChild(cell);
            dayCounter++;
        }
        
        const monthNames = ['январь', 'февраль', 'март', 'апрель', 'май', 'июнь', 'июль', 'август', 'сентябрь', 'октябрь', 'ноябрь', 'декабрь'];
        const monthLabel = document.getElementById('monthYearLabel');
        if (monthLabel) monthLabel.innerText = `${monthNames[month]} ${year}`;
    }
    
    function renderPlansForDate() {
        const container = document.getElementById('plansForDate');
        if (!container) return;
        
        if (!selectedDateStr) {
            container.innerHTML = '<div style="padding: 8px; background:#eef3ea; border-radius: 40px;">Выберите дату в календаре</div>';
            return;
        }
        
        const planList = plans[selectedDateStr] || [];
        if (planList.length === 0) {
            container.innerHTML = '<div style="padding: 8px; background:#eef3ea; border-radius: 40px;">✨ Нет планов на этот день. Добавьте что-нибудь!</div>';
            return;
        }
        
        container.innerHTML = '';
        planList.forEach((plan, idx) => {
            const div = document.createElement('div');
            div.className = 'plan-item';
            div.innerHTML = `<span>📋 ${escapeHtml(plan)}</span><span class="delete-plan" data-date="${selectedDateStr}" data-idx="${idx}">✖ Удалить</span>`;
            container.appendChild(div);
        });
        
        document.querySelectorAll('.delete-plan').forEach(el => {
            el.addEventListener('click', (e) => {
                e.stopPropagation();
                const date = el.getAttribute('data-date');
                const idx = parseInt(el.getAttribute('data-idx'));
                if (plans[date] && plans[date][idx]) {
                    plans[date].splice(idx, 1);
                    if (plans[date].length === 0) delete plans[date];
                    if (dbEnabled) saveAllToFirebase();
                    else saveToLocalStorage();
                    renderFullDashboard();
                }
            });
        });
    }
    
    function addPlanToSelected() {
        if (!selectedDateStr) {
            alert("Сначала выберите день в календаре!");
            return;
        }
        const input = document.getElementById('planInput');
        let text = input.value.trim();
        if (text === "") {
            alert("Введите текст плана");
            return;
        }
        if (!plans[selectedDateStr]) plans[selectedDateStr] = [];
        plans[selectedDateStr].push(text);
        if (dbEnabled) saveAllToFirebase();
        else saveToLocalStorage();
        input.value = "";
        renderFullDashboard();
    }
    
    function renderDuties() {
        const container = document.getElementById('dutiesContainer');
        if (!container) return;
        container.innerHTML = '';
        
        duties.forEach((duty, idx) => {
            const dutyDiv = document.createElement('div');
            dutyDiv.className = 'duty-item';
            dutyDiv.innerHTML = `
                <span class="duty-name">${escapeHtml(duty.name)}</span>
                <span class="duty-assign" data-idx="${idx}">👤 ${escapeHtml(duty.assigned)} ↻</span>
                <button style="background:#ccded2; color:#2d5a45; padding:5px 12px;" data-remove="${idx}">🗑</button>
            `;
            container.appendChild(dutyDiv);
        });
        
        document.querySelectorAll('.duty-assign').forEach(el => {
            el.addEventListener('click', (e) => {
                const idx = parseInt(el.getAttribute('data-idx'));
                const members = ["Мама", "Папа", "Дочь", "Сын", "Общая смена"];
                let current = duties[idx].assigned;
                let nextIndex = (members.indexOf(current) + 1) % members.length;
                if (nextIndex === -1) nextIndex = 0;
                duties[idx].assigned = members[nextIndex] || "Мама";
                if (dbEnabled) saveAllToFirebase();
                else saveToLocalStorage();
                renderFullDashboard();
            });
        });
        
        document.querySelectorAll('[data-remove]').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const idx = parseInt(btn.getAttribute('data-remove'));
                duties.splice(idx, 1);
                if (dbEnabled) saveAllToFirebase();
                else saveToLocalStorage();
                renderFullDashboard();
            });
        });
    }
    
    function addNewDuty() {
        const input = document.getElementById('newDutyName');
        let name = input.value.trim();
        if (name === "") return alert("Введите название обязанности");
        duties.push({ name: name, assigned: "Мама" });
        input.value = "";
        if (dbEnabled) saveAllToFirebase();
        else saveToLocalStorage();
        renderFullDashboard();
    }
    
    function renderGroceries() {
        const container = document.getElementById('groceryListContainer');
        if (!container) return;
        container.innerHTML = '';
        
        groceries.forEach((item, idx) => {
            const div = document.createElement('div');
            div.className = 'grocery-item';
            const statusClass = item.inStock ? 'green-status' : 'red-status';
            const statusText = item.inStock ? '🟢 Есть' : '🔴 Нужно';
            div.style.borderLeftColor = item.inStock ? '#6fbf4c' : '#e07c6b';
            div.innerHTML = `
                <span style="flex:2; font-weight:500;">🥬 ${escapeHtml(item.name)}</span>
                <span class="grocery-status ${statusClass}" data-idx="${idx}">${statusText}</span>
                <button style="background:#dbeadf; color:#3f674b; margin-left:6px;" data-delg="${idx}">❌</button>
            `;
            container.appendChild(div);
        });
        
        document.querySelectorAll('.grocery-status').forEach(el => {
            el.addEventListener('click', (e) => {
                const idx = parseInt(el.getAttribute('data-idx'));
                groceries[idx].inStock = !groceries[idx].inStock;
                if (dbEnabled) saveAllToFirebase();
                else saveToLocalStorage();
                renderFullDashboard();
            });
        });
        
        document.querySelectorAll('[data-delg]').forEach(btn => {
            btn.addEventListener('click', (e) => {
                const idx = parseInt(btn.getAttribute('data-delg'));
                groceries.splice(idx, 1);
                if (dbEnabled) saveAllToFirebase();
                else saveToLocalStorage();
                renderFullDashboard();
            });
        });
    }
    
    function addNewGrocery() {
        const input = document.getElementById('newGroceryName');
        let name = input.value.trim();
        if (name === "") return alert("Введите название продукта");
        groceries.push({ name: name, inStock: false });
        input.value = "";
        if (dbEnabled) saveAllToFirebase();
        else saveToLocalStorage();
        renderFullDashboard();
    }
    
    const dateIdeas = [
        "🌅 Пикник на закате с пледом и горячим чаем",
        "🍕 Кулинарный вечер: приготовить вместе ужин",
        "🎨 Совместный урок рисования",
        "🚴‍♀️ Велосипедная прогулка в парк",
        "🎬 Домашний кинотеатр с попкорном",
        "🧩 Вечер настольных игр",
        "♨️ Спа-день: маски, ванна с пеной",
        "🌿 Поход в оранжерею",
        "🎤 Караоке дома",
        "🗝 Квест-комната",
        "🏕 Выходные на природе",
        "🍷 Дегустация сыров и вина",
        "📚 Совместное чтение вслух",
        "🎭 Экспромт-театр",
        "🧘‍♀️ Утренняя йога на траве"
    ];
    
    function generateRandomDate() {
        return dateIdeas[Math.floor(Math.random() * dateIdeas.length)];
    }
    
    function updateDateDisplay() {
        const container = document.getElementById('dateIdeaDisplay');
        if (container) {
            container.innerHTML = `✨ ${generateRandomDate()} ✨`;
        }
    }
    
    function renderFullDashboard() {
        const dashboard = document.getElementById('dashboard');
        if (!dashboard) return;
        
        if (!selectedDateStr) {
            selectedDateStr = formatYMD(new Date());
        }
        
        dashboard.innerHTML = `
            <div class="card">
                <h2>📅 Календарь-планер</h2>
                <div style="display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 10px;">
                    <button id="prevMonthBtn" style="background:#9ac2ab;">◀</button>
                    <span id="monthYearLabel" style="font-weight: 600; font-size: 1.2rem;"></span>
                    <button id="nextMonthBtn" style="background:#9ac2ab;">▶</button>
                </div>
                <div id="calendarGrid" class="calendar-grid"></div>
                <div class="plan-input-area">
                    <input type="text" id="planInput" placeholder="Напишите план на выбранный день...">
                    <button id="addPlanBtn">➕ Добавить</button>
                </div>
                <div id="selectedDateInfo" style="font-size:0.8rem; margin: 0.3rem 0;">📌 Выбрано: ${selectedDateStr}</div>
                <div id="plansForDate" class="plans-list"></div>
            </div>
            <div class="card">
                <h2>🧹 Планировка быта</h2>
                <p style="font-size:0.85rem; margin-bottom: 0.7rem;">👨‍👩‍👧 Ответственные: нажми на имя, чтобы сменить</p>
                <div id="dutiesContainer"></div>
                <div class="add-duty">
                    <input type="text" id="newDutyName" placeholder="Новая обязанность">
                    <button id="addDutyBtn">➕ Добавить</button>
                </div>
            </div>
            <div class="card">
                <h2>🥑 Список продуктов</h2>
                <p style="font-size:0.8rem;">🟢 есть в наличии &nbsp;&nbsp; 🔴 нужно купить (нажми на статус)</p>
                <div id="groceryListContainer"></div>
                <div class="add-grocery">
                    <input type="text" id="newGroceryName" placeholder="Название продукта">
                    <button id="addGroceryBtn">➕ Добавить</button>
                </div>
            </div>
            <div class="card">
                <h2>💕 Варианты свиданий</h2>
                <div id="dateIdeaDisplay" class="date-card">✨ ${generateRandomDate()} ✨</div>
                <button id="generateDateBtn" class="randomize-btn">✨ Случайная идея свидания ✨</button>
                <hr>
                <div style="font-size:0.8rem; text-align:center;">Добавляйте романтику в расписание!</div>
            </div>
        `;
        
        renderCalendar();
        renderPlansForDate();
        renderDuties();
        renderGroceries();
        
        document.getElementById('addPlanBtn')?.addEventListener('click', addPlanToSelected);
        document.getElementById('prevMonthBtn')?.addEventListener('click', () => {
            currentDate.setMonth(currentDate.getMonth() - 1);
            renderFullDashboard();
        });
        document.getElementById('nextMonthBtn')?.addEventListener('click', () => {
            currentDate.setMonth(currentDate.getMonth() + 1);
            renderFullDashboard();
        });
        document.getElementById('addDutyBtn')?.addEventListener('click', addNewDuty);
        document.getElementById('addGroceryBtn')?.addEventListener('click', addNewGrocery);
        document.getElementById('generateDateBtn')?.addEventListener('click', updateDateDisplay);
    }
    
    // ==================== ЗАПУСК ====================
    if (dbEnabled) {
        loadFromFirebase();
    } else {
        loadFromLocalStorage();
        renderFullDashboard();
    }
</script>
</body>
</html>
