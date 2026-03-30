<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
<title>Эко-Планинг: Семейный календарь и быт</title>
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

/* главный контейнер */
.dashboard {
max-width: 1400px;
margin: 0 auto;
display: grid;
grid-template-columns: repeat(auto-fit, minmax(360px, 1fr));
gap: 1.8rem;
align-items: start;
}

/* карточки */
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

/* календарь-планер */
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

/* обязанности */
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

/* продукты */
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

/* свидания-генератор */
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

/* ========== СТИЛИ ДЛЯ КНИГИ-МЕНЮ (разворот) ========== */
.book-spread {
background: #fef9e8;
border-radius: 1.5rem;
padding: 1.2rem;
margin-top: 0.5rem;
box-shadow: inset 0 0 0 1px #f5e7c8, 0 8px 20px rgba(60, 40, 20, 0.08);
position: relative;
}

.book-spread::before {
content: "📖";
position: absolute;
top: -12px;
left: 20px;
font-size: 2rem;
opacity: 0.5;
transform: rotate(-15deg);
}

.meal-header {
display: flex;
justify-content: space-between;
align-items: baseline;
border-bottom: 2px dashed #d9c8a7;
padding-bottom: 0.5rem;
margin-bottom: 1rem;
flex-wrap: wrap;
gap: 10px;
}

.meal-date-selector {
display: flex;
align-items: center;
gap: 12px;
background: #f2ecdc;
padding: 6px 12px;
border-radius: 60px;
}

.meal-date-selector input {
background: white;
border: 1px solid #dacba8;
border-radius: 40px;
padding: 6px 12px;
font-size: 0.85rem;
}

.meal-section {
margin-bottom: 1rem;
background: #fffcf0;
border-radius: 1rem;
padding: 0.8rem;
border-left: 4px solid #9eb87c;
}

.meal-section h4 {
font-size: 1rem;
color: #6b4c2c;
margin-bottom: 0.5rem;
display: flex;
align-items: center;
gap: 8px;
}

.meal-items {
display: flex;
flex-wrap: wrap;
gap: 8px;
margin-bottom: 10px;
align-items: center;
}

.meal-tag {
background: #e7e0cf;
border-radius: 30px;
padding: 5px 12px;
font-size: 0.8rem;
display: inline-flex;
align-items: center;
gap: 6px;
}

.meal-tag .remove-meal {
cursor: pointer;
font-weight: bold;
color: #a5512c;
margin-left: 6px;
}

.add-meal-input {
display: flex;
gap: 6px;
margin-top: 6px;
}

.add-meal-input input {
flex: 1;
padding: 6px 10px;
border-radius: 30px;
border: 1px solid #ddd0b2;
background: white;
font-size: 0.8rem;
}

.add-meal-input button {
padding: 5px 12px;
font-size: 0.7rem;
background: #9eb87c;
}

.meal-actions {
margin-top: 0.8rem;
text-align: right;
}

.icon-btn {
background: #e2d9c4;
color: #4d3e2b;
padding: 4px 12px;
font-size: 0.7rem;
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
</style>
</head>
<body>
<div class="dashboard">
<!-- БЛОК 1: Календарь-планер -->
<div class="card">
<h2>📅 Календарь-планер</h2>
<div id="calendarHeader" style="display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 10px;">
<button id="prevMonthBtn" style="background:#9ac2ab;">◀</button>
<span id="monthYearLabel" style="font-weight: 600; font-size: 1.2rem;"></span>
<button id="nextMonthBtn" style="background:#9ac2ab;">▶</button>
</div>
<div id="calendarGrid" class="calendar-grid"></div>
<div class="plan-input-area">
<input type="text" id="planInput" placeholder="Напишите план на выбранный день...">
<button id="addPlanBtn">➕ Добавить</button>
</div>
<div id="selectedDateInfo" style="font-size:0.8rem; margin: 0.3rem 0 0.3rem 0; color:#3f6e58;">Выбран: --</div>
<div id="plansForDate" class="plans-list"></div>
</div>

<!-- БЛОК 2: Планировка быта (только Саша и Артем) -->
<div class="card">
<h2>🧹 Планировка быта</h2>
<p style="font-size:0.85rem; margin-bottom: 0.7rem;">👥 Ответственные: <strong>Саша</strong> и <strong>Артем</strong> — нажми на имя, чтобы переключить</p>
<div id="dutiesContainer"></div>
<div class="add-duty">
<input type="text" id="newDutyName" placeholder="Новая обязанность, например: Вынос мусора">
<button id="addDutyBtn">➕ Добавить</button>
</div>
</div>

<!-- БЛОК 3: Список продуктов с переключателем (зелёный/красный) -->
<div class="card">
<h2>🥑 Список продуктов</h2>
<p style="font-size:0.8rem;">🟢 есть в наличии &nbsp;&nbsp; 🔴 нужно купить (нажми на статус)</p>
<div id="groceryListContainer"></div>
<div class="add-grocery">
<input type="text" id="newGroceryName" placeholder="Название продукта">
<button id="addGroceryBtn">➕ Добавить</button>
</div>
</div>

<!-- БЛОК 4: Генератор идей свиданий -->
<div class="card">
<h2>💕 Варианты свиданий</h2>
<div id="dateIdeaDisplay" class="date-card">
🎲 Нажмите на кнопку, чтобы получить идею
</div>
<button id="generateDateBtn" class="randomize-btn">✨ Случайная идея свидания ✨</button>
<hr>
<div style="font-size:0.8rem; text-align:center;">Добавляйте романтику в расписание!</div>
</div>

<!-- БЛОК 5: КУЛИНАРНАЯ КНИГА - МЕНЮ НА ДЕНЬ (разворот) -->
<div class="card">
<h2>📖 Кулинарная книга</h2>
<p style="font-size:0.8rem; margin-bottom: 0.5rem;">🍽 Планируйте меню на каждый день — полноценный разворот для завтрака, обеда и ужина</p>
<div class="book-spread" id="menuBookSpread">
<div class="meal-header">
<span style="font-weight: 600; font-size: 1.1rem;">📅 Меню на день</span>
<div class="meal-date-selector">
<label style="font-size:0.75rem;">Выбрать дату:</label>
<input type="date" id="menuDatePicker" value="">
</div>
</div>

<div id="dailyMenuContainer">
<!-- Завтрак -->
<div class="meal-section">
<h4>🌞 ЗАВТРАК</h4>
<div id="breakfastItems" class="meal-items"></div>
<div class="add-meal-input">
<input type="text" id="newBreakfastItem" placeholder="Блюдо, например: Овсянка с ягодами">
<button id="addBreakfastBtn">➕ Добавить</button>
</div>
</div>

<!-- Обед -->
<div class="meal-section">
<h4>🍲 ОБЕД</h4>
<div id="lunchItems" class="meal-items"></div>
<div class="add-meal-input">
<input type="text" id="newLunchItem" placeholder="Блюдо, например: Суп с фрикадельками">
<button id="addLunchBtn">➕ Добавить</button>
</div>
</div>

<!-- Ужин -->
<div class="meal-section">
<h4>🌙 УЖИН</h4>
<div id="dinnerItems" class="meal-items"></div>
<div class="add-meal-input">
<input type="text" id="newDinnerItem" placeholder="Блюдо, например: Запеченная рыба">
<button id="addDinnerBtn">➕ Добавить</button>
</div>
</div>
</div>
<div class="meal-actions">
<button id="copyTodayMenuBtn" class="icon-btn" style="background:#d4e0ca;">📋 Скопировать сегодняшнее меню</button>
</div>
</div>
</div>
</div>
<footer>🌿 Эко-планинг — планируйте дела, быт, меню и заботу друг о друге в светло-зелёной атмосфере</footer>

<script>
// ======================== 1. КАЛЕНДАРЬ-ПЛАНЕР ========================
let currentDate = new Date();
let selectedDateStr = "";
let plans = JSON.parse(localStorage.getItem('ecoPlans')) || {};

function savePlansToLocal() { localStorage.setItem('ecoPlans', JSON.stringify(plans)); }
function formatYMD(date) {
let y = date.getFullYear();
let m = String(date.getMonth() + 1).padStart(2,'0');
let d = String(date.getDate()).padStart(2,'0');
return `${y}-${m}-${d}`;
}

function renderCalendar() {
const year = currentDate.getFullYear(), month = currentDate.getMonth();
const firstDayOfMonth = new Date(year, month, 1);
const startWeekday = firstDayOfMonth.getDay();
const daysInMonth = new Date(year, month+1, 0).getDate();
const calendarGrid = document.getElementById('calendarGrid');
calendarGrid.innerHTML = '';
const weekdays = ['Пн','Вт','Ср','Чт','Пт','Сб','Вс'];
weekdays.forEach(w => { let d=document.createElement('div'); d.className='cal-weekday'; d.innerText=w; calendarGrid.appendChild(d); });
let offset = (startWeekday === 0 ? 6 : startWeekday-1);
let dayCounter = 1;
for(let i=0;i<42;i++){
let cell=document.createElement('div'); cell.className='cal-day';
if(i<offset || dayCounter>daysInMonth){
cell.innerText=''; cell.style.background='#f1f9f0'; cell.style.opacity='0.5'; cell.style.cursor='default';
calendarGrid.appendChild(cell); continue;
}
let dayNum=dayCounter;
let cellDate=new Date(year, month, dayNum);
let cellDateStr=formatYMD(cellDate);
cell.innerText=dayNum;
if(plans[cellDateStr] && plans[cellDateStr].length) cell.classList.add('has-plan');
if(cellDateStr===selectedDateStr) cell.classList.add('selected');
cell.addEventListener('click', (function(ds){ return function(){ selectedDateStr=ds; renderCalendar(); renderPlansForDate(); document.getElementById('selectedDateInfo').innerText=`📌 Выбрано: ${ds}`; }; })(cellDateStr));
calendarGrid.appendChild(cell);
dayCounter++;
}
const monthNames=['январь','февраль','март','апрель','май','июнь','июль','август','сентябрь','октябрь','ноябрь','декабрь'];
document.getElementById('monthYearLabel').innerText=`${monthNames[month]} ${year}`;
}
function renderPlansForDate(){
const container=document.getElementById('plansForDate');
if(!selectedDateStr){ container.innerHTML='<div style="padding:8px; background:#eef3ea; border-radius:40px;">Выберите дату</div>'; return; }
const list=plans[selectedDateStr]||[];
if(list.length===0){ container.innerHTML='<div style="padding:8px; background:#eef3ea; border-radius:40px;">✨ Нет планов</div>'; return; }
container.innerHTML='';
list.forEach((p,idx)=>{
let div=document.createElement('div'); div.className='plan-item';
div.innerHTML=`<span>📋 ${escapeHtml(p)}</span><span class="delete-plan" data-date="${selectedDateStr}" data-idx="${idx}">✖ Удалить</span>`;
container.appendChild(div);
});
document.querySelectorAll('.delete-plan').forEach(el=>{
el.addEventListener('click',(e)=>{
e.stopPropagation();
let date=el.getAttribute('data-date'), idx=parseInt(el.getAttribute('data-idx'));
if(plans[date] && plans[date][idx]){ plans[date].splice(idx,1); if(plans[date].length===0) delete plans[date]; savePlansToLocal(); renderCalendar(); renderPlansForDate(); }
});
});
}
function escapeHtml(str){ return str.replace(/[&<>]/g, function(m){ if(m==='&') return '&amp;'; if(m==='<') return '&lt;'; if(m==='>') return '&gt;'; return m;}); }
function addPlanToSelected(){
if(!selectedDateStr){ alert("Выберите день"); return; }
let text=document.getElementById('planInput').value.trim();
if(!text) { alert("Введите план"); return; }
if(!plans[selectedDateStr]) plans[selectedDateStr]=[];
plans[selectedDateStr].push(text);
savePlansToLocal();
document.getElementById('planInput').value="";
renderCalendar(); renderPlansForDate();
}

// ======================== 2. БЫТ: ТОЛЬКО САША И АРТЕМ ========================
let duties = JSON.parse(localStorage.getItem('ecoDuties')) || [
{ name: "🍽 Мытьё посуды", assigned: "Саша" },
{ name: "🧹 Уборка в гостиной", assigned: "Артем" },
{ name: "🐕 Выгул собаки", assigned: "Саша" },
{ name: "🧺 Стирка", assigned: "Артем" },
{ name: "🗑 Вынос мусора", assigned: "Саша" }
];
const allowedMembers = ["Саша", "Артем"];
function saveDuties(){ localStorage.setItem('ecoDuties',JSON.stringify(duties)); renderDuties(); }
function renderDuties(){
const container=document.getElementById('dutiesContainer'); if(!container) return;
container.innerHTML='';
duties.forEach((duty,idx)=>{
const div=document.createElement('div'); div.className='duty-item';
div.innerHTML=`<span class="duty-name">${escapeHtml(duty.name)}</span><span class="duty-assign" data-idx="${idx}">👤 ${escapeHtml(duty.assigned)} ↻</span><button style="background:#ccded2; color:#2d5a45;" data-remove="${idx}">🗑</button>`;
container.appendChild(div);
});
document.querySelectorAll('.duty-assign').forEach(el=>{
el.addEventListener('click',()=>{
const idx=parseInt(el.getAttribute('data-idx'));
let current=duties[idx].assigned;
let curIdx=allowedMembers.indexOf(current);
let nextIdx=(curIdx+1)%allowedMembers.length;
if(curIdx===-1) nextIdx=0;
duties[idx].assigned=allowedMembers[nextIdx];
saveDuties();
});
});
document.querySelectorAll('[data-remove]').forEach(btn=>{
btn.addEventListener('click',()=>{
const idx=parseInt(btn.getAttribute('data-remove'));
duties.splice(idx,1); saveDuties();
});
});
}
function addNewDuty(){
let name=document.getElementById('newDutyName').value.trim();
if(!name) return alert("Введите название");
duties.push({ name: name, assigned: "Саша" });
document.getElementById('newDutyName').value="";
saveDuties();
}

// ======================== 3. ПРОДУКТЫ ========================
let groceries = JSON.parse(localStorage.getItem('ecoGroceries')) || [
{ name:"Молоко", inStock:true },{ name:"Хлеб", inStock:false },{ name:"Яйца", inStock:true },
{ name:"Помидоры", inStock:false },{ name:"Яблоки", inStock:true }
];
function saveGroceries(){ localStorage.setItem('ecoGroceries',JSON.stringify(groceries)); renderGroceries(); }
function renderGroceries(){
const container=document.getElementById('groceryListContainer'); if(!container) return;
container.innerHTML='';
groceries.forEach((item,idx)=>{
const div=document.createElement('div'); div.className='grocery-item';
const statusClass=item.inStock?'green-status':'red-status';
const statusText=item.inStock?'🟢 Есть':'🔴 Нужно';
div.style.borderLeftColor=item.inStock?'#6fbf4c':'#e07c6b';
div.innerHTML=`<span style="flex:2;">🥬 ${escapeHtml(item.name)}</span><span class="grocery-status ${statusClass}" data-idx="${idx}">${statusText}</span><button data-delg="${idx}" style="background:#dbeadf;">❌</button>`;
container.appendChild(div);
});
document.querySelectorAll('.grocery-status').forEach(el=>{
el.addEventListener('click',()=>{ let idx=parseInt(el.getAttribute('data-idx')); groceries[idx].inStock=!groceries[idx].inStock; saveGroceries(); });
});
document.querySelectorAll('[data-delg]').forEach(btn=>{
btn.addEventListener('click',()=>{ let idx=parseInt(btn.getAttribute('data-delg')); groceries.splice(idx,1); saveGroceries(); });
});
}
function addNewGrocery(){
let name=document.getElementById('newGroceryName').value.trim();
if(!name) return alert("Введите продукт");
groceries.push({ name: name, inStock: false });
document.getElementById('newGroceryName').value="";
saveGroceries();
}

// ======================== 4. СВИДАНИЯ ========================
const dateIdeas=["🌅 Пикник на закате","🍕 Кулинарный вечер","🎨 Совместный урок рисования","🚴‍♀️ Велосипедная прогулка","🎬 Домашний кинотеатр","🧩 Настольные игры","♨️ Спа-день","🌿 Поход в оранжерею","🎤 Караоке","🗝 Квест-комната","🏕 Выходные на природе","🍷 Дегустация сыров","📚 Чтение вслух","🎭 Экспромт-театр","🧘‍♀️ Утренняя йога"];
function generateRandomDate(){ return dateIdeas[Math.floor(Math.random()*dateIdeas.length)]; }
function updateDateDisplay(){ document.getElementById('dateIdeaDisplay').innerHTML=`✨ ${generateRandomDate()} ✨`; }

// ======================== 5. КУЛИНАРНАЯ КНИГА (МЕНЮ НА ДЕНЬ) ========================
let dailyMenus = JSON.parse(localStorage.getItem('ecoDailyMenus')) || {};
// структура: dailyMenus["2025-03-30"] = { breakfast: ["каша"], lunch: ["суп"], dinner: ["котлета"] }

function saveMenusToLocal(){ localStorage.setItem('ecoDailyMenus', JSON.stringify(dailyMenus)); }

function getCurrentMenuDate(){
let picker = document.getElementById('menuDatePicker');
if(picker.value) return picker.value;
let today = formatYMD(new Date());
picker.value = today;
return today;
}

function renderMenuForDate(dateStr){
if(!dateStr) return;
if(!dailyMenus[dateStr]) dailyMenus[dateStr] = { breakfast: [], lunch: [], dinner: [] };
const menu = dailyMenus[dateStr];
// завтрак
const breakfastDiv = document.getElementById('breakfastItems');
breakfastDiv.innerHTML = '';
menu.breakfast.forEach((item, idx) => {
let tag = document.createElement('span'); tag.className='meal-tag';
tag.innerHTML = `${escapeHtml(item)} <span class="remove-meal" data-type="breakfast" data-idx="${idx}">✖</span>`;
breakfastDiv.appendChild(tag);
});
// обед
const lunchDiv = document.getElementById('lunchItems');
lunchDiv.innerHTML = '';
menu.lunch.forEach((item, idx) => {
let tag = document.createElement('span'); tag.className='meal-tag';
tag.innerHTML = `${escapeHtml(item)} <span class="remove-meal" data-type="lunch" data-idx="${idx}">✖</span>`;
lunchDiv.appendChild(tag);
});
// ужин
const dinnerDiv = document.getElementById('dinnerItems');
dinnerDiv.innerHTML = '';
menu.dinner.forEach((item, idx) => {
let tag = document.createElement('span'); tag.className='meal-tag';
tag.innerHTML = `${escapeHtml(item)} <span class="remove-meal" data-type="dinner" data-idx="${idx}">✖</span>`;
dinnerDiv.appendChild(tag);
});

// делегирование удаления
document.querySelectorAll('.remove-meal').forEach(el => {
el.removeEventListener('click', handleRemoveMeal);
el.addEventListener('click', handleRemoveMeal);
});
}

function handleRemoveMeal(e){
let target = e.currentTarget;
let type = target.getAttribute('data-type');
let idx = parseInt(target.getAttribute('data-idx'));
let currentDate = getCurrentMenuDate();
if(!dailyMenus[currentDate]) return;
if(type === 'breakfast') dailyMenus[currentDate].breakfast.splice(idx,1);
else if(type === 'lunch') dailyMenus[currentDate].lunch.splice(idx,1);
else if(type === 'dinner') dailyMenus[currentDate].dinner.splice(idx,1);
if(dailyMenus[currentDate].breakfast.length===0 && dailyMenus[currentDate].lunch.length===0 && dailyMenus[currentDate].dinner.length===0){
delete dailyMenus[currentDate];
}
saveMenusToLocal();
renderMenuForDate(currentDate);
}

function addMealItem(type){
let currentDate = getCurrentMenuDate();
if(!dailyMenus[currentDate]) dailyMenus[currentDate] = { breakfast: [], lunch: [], dinner: [] };
let inputId = '', inputValue = '';
if(type === 'breakfast') { inputId = 'newBreakfastItem'; inputValue = document.getElementById('newBreakfastItem').value.trim(); }
else if(type === 'lunch') { inputId = 'newLunchItem'; inputValue = document.getElementById('newLunchItem').value.trim(); }
else if(type === 'dinner') { inputId = 'newDinnerItem'; inputValue = document.getElementById('newDinnerItem').value.trim(); }
if(!inputValue) { alert("Введите блюдо"); return; }
dailyMenus[currentDate][type].push(inputValue);
saveMenusToLocal();
renderMenuForDate(currentDate);
if(type === 'breakfast') document.getElementById('newBreakfastItem').value = '';
else if(type === 'lunch') document.getElementById('newLunchItem').value = '';
else if(type === 'dinner') document.getElementById('newDinnerItem').value = '';
}

function copyTodayMenu(){
let todayStr = formatYMD(new Date());
let currentDate = getCurrentMenuDate();
if(!dailyMenus[currentDate]) { alert("На выбранный день нет меню, нечего копировать"); return; }
let sourceMenu = dailyMenus[currentDate];
dailyMenus[todayStr] = JSON.parse(JSON.stringify(sourceMenu));
saveMenusToLocal();
alert(`Меню скопировано на сегодня (${todayStr})!`);
if(getCurrentMenuDate() === todayStr) renderMenuForDate(todayStr);
}

function onMenuDateChange(){
let newDate = document.getElementById('menuDatePicker').value;
if(newDate){
renderMenuForDate(newDate);
}
}

// ======================== ИНИЦИАЛИЗАЦИЯ ========================
function init(){
if(!selectedDateStr) selectedDateStr = formatYMD(new Date());
renderCalendar();
renderPlansForDate();
document.getElementById('selectedDateInfo').innerText=`📌 Выбрано: ${selectedDateStr}`;
document.getElementById('addPlanBtn').addEventListener('click', addPlanToSelected);
document.getElementById('prevMonthBtn').addEventListener('click',()=>{ currentDate.setMonth(currentDate.getMonth()-1); renderCalendar(); renderPlansForDate(); });
document.getElementById('nextMonthBtn').addEventListener('click',()=>{ currentDate.setMonth(currentDate.getMonth()+1); renderCalendar(); renderPlansForDate(); });
renderDuties();
document.getElementById('addDutyBtn').addEventListener('click', addNewDuty);
renderGroceries();
document.getElementById('addGroceryBtn').addEventListener('click', addNewGrocery);
document.getElementById('generateDateBtn').addEventListener('click', updateDateDisplay);
document.getElementById('dateIdeaDisplay').innerHTML=`💚 ${generateRandomDate()} 💚`;

// инициализация кулинарной книги
let todayDefault = formatYMD(new Date());
let menuPicker = document.getElementById('menuDatePicker');
menuPicker.value = todayDefault;
renderMenuForDate(todayDefault);
menuPicker.addEventListener('change', onMenuDateChange);
document.getElementById('addBreakfastBtn').addEventListener('click', ()=> addMealItem('breakfast'));
document.getElementById('addLunchBtn').addEventListener('click', ()=> addMealItem('lunch'));
document.getElementById('addDinnerBtn').addEventListener('click', ()=> addMealItem('dinner'));
document.getElementById('copyTodayMenuBtn').addEventListener('click', copyTodayMenu);
}
init();
</script>
</body>
</html>
