# ConfigHub532.github.io
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Справочник по конфигурации ПК — подбор комплектующих</title>
<style>
  :root {
    --bg: #0f1419;
    --panel: #1a2129;
    --panel2: #232c36;
    --accent: #00d4aa;
    --accent2: #0099ff;
    --text: #e8eef3;
    --muted: #8b98a5;
    --border: #2d3844;
    --danger: #ff5c5c;
    --warn: #ffb84d;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Segoe UI', system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    line-height: 1.5;
  }
  header {
    background: linear-gradient(135deg, #1a2129, #0f1419);
    border-bottom: 2px solid var(--accent);
    padding: 24px 20px;
    text-align: center;
  }
  header h1 {
    font-size: 1.8rem;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 6px;
  }
  header p { color: var(--muted); font-size: 0.95rem; }

  .container {
    max-width: 1280px;
    margin: 0 auto;
    padding: 24px 16px;
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 20px;
  }
  @media (max-width: 900px) {
    .container { grid-template-columns: 1fr; }
  }

  .catalog { display: flex; flex-direction: column; gap: 20px; }

  .category {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
  }
  .category-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 14px 18px;
    background: var(--panel2);
    cursor: pointer;
    user-select: none;
  }
  .category-header h2 {
    font-size: 1.05rem;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .category-header .icon { color: var(--accent); }
  .category-header .chevron {
    color: var(--muted);
    transition: transform 0.25s;
  }
  .category.open .chevron { transform: rotate(180deg); }

  .items {
    display: none;
    padding: 10px;
    flex-direction: column;
    gap: 8px;
  }
  .category.open .items { display: flex; }

  .item {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 10px;
    align-items: center;
    padding: 12px 14px;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 8px;
    transition: border-color 0.2s, transform 0.1s;
  }
  .item:hover { border-color: var(--accent); }
  .item.selected { border-color: var(--accent); box-shadow: 0 0 0 1px var(--accent) inset; }
  .item-info .name { font-weight: 600; font-size: 0.95rem; }
  .item-info .specs { color: var(--muted); font-size: 0.82rem; margin-top: 2px; }
  .item-right { display: flex; align-items: center; gap: 12px; }
  .price { color: var(--accent); font-weight: 700; white-space: nowrap; font-size: 0.95rem; }

  .btn {
    background: var(--panel2);
    color: var(--text);
    border: 1px solid var(--border);
    padding: 7px 14px;
    border-radius: 7px;
    cursor: pointer;
    font-size: 0.85rem;
    font-weight: 600;
    transition: all 0.2s;
    white-space: nowrap;
  }
  .btn:hover { border-color: var(--accent); color: var(--accent); }
  .item.selected .btn-add {
    background: var(--accent);
    color: #0f1419;
    border-color: var(--accent);
  }
  .btn-reset {
    width: 100%;
    padding: 11px;
    background: transparent;
    border: 1px solid var(--danger);
    color: var(--danger);
    font-weight: 600;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s;
  }
  .btn-reset:hover { background: var(--danger); color: #fff; }

  aside { position: sticky; top: 20px; height: fit-content; }
  .build-panel {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px;
    }
  .build-panel h2 {
    font-size: 1.1rem;
    margin-bottom: 14px;
    padding-bottom: 10px;
    border-bottom: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .build-list { display: flex; flex-direction: column; gap: 8px; min-height: 40px; }
  .empty-msg { color: var(--muted); font-size: 0.85rem; text-align: center; padding: 14px 0; }
  .build-row {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    gap: 8px;
    padding: 8px 10px;
    background: var(--bg);
    border-radius: 7px;
    border-left: 3px solid var(--accent);
    font-size: 0.85rem;
  }
  .build-row .br-cat { color: var(--muted); font-size: 0.75rem; }
  .build-row .br-name { font-weight: 600; }
  .build-row .br-price { color: var(--accent); font-weight: 700; white-space: nowrap; }
  .br-remove {
    background: none; border: none; color: var(--muted); cursor: pointer;
    font-size: 1rem; padding: 0 2px; line-height: 1;
  }
  .br-remove:hover { color: var(--danger); }

  .total {
    display: flex; justify-content: space-between; align-items: center;
    margin: 16px 0 12px; padding-top: 14px; border-top: 1px solid var(--border);
    font-size: 1.05rem; font-weight: 700;
  }
  .total .sum { color: var(--accent); font-size: 1.35rem; }

  .compat {
    margin-top: 12px; padding: 10px 12px; border-radius: 8px;
    font-size: 0.82rem; display: none;
  }
  .compat.show { display: block; }
  .compat.ok { background: rgba(0,212,170,0.12); border: 1px solid var(--accent); color: var(--accent); }
  .compat.warn { background: rgba(255,184,77,0.12); border: 1px solid var(--warn); color: var(--warn); }
  .compat.bad { background: rgba(255,92,92,0.12); border: 1px solid var(--danger); color: var(--danger); }

  .saved-builds { margin-top: 16px; }
  .saved-builds h3 { font-size: 0.9rem; color: var(--muted); margin-bottom: 8px; }
  .saved-item {
    display: flex; justify-content: space-between; align-items: center;
    padding: 8px 10px; background: var(--bg); border-radius: 7px;
    margin-bottom: 6px; font-size: 0.82rem; cursor: pointer;
    border: 1px solid var(--border);
  }
  .saved-item:hover { border-color: var(--accent2); }
  .saved-item .s-price { color: var(--accent2); font-weight: 700; }

  .btn-save {
    width: 100%; padding: 10px; margin-top: 10px;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    border: none; border-radius: 8px; color: #0f1419;
    font-weight: 700; cursor: pointer; font-size: 0.9rem;
    transition: opacity 0.2s;
  }
  .btn-save:hover { opacity: 0.9; }
  .btn-save:disabled { opacity: 0.4; cursor: not-allowed; }
</style>
</head>
<body>

<header>
  <h1>Справочник по конфигурации ПК</h1>
  <p>Подбери совместимые комплектующие и собери свою сборку</p>
</header>

<div class="container">
  <main class="catalog" id="catalog"></main>

  <aside>
    <div class="build-panel">
      <h2>Ваша сборка <span id="count" style="color:var(--muted);font-size:.85rem;font-weight:400">0 / 8</span></h2>
      <div class="build-list" id="buildList">
        <div class="empty-msg">Пока ничего не выбрано.<br>Начните с процессора 👇</div>
      </div>

      <div class="total">
        <span>Итого:</span>
        <span class="sum" id="totalSum">0 ₽</span>
      </div>

      <div class="compat" id="compat"></div>

      <button class="btn-save" id="saveBtn" disabled>💾 Сохранить сборку</button>
      <button class="btn-reset" id="resetBtn" style="margin-top:10px">Очистить сборку</button>

      <div class="saved-builds">
        <h3>Сохранённые сборки</h3>
        <div id="savedList"><div class="empty-msg" style="padding:6px 0">Нет сохранённых</div></div>
      </div>
    </div>
  </aside>
</div>

<script>
/* ---------- Данные каталога ---------- */
const CATEGORIES = [
  {
    id: 'cpu', name: 'Процессор', icon: '🧠',
    items: [{ id: 'cpu1', name: 'Intel Core i3-12100F', specs: 'LGA1700 · 4 ядра · 3.3 ГГц', price: 8500, socket: 'LGA1700', tdp: 58 },
      { id: 'cpu2', name: 'Intel Core i5-12400F', specs: 'LGA1700 · 6 ядер · 2.5 ГГц', price: 13500, socket: 'LGA1700', tdp: 65 },
      { id: 'cpu3', name: 'Intel Core i5-13600K', specs: 'LGA1700 · 14 ядер · 3.5 ГГц', price: 26000, socket: 'LGA1700', tdp: 125 },
      { id: 'cpu4', name: 'AMD Ryzen 5 5600', specs: 'AM4 · 6 ядер · 3.5 ГГц', price: 11500, socket: 'AM4', tdp: 65 },
      { id: 'cpu5', name: 'AMD Ryzen 7 5800X3D', specs: 'AM4 · 8 ядер · 3.4 ГГц', price: 25000, socket: 'AM4', tdp: 105 },
      { id: 'cpu6', name: 'AMD Ryzen 5 7600', specs: 'AM5 · 6 ядер · 3.8 ГГц', price: 21000, socket: 'AM5', tdp: 65 },
      { id: 'cpu7', name: 'AMD Ryzen 7 7800X3D', specs: 'AM5 · 8 ядер · 4.2 ГГц', price: 38000, socket: 'AM5', tdp: 120 }
    ]
  },
  {
    id: 'mb', name: 'Материнская плата', icon: '🔲',
    items: [
      { id: 'mb1', name: 'MSI PRO H610M-E', specs: 'LGA1700 · DDR4 · mATX', price: 6500, socket: 'LGA1700', ram: 'DDR4' },
      { id: 'mb2', name: 'ASUS PRIME B660M-A', specs: 'LGA1700 · DDR4 · mATX', price: 11000, socket: 'LGA1700', ram: 'DDR4' },
      { id: 'mb3', name: 'Gigabyte B760M DS3H', specs: 'LGA1700 · DDR5 · mATX', price: 14000, socket: 'LGA1700', ram: 'DDR5' },
      { id: 'mb4', name: 'MSI B450M-A PRO', specs: 'AM4 · DDR4 · mATX', price: 6000, socket: 'AM4', ram: 'DDR4' },
      { id: 'mb5', name: 'ASUS TUF B550-PLUS', specs: 'AM4 · DDR4 · ATX', price: 12000, socket: 'AM4', ram: 'DDR4' },
      { id: 'mb6', name: 'Gigabyte B650M DS3H', specs: 'AM5 · DDR5 · mATX', price: 13500, socket: 'AM5', ram: 'DDR5' },
      { id: 'mb7', name: 'ASUS ROG STRIX B650E-F', specs: 'AM5 · DDR5 · ATX', price: 24000, socket: 'AM5', ram: 'DDR5' }
    ]
  },
  {
    id: 'ram', name: 'Оперативная память', icon: '💾',
    items: [
      { id: 'ram1', name: 'Kingston Fury 8 ГБ', specs: 'DDR4 · 3200 МГц', price: 2200, ram: 'DDR4', capacity: 8 },
      { id: 'ram2', name: 'Kingston Fury 16 ГБ (2×8)', specs: 'DDR4 · 3200 МГц', price: 4200, ram: 'DDR4', capacity: 16 },
      { id: 'ram3', name: 'G.Skill Ripjaws 32 ГБ (2×16)', specs: 'DDR4 · 3600 МГц', price: 8500, ram: 'DDR4', capacity: 32 },
      { id: 'ram4', name: 'Kingston Fury 16 ГБ (2×8)', specs: 'DDR5 · 5200 МГц', price: 5500, ram: 'DDR5', capacity: 16 },
      { id: 'ram5', name: 'G.Skill Trident 32 ГБ (2×16)', specs: 'DDR5 · 6000 МГц', price: 11000, ram: 'DDR5', capacity: 32 },
      { id: 'ram6', name: 'Corsair Vengeance 64 ГБ (2×32)', specs: 'DDR5 · 5600 МГц', price: 21000, ram: 'DDR5', capacity: 64 }
    ]
  },
  {
    id: 'gpu', name: 'Видеокарта', icon: '🎮',
    items: [
      { id: 'gpu1', name: 'NVIDIA GTX 1650', specs: '4 ГБ GDDR6 · 75 Вт', price: 14000, tdp: 75 },
      { id: 'gpu2', name: 'NVIDIA RTX 3060', specs: '12 ГБ GDDR6 · 170 Вт', price: 27000, tdp: 170 },
      { id: 'gpu3', name: 'NVIDIA RTX 4060 Ti', specs: '8 ГБ GDDR6 · 160 Вт', price: 42000, tdp: 160 },
      { id: 'gpu4', name: 'NVIDIA RTX 4070', specs: '12 ГБ GDDR6X · 200 Вт', price: 60000, tdp: 200 },
      { id: 'gpu5', name: 'AMD RX 6600', specs: '8 ГБ GDDR6 · 132 Вт', price: 22000, tdp: 132 },
      { id: 'gpu6', name: 'AMD RX 7800 XT', specs: '16 ГБ GDDR6 · 263 Вт', price: 55000, tdp: 263 },
      { id: 'gpu7', name: 'Без видеокарты (встройка)', specs: 'Интегрированная графика', price: 0, tdp: 0 }
    ]
  },
  {
    id: 'storage', name: 'Накопитель', icon: '💿',
    items: [
      { id: 'st1', name: 'Kingston A400 480 ГБ', specs: 'SATA SSD', price: 3200 },
      { id: 'st2', name: 'Samsung 980 1 ТБ', specs: 'NVMe M.2', price: 6500 },
      { id: 'st3', name: 'WD Black SN850X 2 ТБ', specs: 'NVMe M.2 PCIe 4.0', price: 14000 },
      { id: 'st4', name: 'Seagate Barracuda 2 ТБ', specs: 'HDD 7200 об/мин', price: 5500 }
    ]
  },
  {
    id: 'psu', name: 'Блок питания', icon: '🔌',
    items: [{ id: 'psu1', name: 'AeroCool 500 Вт', specs: '80+ Bronze', price: 3500, watt: 500 },
      { id: 'psu2', name: 'Be Quiet 650 Вт', specs: '80+ Gold · модульный', price: 7500, watt: 650 },
      { id: 'psu3', name: 'Corsair RM750e 750 Вт', specs: '80+ Gold · модульный', price: 11000, watt: 750 },
      { id: 'psu4', name: 'Seasonic 850 Вт', specs: '80+ Gold · модульный', price: 15000, watt: 850 },
      { id: 'psu5', name: 'Corsair 1000 Вт', specs: '80+ Platinum', price: 22000, watt: 1000 }
    ]
  },
  {
    id: 'case', name: 'Корпус', icon: '🗄️',
    items: [
      { id: 'case1', name: 'Zalman T3', specs: 'mATX · 2 вентилятора', price: 3500 },
      { id: 'case2', name: 'DeepCool CC560', specs: 'ATX · 4 вентилятора', price: 5000 },
      { id: 'case3', name: 'NZXT H5 Flow', specs: 'ATX · 2 вентилятора', price: 9000 }
    ]
  },
  {
    id: 'cooler', name: 'Охлаждение', icon: '❄️',
    items: [
      { id: 'cool1', name: 'Боксовый кулер', specs: 'Идёт с CPU · до 65 Вт', price: 0, maxTdp: 65 },
      { id: 'cool2', name: 'DeepCool AK400', specs: 'Башенный · до 150 Вт', price: 3500, maxTdp: 150 },
      { id: 'cool3', name: 'Noctua NH-D15', specs: 'Двухбашенный · до 250 Вт', price: 12000, maxTdp: 250 },
      { id: 'cool4', name: 'DeepCool LT720 (СЖО)', specs: 'Жидкостное · до 300 Вт', price: 16000, maxTdp: 300 }
    ]
  }
];

/* ---------- Состояние ---------- */
const selection = {}; // categoryId -> item
const MAX_CATEGORIES = CATEGORIES.length;

const catalogEl = document.getElementById('catalog');
const buildListEl = document.getElementById('buildList');
const totalSumEl = document.getElementById('totalSum');
const countEl = document.getElementById('count');
const compatEl = document.getElementById('compat');
const saveBtn = document.getElementById('saveBtn');
const resetBtn = document.getElementById('resetBtn');
const savedListEl = document.getElementById('savedList');

let savedBuilds = JSON.parse(localStorage.getItem('pc_builds') || '[]');

/* ---------- Рендер каталога ---------- */
function renderCatalog() {
  catalogEl.innerHTML = '';
  CATEGORIES.forEach(cat => {
    const catEl = document.createElement('div');
    catEl.className = 'category';
    catEl.dataset.cat = cat.id;

    const header = document.createElement('div');
    header.className = 'category-header';
    header.innerHTML = `
      <h2><span class="icon">${cat.icon}</span>${cat.name}</h2>
      <span class="chevron">▼</span>
    `;
    header.addEventListener('click', () => catEl.classList.toggle('open'));

    const itemsEl = document.createElement('div');
    itemsEl.className = 'items';

    cat.items.forEach(item => {
      const el = document.createElement('div');
      el.className = 'item';
      el.dataset.id = item.id;
      el.innerHTML = `
        <div class="item-info">
          <div class="name">${item.name}</div>
          <div class="specs">${item.specs}</div>
        </div>
        <div class="item-right">
          <span class="price">${item.price === 0 ? '—' : item.price.toLocaleString('ru-RU') + ' ₽'}</span>
          <button class="btn btn-add">Выбрать</button>
        </div>
      `;
      el.querySelector('.btn-add').addEventListener('click', () => toggleItem(cat.id, item));
      itemsEl.appendChild(el);
    });

    catEl.appendChild(header);
    catEl.appendChild(itemsEl);
    catalogEl.appendChild(catEl);
  });
}

/* ---------- Выбор / снятие ---------- */
function toggleItem(catId, item) {
  if (selection[catId] && selection[catId].id === item.id) {
    delete selection[catId];
  } else {
    selection[catId] = item;
  }
  updateUI();
}

/* ---------- Обновление интерфейса ---------- */
function updateUI() {
  // подсветка выбранных элементов
  document.querySelectorAll('.item').forEach(el => {
    const catId = el.closest('.category').dataset.cat;
    const sel = selection[catId];
    el.classList.toggle('selected', !!(sel && sel.id === el.dataset.id));
    const btn = el.querySelector('.btn-add');if (btn) btn.textContent = (sel && sel.id === el.dataset.id) ? '✓ Выбрано' : 'Выбрать';
  });

  // панель сборки
  const chosen = Object.entries(selection);
  countEl.textContent = `${chosen.length} / ${MAX_CATEGORIES}`;

  if (chosen.length === 0) {
    buildListEl.innerHTML = '<div class="empty-msg">Пока ничего не выбрано.<br>Начните с процессора 👇</div>';
  } else {
    buildListEl.innerHTML = '';
    // порядок — как в каталоге
    CATEGORIES.forEach(cat => {
      const item = selection[cat.id];
      if (!item) return;
      const row = document.createElement('div');
      row.className = 'build-row';
      row.innerHTML = `
        <div>
          <div class="br-cat">${cat.icon} ${cat.name}</div>
          <div class="br-name">${item.name}</div>
        </div>
        <div style="display:flex;align-items:center;gap:8px">
          <span class="br-price">${item.price === 0 ? '—' : item.price.toLocaleString('ru-RU') + ' ₽'}</span>
          <button class="br-remove" title="Удалить">✕</button>
        </div>
      `;
      row.querySelector('.br-remove').addEventListener('click', () => {
        delete selection[cat.id];
        updateUI();
      });
      buildListEl.appendChild(row);
    });
  }

  const total = chosen.reduce((s, [, it]) => s + it.price, 0);
  totalSumEl.textContent = total.toLocaleString('ru-RU') + ' ₽';
  saveBtn.disabled = chosen.length === 0;

  checkCompatibility();
}

/* ---------- Проверка совместимости ---------- */
function checkCompatibility() {
  const msgs = [];
  let level = 'ok';

  const cpu = selection.cpu, mb = selection.mb, ram = selection.ram,
        psu = selection.psu, cooler = selection.cooler, gpu = selection.gpu;

  if (cpu && mb && cpu.socket !== mb.socket) {
    msgs.push(`❌ Сокет CPU (${cpu.socket}) не совпадает с платой (${mb.socket})`);
    level = 'bad';
  }
  if (mb && ram && mb.ram !== ram.ram) {
    msgs.push(`❌ Память ${ram.ram} не подходит к плате (${mb.ram})`);
    level = 'bad';
  }
  if (cpu && cooler && cooler.maxTdp && cpu.tdp > cooler.maxTdp) {
    msgs.push(`⚠️ Охлаждение слабовато: CPU ${cpu.tdp} Вт > кулер ${cooler.maxTdp} Вт`);
    if (level !== 'bad') level = 'warn';
  }
  if (psu) {
    const needWatt = (cpu ? cpu.tdp : 0) + (gpu ? gpu.tdp : 0) + 100;
    const recWatt = Math.ceil(needWatt * 1.3 / 50) * 50;
    if (psu.watt < needWatt) {
      msgs.push(`❌ БП ${psu.watt} Вт недостаточно (нужно ~${recWatt} Вт)`);
      level = 'bad';
    } else if (psu.watt < recWatt) {
      msgs.push(`⚠️ БП ${psu.watt} Вт впритык, рекомендуем ~${recWatt} Вт`);
      if (level !== 'bad') level = 'warn';
    }
  }

  if (msgs.length === 0) {
    if (Object.keys(selection).length >= 5) {
      compatEl.className = 'compat show ok';
      compatEl.innerHTML = '✅ Все компоненты совместимы';
    } else {
      compatEl.className = 'compat';
      compatEl.innerHTML = '';
    }
  } else {
    compatEl.className = 'compat show ' + level;
    compatEl.innerHTML = msgs.join('<br>');
  }
}

/* ---------- Сохранение сборок ---------- */
function saveBuild() {
  const entry = {
    id: Date.now(),
    date: new Date().toLocaleDateString('ru-RU'),
    items: JSON.parse(JSON.stringify(selection)),
    total: Object.values(selection).reduce((s, it) => s + it.price, 0)
  };
  savedBuilds.unshift(entry);
  if (savedBuilds.length > 5) savedBuilds.pop();
  localStorage.setItem('pc_builds', JSON.stringify(savedBuilds));
  renderSaved();
}

function renderSaved() {
  if (savedBuilds.length === 0) {
    savedListEl.innerHTML = '<div class="empty-msg" style="padding:6px 0">Нет сохранённых</div>';
    return;
  }
  savedListEl.innerHTML = '';
  savedBuilds.forEach(b => {
    const el = document.createElement('div');
    el.className = 'saved-item';
    const names = Object.values(b.items).map(i => i.name).slice(0, 2).join(', ');
    el.innerHTML = `
      <div>
        <div>${names}${Object.keys(b.items).length > 2 ? '…' : ''}</div><div style="color:var(--muted);font-size:.72rem">${b.date} · ${Object.keys(b.items).length} компл.</div>
      </div>
      <span class="s-price">${b.total.toLocaleString('ru-RU')} ₽</span>
    `;
    el.title = 'Клик — загрузить сборку';
    el.addEventListener('click', () => {
      Object.keys(selection).forEach(k => delete selection[k]);
      Object.assign(selection, JSON.parse(JSON.stringify(b.items)));
      updateUI();
      window.scrollTo({ top: 0, behavior: 'smooth' });
    });
    savedListEl.appendChild(el);
  });
}

/* ---------- События ---------- */
saveBtn.addEventListener('click', saveBuild);
resetBtn.addEventListener('click', () => {
  Object.keys(selection).forEach(k => delete selection[k]);
  updateUI();
});

/* ---------- Инициализация ---------- */
renderCatalog();
updateUI();
renderSaved();
// открыть первую категорию
document.querySelector('.category').classList.add('open');
</script>
</body>
</html>
