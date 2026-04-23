
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Опросник для друзей — Выбор стиля</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Inter', system-ui, sans-serif; }
    
    .card {
      transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    }
    .card:hover {
      transform: translateY(-6px) scale(1.04);
      box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.3);
    }
    .card.selected {
      border-color: #10b981;
      background-color: #052e16;
      box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.3);
      transform: scale(1.05);
    }
    .card.selected .checkmark {
      opacity: 1;
    }
  </style>
</head>
<body class="bg-gradient-to-br from-zinc-950 via-zinc-900 to-black text-white min-h-screen pb-12">
<div class="max-w-4xl mx-auto px-4 pt-8">

  <!-- Выбор пола -->
  <div id="gender-screen">
    <div class="text-center mb-12">
      <h1 class="text-6xl font-bold mb-4 bg-gradient-to-r from-white via-emerald-300 to-teal-300 bg-clip-text text-transparent">
        Привет! 👋
      </h1>
      <p class="text-2xl text-zinc-400">Давай подберём тебе крутой стиль</p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-lg mx-auto">
      <button onclick="selectGender('male')" class="bg-zinc-900 hover:bg-zinc-800 border border-zinc-700 hover:border-emerald-500 rounded-3xl p-10 shadow-2xl flex flex-col items-center transition-all group">
        <div class="text-8xl mb-6 transition-transform group-hover:scale-110">👨</div>
        <h2 class="text-3xl font-semibold">Я мужчина</h2>
      </button>
      <button onclick="selectGender('female')" class="bg-zinc-900 hover:bg-zinc-800 border border-zinc-700 hover:border-pink-500 rounded-3xl p-10 shadow-2xl flex flex-col items-center transition-all group">
        <div class="text-8xl mb-6 transition-transform group-hover:scale-110">👩</div>
        <h2 class="text-3xl font-semibold">Я женщина</h2>
      </button>
    </div>
  </div>

  <!-- Опрос -->
  <div id="survey-screen" class="hidden">
    <div class="flex justify-between items-center mb-8">
      <button onclick="goBackToGender()" class="text-zinc-400 hover:text-white flex items-center gap-2 text-lg">← Назад</button>
      <h1 id="survey-title" class="text-3xl font-bold"></h1>
      <div id="gender-badge" class="px-6 py-2 rounded-3xl text-sm font-medium"></div>
    </div>
    <div id="sections-container" class="space-y-12"></div>
    
    <div class="text-center mt-12">
      <button onclick="finishSurvey()" 
        class="bg-white text-black hover:bg-emerald-400 hover:text-white text-xl font-semibold px-16 py-7 rounded-3xl shadow-xl transition-all active:scale-95">
        Завершить опрос →
      </button>
    </div>
  </div>

  <!-- Результаты -->
  <div id="result-screen" class="hidden">
    <div class="text-center mb-10">
      <div class="mx-auto w-24 h-24 bg-gradient-to-br from-emerald-400 to-teal-500 rounded-3xl flex items-center justify-center text-6xl shadow-xl mb-6">🎉</div>
      <h1 class="text-5xl font-bold mb-2">Твой стиль готов!</h1>
      <p id="result-subtitle" class="text-xl text-zinc-400"></p>
    </div>

    <div id="result-content" class="space-y-10"></div>

    <div class="flex flex-wrap gap-4 justify-center mt-12">
      <button onclick="copyResults()" class="flex items-center gap-3 bg-zinc-800 hover:bg-zinc-700 px-8 py-5 rounded-3xl font-medium transition-all">📋 Скопировать</button>
      <button onclick="shareResults()" class="flex items-center gap-3 bg-emerald-600 hover:bg-emerald-500 px-10 py-5 rounded-3xl font-semibold transition-all">🚀 Поделиться с друзьями</button>
      <button onclick="restartAll()" class="flex items-center gap-3 bg-zinc-800 hover:bg-zinc-700 px-8 py-5 rounded-3xl font-medium transition-all">Начать заново</button>
    </div>
  </div>
</div>

<script>
// ==================== ДАННЫЕ ====================
const surveyData = {
  male: {
    tshirts: [{id:1,name:"Чёрная классика Nike"},{id:2,name:"Белая Oversize"},{id:3,name:"С принтом Supreme"},{id:4,name:"Синяя с длинным рукавом"},{id:5,name:"Зелёная Adidas"},{id:6,name:"Красная с логотипом"},{id:7,name:"Серая меланж"},{id:8,name:"Жёлтая яркая"},{id:9,name:"Полосатая"},{id:10,name:"С вышивкой"}],
    shorts: [{id:1,name:"Чёрные карго"},{id:2,name:"Синие джинсовые"},{id:3,name:"Белые спортивные"},{id:4,name:"Зелёные хаки"},{id:5,name:"Красные баскетбольные"},{id:6,name:"Серые трикотажные"},{id:7,name:"С принтом"},{id:8,name:"Короткие беговые"},{id:9,name:"Камуфляж"},{id:10,name:"С карманами"}],
    sneakers: [{id:1,name:"Nike Air Force 1"},{id:2,name:"Adidas Stan Smith"},{id:3,name:"Puma RS-X"},{id:4,name:"New Balance 550"},{id:5,name:"Nike Dunk Low"},{id:6,name:"Adidas Gazelle"},{id:7,name:"Jordan 1 Low"},{id:8,name:"Vans Old Skool"},{id:9,name:"Reebok Club C"},{id:10,name:"Converse Chuck 70"}]
  },
  female: {
    tshirts: [{id:1,name:"Чёрный кроп-топ"},{id:2,name:"Белая oversize"},{id:3,name:"Розовая с принтом"},{id:4,name:"Бежевая базовая"},{id:5,name:"Синяя с вырезом"},{id:6,name:"Жёлтая яркая"},{id:7,name:"Серая меланж"},{id:8,name:"С цветочным принтом"},{id:9,name:"С блестками"},{id:10,name:"Полосатая"}],
    shorts: [{id:1,name:"Чёрные велосипедки"},{id:2,name:"Деним мини-шорты"},{id:3,name:"Белые льняные"},{id:4,name:"Розовые спортивные"},{id:5,name:"Джинсовые с высокой посадкой"},{id:6,name:"Серые трикотажные"},{id:7,name:"С цветочным принтом"},{id:8,name:"Короткие пляжные"},{id:9,name:"Камуфляж"},{id:10,name:"С карманами"}],
    sneakers: [{id:1,name:"Nike Air Force 1"},{id:2,name:"Adidas Samba"},{id:3,name:"Puma Cali"},{id:4,name:"New Balance 530"},{id:5,name:"Nike Dunk Low"},{id:6,name:"Adidas Gazelle"},{id:7,name:"Converse Chuck Taylor"},{id:8,name:"Vans Old Skool"},{id:9,name:"Reebok Club C"},{id:10,name:"On Cloud 5"}]
  }
};

let currentGender = null;
let selections = { tshirts: [], shorts: [], sneakers: [] };

// ==================== ФУНКЦИИ ====================
function selectGender(gender) {
  currentGender = gender;
  document.getElementById('gender-screen').classList.add('hidden');
  document.getElementById('survey-screen').classList.remove('hidden');
  
  document.getElementById('survey-title').textContent = gender === 'male' ? 'Мужской стиль 🔥' : 'Женский стиль ✨';
  const badge = document.getElementById('gender-badge');
  badge.innerHTML = gender === 'male' ? '👨 Мужчина' : '👩 Женщина';
  badge.className = gender === 'male' 
    ? 'px-6 py-2 bg-emerald-900/50 text-emerald-400 rounded-3xl text-sm font-medium' 
    : 'px-6 py-2 bg-pink-900/50 text-pink-400 rounded-3xl text-sm font-medium';
  
  renderSurvey();
}

function renderSurvey() {
  const container = document.getElementById('sections-container');
  container.innerHTML = '';

  const sections = [
    {key: 'tshirts', title: '1. Футболки', emoji: '👕'},
    {key: 'shorts', title: '2. Шорты', emoji: '🩳'},
    {key: 'sneakers', title: '3. Кроссовки', emoji: '👟'}
  ];

  sections.forEach(section => {
    const items = surveyData[currentGender][section.key];
    const selected = selections[section.key];

    let html = `
      <div class="bg-zinc-900/70 backdrop-blur-xl border border-zinc-700 rounded-3xl p-8">
        <div class="flex justify-between items-center mb-8">
          <h2 class="text-2xl font-semibold flex items-center gap-4">${section.emoji} ${section.title}</h2>
          <div class="text-right">
            <span class="text-4xl font-bold text-emerald-400">${selected.length}</span>
            <span class="text-zinc-500"> / 5</span>
          </div>
        </div>
        <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-5 gap-4">
    `;

    items.forEach(item => {
      const isSelected = selected.includes(item.id);
      html += `
        <div onclick="toggleSelect('${section.key}', ${item.id})" 
          class="card cursor-pointer border-2 ${isSelected ? 'selected' : 'border-zinc-700 hover:border-zinc-500'} 
                 rounded-2xl p-5 text-center bg-zinc-900 relative overflow-hidden">
          <div class="text-6xl mb-4">${section.emoji}</div>
          <p class="font-medium text-sm leading-tight min-h-[2.5rem]">${item.name}</p>
          
          ${isSelected ? `
            <div class="checkmark absolute top-3 right-3 w-6 h-6 bg-emerald-500 text-white rounded-full flex items-center justify-center text-xs font-bold shadow">
              ✓
            </div>
          ` : ''}
        </div>`;
    });

    html += `</div></div>`;
    container.innerHTML += html;
  });
}

function toggleSelect(category, id) {
  const arr = selections[category];
  if (arr.includes(id)) {
    selections[category] = arr.filter(x => x !== id);
  } else if (arr.length < 5) {
    arr.push(id);
  } else {
    alert("Можно выбрать максимум 5 позиций в каждой категории!");
    return;
  }
  renderSurvey();
}

// Остальные функции (finishSurvey, renderResults, shareResults и т.д.) оставил без изменений
function finishSurvey() {
  if (Object.values(selections).some(arr => arr.length !== 5)) {
    alert("❗ Выбери ровно 5 позиций в каждой категории!");
    return;
  }
  document.getElementById('survey-screen').classList.add('hidden');
  document.getElementById('result-screen').classList.remove('hidden');
  renderResults();
}

function renderResults() {
  const container = document.getElementById('result-content');
  const subtitle = document.getElementById('result-subtitle');
  subtitle.textContent = currentGender === 'male' ? 'Твой мужской вайб 🔥' : 'Твой женский вайб ✨';

  let html = `<div class="space-y-12">`;

  const categories = [
    {key: 'tshirts', title: 'Футболки 👕'},
    {key: 'shorts', title: 'Шорты 🩳'},
    {key: 'sneakers', title: 'Кроссовки 👟'}
  ];

  categories.forEach(cat => {
    const selectedIds = selections[cat.key];
    const items = surveyData[currentGender][cat.key];
    const names = selectedIds.map(id => items.find(i => i.id === id).name);

    html += `
      <div class="bg-zinc-900/70 border border-zinc-700 rounded-3xl p-8">
        <h3 class="font-semibold text-2xl mb-6 text-emerald-400">${cat.title}</h3>
        <div class="flex flex-wrap gap-3">
          ${names.map(name => `
            <div class="bg-zinc-800 border border-emerald-900 px-6 py-4 rounded-2xl font-medium text-sm">
              ${name}
            </div>
          `).join('')}
        </div>
      </div>`;
  });

  html += `</div>`;
  container.innerHTML = html;
}

async function shareResults() {
  let text = currentGender === 'male' ? "🔥 Мой мужской стиль:\n\n" : "✨ Мой женский стиль:\n\n";
  const categories = [
    {key: 'tshirts', title: '👕 Футболки'},
    {key: 'shorts', title: '🩳 Шорты'},
    {key: 'sneakers', title: '👟 Кроссовки'}
  ];

  categories.forEach(cat => {
    const names = selections[cat.key].map(id => 
      surveyData[currentGender][cat.key].find(i => i.id === id).name
    );
    text += `${cat.title}:\n${names.join('\n')}\n\n`;
  });

  text += "\nСделано в стильном опроснике 😉";

  if (navigator.share) {
    await navigator.share({ title: 'Мой стиль одежды', text: text });
  } else {
    navigator.clipboard.writeText(text).then(() => alert("✅ Скопировано! Отправляй друзьям"));
  }
}

function copyResults() { shareResults(); }
function goBackToGender() {
  document.getElementById('survey-screen').classList.add('hidden');
  document.getElementById('gender-screen').classList.remove('hidden');
}
function restartAll() {
  selections = { tshirts: [], shorts: [], sneakers: [] };
  document.getElementById('result-screen').classList.add('hidden');
  document.getElementById('gender-screen').classList.remove('hidden');
}

window.onload = () => console.log('%cОпросник обновлён 🚀', 'color:#10b981; font-weight:600');
</script>
</body>
</html>
