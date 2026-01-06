<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>بازی به پیش برو | نسخه پیشرفته</title>
  <style>
    body { 
      margin:0; 
      background: linear-gradient(135deg, #111 0%, #222 100%); 
      display:flex; 
      justify-content:center; 
      align-items:center; 
      height:100vh; 
      touch-action:none; 
      font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
      overflow:hidden;
    }
    canvas { 
      background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%); 
      border:3px solid #00b4d8; 
      border-radius:10px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.7);
      z-index:1; 
      position:absolute; 
    }
    #menu { 
      position:absolute; 
      display:flex; 
      flex-direction:column; 
      align-items:center; 
      top:0; 
      left:0; 
      width:100%; 
      height:100%; 
      background: linear-gradient(rgba(10, 10, 30, 0.95), rgba(5, 5, 20, 0.98));
      color:#fff; 
      z-index:2; 
      overflow-y:auto; 
      padding:20px 0; 
      box-sizing:border-box; 
      transition: opacity 0.5s;
    }
    .menu-container { 
      width:100%; 
      max-width:500px; 
      display:flex; 
      flex-direction:column; 
      align-items:center; 
      padding:0 15px; 
      box-sizing:border-box; 
    }
    h1 {
      color: #00b4d8;
      text-shadow: 0 0 10px rgba(0, 180, 216, 0.5);
      font-size: 2.5rem;
      margin-bottom: 10px;
    }
    .btn { 
      padding:18px 35px; 
      margin:12px; 
      font-size:22px; 
      background: linear-gradient(to right, #0077b6, #0096c7);
      border:none; 
      color:#fff; 
      border-radius:15px; 
      width:240px; 
      max-width:90%; 
      cursor:pointer;
      transition: all 0.3s ease;
      box-shadow: 0 5px 15px rgba(0, 119, 182, 0.3);
      font-weight:bold;
      position:relative;
      overflow:hidden;
    }
    .btn:hover {
      transform: translateY(-3px);
      box-shadow: 0 8px 20px rgba(0, 119, 182, 0.5);
      background: linear-gradient(to right, #0096c7, #00b4d8);
    }
    .btn:active {
      transform: translateY(1px);
    }
    .btn::after {
      content: '';
      position: absolute;
      top: 0;
      left: -100%;
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
      transition: left 0.7s;
    }
    .btn:hover::after {
      left: 100%;
    }
    .btn.easy { background: linear-gradient(to right, #2a9d8f, #38b000); }
    .btn.easy:hover { background: linear-gradient(to right, #38b000, #70e000); }
    .btn.medium { background: linear-gradient(to right, #e9c46a, #f4a261); }
    .btn.medium:hover { background: linear-gradient(to right, #f4a261, #e76f51); }
    .btn.hard { background: linear-gradient(to right, #e63946, #d90429); }
    .btn.hard:hover { background: linear-gradient(to right, #d90429, #9d0208); }
    
    #creator { 
      position:relative; 
      bottom:auto; 
      left:auto; 
      font-size:14px; 
      opacity:0.8; 
      margin-top:30px;
      color: #90e0ef;
      padding: 10px;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      width: 100%;
      text-align: center;
    }
    .shop-section { 
      margin-top:25px; 
      text-align:center; 
      width:100%; 
      max-width:320px; 
      background: rgba(255, 255, 255, 0.1);
      padding: 15px;
      border-radius: 10px;
      backdrop-filter: blur(5px);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }
    .shop-items-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-top: 10px;
    }
    .shop-item { 
      margin:8px auto; 
      padding:12px; 
      border:2px solid gold; 
      border-radius:8px; 
      cursor:pointer; 
      width:100%; 
      background: rgba(255, 215, 0, 0.1);
      transition: all 0.3s;
      text-align: center;
      font-size: 14px;
    }
    .shop-item:hover {
      transform: scale(1.03);
      background: rgba(255, 215, 0, 0.2);
    }
    .shop-item.locked { 
      opacity:0.5; 
      filter:grayscale(100%); 
      cursor: not-allowed;
    }
    .shop-item.locked:hover {
      transform: none;
      background: rgba(255, 215, 0, 0.1);
    }
    .shop-item.owned {
      border-color: #38b000;
      background: rgba(56, 176, 0, 0.1);
    }
    .shop-item.owned:hover {
      background: rgba(56, 176, 0, 0.2);
    }
    #starsCount { 
      font-size:28px; 
      color:gold; 
      margin-bottom:10px; 
      text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
    }
    label { 
      display:block; 
      margin-bottom:8px; 
      color: #90e0ef;
      font-weight: bold;
    }
    select { 
      margin-bottom:10px; 
      padding:8px; 
      font-size:16px; 
      border-radius: 8px;
      border: 2px solid #00b4d8;
      background: rgba(0, 0, 0, 0.5);
      color: white;
      width: 100%;
      box-sizing: border-box;
    }
    select option {
      background: #16213e;
      color: white;
    }
    .section-title {
      color: #00b4d8;
      font-size: 1.5rem;
      margin: 25px 0 15px 0;
      text-align: center;
      border-bottom: 2px solid #00b4d8;
      padding-bottom: 8px;
      width: 100%;
    }
    .stats-container {
      display: flex;
      justify-content: space-around;
      width: 100%;
      max-width: 300px;
      margin-top: 20px;
      background: rgba(255, 255, 255, 0.05);
      padding: 15px;
      border-radius: 10px;
    }
    .stat {
      text-align: center;
    }
    .stat-value {
      font-size: 24px;
      font-weight: bold;
      color: #00b4d8;
    }
    .stat-label {
      font-size: 14px;
      color: #90e0ef;
    }
    #gameOverMenu {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.85);
      z-index: 3;
      display: none;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: white;
    }
    .game-over-content {
      text-align: center;
      background: rgba(26, 26, 46, 0.9);
      padding: 30px;
      border-radius: 15px;
      border: 3px solid #00b4d8;
      max-width: 400px;
      width: 90%;
    }
    .particles {
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
      z-index: 0;
      pointer-events: none;
    }
    .particle {
      position: absolute;
      background: rgba(0, 180, 216, 0.3);
      border-radius: 50%;
      animation: float 15s infinite linear;
    }
    @keyframes float {
      0% { transform: translateY(100vh) translateX(0); }
      100% { transform: translateY(-100px) translateX(calc(100vw * var(--x-end))); }
    }
    .level-indicator {
      position: absolute;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0, 0, 0, 0.7);
      padding: 8px 20px;
      border-radius: 20px;
      color: white;
      font-weight: bold;
      border: 2px solid #00b4d8;
      z-index: 10;
      display: none;
    }
    .emoji-shop {
      font-size: 20px;
      margin-right: 8px;
    }
    .power-section {
      margin-top: 20px;
      text-align: center;
      width: 100%;
      max-width: 320px;
      background: rgba(255, 255, 255, 0.1);
      padding: 15px;
      border-radius: 10px;
      backdrop-filter: blur(5px);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }
    .power-controls {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 10px;
    }
    .power-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px;
      background: rgba(0, 0, 0, 0.3);
      border-radius: 8px;
      border: 1px solid rgba(0, 180, 216, 0.3);
    }
    .power-info {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
    }
    .power-name {
      font-weight: bold;
      color: #90e0ef;
    }
    .power-status {
      font-size: 12px;
      color: #aaa;
    }
    .power-count {
      color: gold;
      font-weight: bold;
    }
    .power-toggle {
      padding: 5px 15px;
      background: linear-gradient(to right, #0077b6, #0096c7);
      border: none;
      color: white;
      border-radius: 5px;
      cursor: pointer;
      font-size: 12px;
    }
    .power-toggle.active {
      background: linear-gradient(to right, #38b000, #70e000);
    }
    .power-toggle:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }
  </style>
</head>
<body>
<div class="particles" id="particles"></div>

<div id="menu">
  <div class="menu-container">
    <h1>🚀 بازی به پیش برو</h1>
    <p style="color: #90e0ef; text-align: center; margin-bottom: 25px;">از دشمنان دوری کن، ستاره جمع کن و رکورد بزن!</p>
    
    <div class="section-title">🎮 انتخاب سطح</div>
    <button class="btn easy" onclick="startGame('آسان')">آسان 🌟</button>
    <button class="btn medium" onclick="startGame('متوسط')">متوسط ⚡</button>
    <button class="btn hard" onclick="startGame('سخت')">سخت 🔥</button>
    
    <div class="section-title">🎨 شخصی‌سازی بازیکن</div>
    <div style="width: 100%; max-width: 300px; text-align: center; background: rgba(255, 255, 255, 0.1); padding: 15px; border-radius: 10px;">
      <label>🔵 شکل بازیکن:</label>
      <select id="playerShape">
        <!-- گزینه‌ها توسط جاوااسکریپت پر می‌شوند -->
      </select>
    </div>
    
    <!-- بخش قدرت‌ها -->
    <div class="section-title">⚡ قدرت‌های بازی</div>
    <div class="power-section">
      <h3 style="color: #00b4d8; margin-top: 0;">کنترل قدرت‌ها</h3>
      <div class="power-controls" id="powerControls">
        <!-- قدرت‌ها توسط جاوااسکریپت اضافه می‌شوند -->
      </div>
    </div>
    
    <!-- بخش فروشگاه -->
    <div class="section-title">🛒 فروشگاه</div>
    
    <div id="starsCount">⭐ ستاره‌ها: 0</div>
    
    <!-- فروشگاه شکل‌ها -->
    <div class="shop-section">
      <h3 style="color: #00b4d8; margin-top: 0;">🔄 شکل‌های جدید</h3>
      <div class="shop-items-grid" id="shapeShop">
        <!-- آیتم‌های شکل توسط جاوااسکریپت پر می‌شوند -->
      </div>
    </div>
    
    <!-- فروشگاه قدرت‌ها -->
    <div class="shop-section">
      <h3 style="color: #00b4d8; margin-top: 0;">⚡ قدرت‌های ویژه</h3>
      <div class="shop-items-grid" id="powerShop">
        <!-- آیتم‌های قدرت توسط جاوااسکریپت پر می‌شوند -->
      </div>
    </div>
    
    <div class="stats-container">
      <div class="stat">
        <div class="stat-value" id="easyBest">0</div>
        <div class="stat-label">آسان</div>
      </div>
      <div class="stat">
        <div class="stat-value" id="mediumBest">0</div>
        <div class="stat-label">متوسط</div>
      </div>
      <div class="stat">
        <div class="stat-value" id="hardBest">0</div>
        <div class="stat-label">سخت</div>
      </div>
    </div>
    
    <div id="creator">🎮 طراحی و توسعه: محمدجواد هاتف</div>
  </div>
</div>

<div id="gameOverMenu">
  <div class="game-over-content">
    <h2 style="color: #e63946;">بازی تمام شد! 🎮</h2>
    <p id="finalScore" style="font-size: 28px; margin: 20px 0; color: gold;">امتیاز: 0</p>
    <p id="newRecord" style="color: #70e000; display: none;">🎉 رکورد جدید! 🎉</p>
    <button class="btn easy" onclick="restartGame()">بازی مجدد 🔄</button>
    <button class="btn medium" onclick="backToMenu()">بازگشت به منو 🏠</button>
  </div>
</div>

<div class="level-indicator" id="levelIndicator">سطح: متوسط</div>

<canvas id="game" width="360" height="640"></canvas>

<script>
const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');
const menu = document.getElementById('menu');
const gameOverMenu = document.getElementById('gameOverMenu');
const starsCountDisplay = document.getElementById('starsCount');
const playerShapeSelect = document.getElementById('playerShape');
const levelIndicator = document.getElementById('levelIndicator');
const finalScoreDisplay = document.getElementById('finalScore');
const newRecordDisplay = document.getElementById('newRecord');
const easyBestDisplay = document.getElementById('easyBest');
const mediumBestDisplay = document.getElementById('mediumBest');
const hardBestDisplay = document.getElementById('hardBest');
const shapeShop = document.getElementById('shapeShop');
const powerShop = document.getElementById('powerShop');
const powerControls = document.getElementById('powerControls');

// متغیرهای بازی
let player = { 
  x: 160, 
  y: 560, 
  w: 40, 
  h: 40, 
  speed: 5, 
  shieldActive: false, 
  shieldTime: 0
  // trail: [] // حذف شد - دنباله حذف شده است
};
let playerShape = 'rect';
let enemies = [];
let shields = [];
let bonuses = [];
let score = 0;
let bestScores = { 
  'آسان': parseInt(localStorage.getItem('bestScore-آسان')) || 0, 
  'متوسط': parseInt(localStorage.getItem('bestScore-متوسط')) || 0, 
  'سخت': parseInt(localStorage.getItem('bestScore-سخت')) || 0 
};
let gameOver = false;
let enemySpeedMultiplier = 1;
let lastEnemyTime = 0;
let levelTime = 0;
let currentLevel = 'متوسط';
let gameActive = false;
const enemyIntervalBase = 800; 
const maxEnemiesBase = 6;

// سیستم ستاره و فروشگاه
let totalStars = parseInt(localStorage.getItem('totalStars')) || 0;
let unlockedShapes = JSON.parse(localStorage.getItem('unlockedShapes')) || ['rect', 'circle', 'triangle', 'emoji'];

// سیستم قدرت‌ها
let powerUps = {
  doubleStars: {
    active: false,
    count: parseInt(localStorage.getItem('powerDoubleStars')) || 0,
    maxCount: 3,
    price: 20,
    emoji: '⚡',
    name: 'ستاره دو برابر',
    description: 'ستاره‌های جمع‌آوری شده دو برابر می‌شوند'
  },
  magnet: {
    active: false,
    count: parseInt(localStorage.getItem('powerMagnet')) || 0,
    maxCount: 2,
    price: 30,
    emoji: '🧲',
    name: 'آهنربا',
    description: 'ستاره‌ها به سمت شما جذب می‌شوند'
  },
  slowEnemies: {
    active: false,
    count: parseInt(localStorage.getItem('powerSlowEnemies')) || 0,
    maxCount: 2,
    price: 25,
    emoji: '🐌',
    name: 'کند شدن دشمنان',
    description: 'سرعت دشمنان کاهش می‌یابد'
  }
};

// داده‌های فروشگاه شکل‌ها
const shapeShopItems = [
  { id: 'heart', emoji: '❤️', name: 'قلب', price: 10 },
  { id: 'alien', emoji: '👽', name: 'بیگانه', price: 25 },
  { id: 'rocket', emoji: '🚀', name: 'موشک', price: 50 },
  { id: 'butterfly', emoji: '🦋', name: 'پروانه', price: 30 },
  { id: 'star', emoji: '⭐', name: 'ستاره', price: 40 },
  { id: 'alien2', emoji: '👾', name: 'موجود فضایی', price: 60 },
  { id: 'controller', emoji: '🎮', name: 'دسته بازی', price: 35 },
  { id: 'crown', emoji: '👑', name: 'تاج', price: 75 },
  { id: 'car', emoji: '🏎️', name: 'ماشین', price: 25 },
  { id: 'dragon', emoji: '🐉', name: 'اژدها', price: 80 },
  { id: 'unicorn', emoji: '🦄', name: 'اسب تک‌شاخ', price: 65 },
  { id: 'ninja', emoji: '🥷', name: 'نینجا', price: 45 },
  { id: 'monkey', emoji: '🐵', name: 'میمون', price: 35 }
];

// ایجاد ذرات پس‌زمینه
function createParticles() {
  const particlesContainer = document.getElementById('particles');
  particlesContainer.innerHTML = '';
  
  for (let i = 0; i < 30; i++) {
    const particle = document.createElement('div');
    particle.classList.add('particle');
    particle.style.width = Math.random() * 5 + 2 + 'px';
    particle.style.height = particle.style.width;
    particle.style.left = Math.random() * 100 + 'vw';
    particle.style.opacity = Math.random() * 0.5 + 0.2;
    particle.style.animationDuration = Math.random() * 10 + 10 + 's';
    particle.style.animationDelay = Math.random() * 5 + 's';
    particle.style.setProperty('--x-end', Math.random() * 2 - 1);
    
    particlesContainer.appendChild(particle);
  }
}

// به‌روزرسانی نمایش رکوردها
function updateBestScoresDisplay() {
  easyBestDisplay.textContent = bestScores['آسان'];
  mediumBestDisplay.textContent = bestScores['متوسط'];
  hardBestDisplay.textContent = bestScores['سخت'];
}

// به‌روزرسانی پنل کنترل قدرت‌ها
function updatePowerControls() {
  powerControls.innerHTML = '';
  
  Object.keys(powerUps).forEach(powerKey => {
    const power = powerUps[powerKey];
    const powerItem = document.createElement('div');
    powerItem.className = 'power-item';
    
    powerItem.innerHTML = `
      <div class="power-info">
        <div class="power-name">${power.emoji} ${power.name}</div>
        <div class="power-status">${power.description}</div>
        <div class="power-count">تعداد باقیمانده: ${power.count}/${power.maxCount}</div>
      </div>
      <button class="power-toggle ${power.active ? 'active' : ''}" 
              onclick="togglePower('${powerKey}')"
              ${power.count === 0 ? 'disabled' : ''}>
        ${power.active ? 'غیرفعال' : 'فعال'}
      </button>
    `;
    
    powerControls.appendChild(powerItem);
  });
}

// فعال/غیرفعال کردن قدرت
function togglePower(powerKey) {
  const power = powerUps[powerKey];
  
  if (power.count === 0) return;
  
  if (!power.active) {
    // فعال کردن قدرت
    power.active = true;
    power.count--;
    
    // ذخیره تغییرات
    localStorage.setItem(`power${powerKey.charAt(0).toUpperCase() + powerKey.slice(1)}`, power.count);
    
    // نمایش پیام
    showSuccessMessage(`قدرت "${power.name}" فعال شد!`);
    
    // غیرفعال کردن قدرت بعد از مدت زمان معین
    setTimeout(() => {
      if (power.active) {
        power.active = false;
        updatePowerControls();
        showSuccessMessage(`قدرت "${power.name}" به پایان رسید!`);
      }
    }, 10000); // 10 ثانیه
    
  } else {
    // غیرفعال کردن قدرت
    power.active = false;
    power.count++;
    
    // ذخیره تغییرات
    localStorage.setItem(`power${powerKey.charAt(0).toUpperCase() + powerKey.slice(1)}`, power.count);
    
    showSuccessMessage(`قدرت "${power.name}" غیرفعال شد!`);
  }
  
  updatePowerControls();
  updateShop();
}

// به‌روزرسانی منوی فروشگاه
function updateShop() {
  starsCountDisplay.textContent = '⭐ ستاره‌ها: ' + totalStars;
  
  // به‌روزرسانی گزینه‌های کشویی شکل‌ها
  playerShapeSelect.innerHTML = '';
  
  // افزودن شکل‌های ابتدایی
  const basicShapes = [
    {value: 'rect', text: 'مربع'},
    {value: 'circle', text: 'دایره'},
    {value: 'triangle', text: 'مثلث'},
    {value: 'emoji', text: 'ایموجی 😎'}
  ];
  
  basicShapes.forEach(shape => {
    const option = document.createElement('option');
    option.value = shape.value;
    option.textContent = shape.text;
    if (playerShape === shape.value) option.selected = true;
    playerShapeSelect.appendChild(option);
  });
  
  // افزودن شکل‌های خریداری شده
  unlockedShapes.forEach(shapeId => {
    const shopItem = shapeShopItems.find(item => item.id === shapeId);
    if (shopItem) {
      const option = document.createElement('option');
      option.value = shopItem.id;
      option.textContent = `${shopItem.name} ${shopItem.emoji}`;
      if (playerShape === shopItem.id) option.selected = true;
      playerShapeSelect.appendChild(option);
    }
  });
  
  // به‌روزرسانی فروشگاه شکل‌ها
  shapeShop.innerHTML = '';
  shapeShopItems.forEach(item => {
    const shopItem = document.createElement('div');
    shopItem.className = 'shop-item';
    
    if (unlockedShapes.includes(item.id)) {
      shopItem.classList.add('owned');
      shopItem.innerHTML = `<span class="emoji-shop">${item.emoji}</span> ${item.name} - خریداری شده`;
      shopItem.onclick = () => {
        playerShape = item.id;
        playerShapeSelect.value = item.id;
      };
    } else {
      if (totalStars >= item.price) {
        shopItem.classList.remove('locked');
      } else {
        shopItem.classList.add('locked');
      }
      shopItem.innerHTML = `<span class="emoji-shop">${item.emoji}</span> ${item.name} - ${item.price} ⭐`;
      shopItem.onclick = () => buyShape(item.id, item.price, item.name, item.emoji);
    }
    
    shapeShop.appendChild(shopItem);
  });
  
  // به‌روزرسانی فروشگاه قدرت‌ها
  powerShop.innerHTML = '';
  
  Object.keys(powerUps).forEach(powerKey => {
    const power = powerUps[powerKey];
    const shopItem = document.createElement('div');
    shopItem.className = 'shop-item';
    
    if (power.count >= power.maxCount) {
      shopItem.classList.add('owned');
      shopItem.innerHTML = `<span class="emoji-shop">${power.emoji}</span> ${power.name} - حداکثر تعداد`;
      shopItem.onclick = () => {
        alert(`شما حداکثر تعداد ${power.maxCount} از این قدرت را دارید!`);
      };
    } else {
      if (totalStars >= power.price) {
        shopItem.classList.remove('locked');
      } else {
        shopItem.classList.add('locked');
      }
      shopItem.innerHTML = `<span class="emoji-shop">${power.emoji}</span> ${power.name} - ${power.price} ⭐`;
      shopItem.onclick = () => buyPower(powerKey, power.price, power.name, power.emoji);
    }
    
    powerShop.appendChild(shopItem);
  });
}

// خرید شکل جدید
function buyShape(shapeId, price, name, emoji) {
  if (unlockedShapes.includes(shapeId)) {
    alert('این شکل قبلاً خریداری شده است!');
    return;
  }
  
  if (totalStars >= price) {
    totalStars -= price;
    unlockedShapes.push(shapeId);
    localStorage.setItem('totalStars', totalStars);
    localStorage.setItem('unlockedShapes', JSON.stringify(unlockedShapes));
    updateShop();
    
    showSuccessMessage(`شکل ${name} ${emoji} با موفقیت خریداری شد!`);
  } else {
    alert('ستاره کافی ندارید!');
  }
}

// خرید قدرت جدید
function buyPower(powerKey, price, name, emoji) {
  const power = powerUps[powerKey];
  
  if (power.count >= power.maxCount) {
    alert(`شما حداکثر تعداد ${power.maxCount} از این قدرت را دارید!`);
    return;
  }
  
  if (totalStars >= price) {
    totalStars -= price;
    power.count++;
    localStorage.setItem('totalStars', totalStars);
    localStorage.setItem(`power${powerKey.charAt(0).toUpperCase() + powerKey.slice(1)}`, power.count);
    updateShop();
    updatePowerControls();
    
    showSuccessMessage(`قدرت ${name} ${emoji} با موفقیت خریداری شد!`);
  } else {
    alert('ستاره کافی ندارید!');
  }
}

// نمایش پیام موفقیت
function showSuccessMessage(message) {
  const successMsg = document.createElement('div');
  successMsg.textContent = message;
  successMsg.style.cssText = `
    position: fixed;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(56, 176, 0, 0.9);
    color: white;
    padding: 15px 25px;
    border-radius: 10px;
    z-index: 1000;
    font-weight: bold;
    box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    animation: fadeInOut 3s;
  `;
  document.body.appendChild(successMsg);
  
  setTimeout(() => {
    document.body.removeChild(successMsg);
  }, 3000);
}

// شروع بازی
function startGame(level) {
  currentLevel = level;
  playerShape = playerShapeSelect.value;
  menu.style.display = 'none';
  gameOverMenu.style.display = 'none';
  canvas.style.visibility = 'visible';
  levelIndicator.style.display = 'block';
  levelIndicator.textContent = `سطح: ${level}`;
  
  // تنظیم سختی
  switch(level) {
    case 'آسان': 
      enemySpeedMultiplier = 1; 
      levelIndicator.style.borderColor = '#38b000';
      break;
    case 'متوسط': 
      enemySpeedMultiplier = 1.3; 
      levelIndicator.style.borderColor = '#f4a261';
      break;
    case 'سخت': 
      enemySpeedMultiplier = 1.6; 
      levelIndicator.style.borderColor = '#e63946';
      break;
  }

  // ریست بازی
  enemies = [];
  shields = [];
  bonuses = [];
  score = 0;
  gameOver = false;
  gameActive = true;
  player.x = 160;
  player.y = 560;
  // player.trail = []; // حذف شد - دنباله حذف شده است
  player.shieldActive = false;
  player.shieldTime = 0;
  lastEnemyTime = Date.now();
  levelTime = Date.now();
}

// ریستارت بازی
function restartGame() {
  startGame(currentLevel);
}

// بازگشت به منو
function backToMenu() {
  gameOverMenu.style.display = 'none';
  menu.style.display = 'flex';
  updateBestScoresDisplay();
  updateShop();
  updatePowerControls();
}

// تولید دشمن
function spawnEnemy() {
  let now = Date.now();
  let elapsed = (now - levelTime)/1000;
  let enemyInterval = Math.max(300, enemyIntervalBase - elapsed*20);
  let maxEnemies = Math.min(12, maxEnemiesBase + Math.floor(elapsed/5));
  
  if(enemies.length >= maxEnemies) return;
  if(now - lastEnemyTime < enemyInterval) return;
  
  lastEnemyTime = now;
  
  // اعمال اثر کند شدن دشمنان اگر قدرت فعال باشد
  let speedMultiplier = enemySpeedMultiplier;
  if (powerUps.slowEnemies.active) {
    speedMultiplier *= 0.6; // کاهش 40 درصدی سرعت
  }
  
  enemies.push({ 
    x: Math.random() * (canvas.width - 20), 
    y: -30, 
    w: 12, 
    h: 60, 
    speed: (2 + Math.random() * 2) * speedMultiplier 
  });
}

// تولید محافظ
function spawnShield() { 
  if(Math.random() < 0.003) shields.push({ 
    x: Math.random() * (canvas.width - 30), 
    y: -30, 
    w:30, 
    h:30, 
    speed:2 
  }); 
}

// تولید ستاره
function spawnBonus() { 
  if(Math.random() < 0.02) bonuses.push({ 
    x: Math.random() * (canvas.width - 20), 
    y: -20, 
    w:20, 
    h:20, 
    speed:3 
  }); 
}

// حرکت ستاره‌ها به سمت بازیکن (آهنربا)
function moveBonusesWithMagnet() {
  if (!powerUps.magnet.active) return;
  
  bonuses.forEach(b => {
    const dx = player.x + player.w/2 - (b.x + b.w/2);
    const dy = player.y + player.h/2 - (b.y + b.h/2);
    const distance = Math.sqrt(dx*dx + dy*dy);
    
    if (distance < 200) { // شعاع جذب
      b.x += dx * 0.05;
      b.y += dy * 0.05;
    }
  });
}

// رسم بازیکن
function drawPlayer() {
  // رنگ بازیکن همیشه سفید
  ctx.fillStyle = '#ffffff';
  
  // رسم بازیکن اصلی
  if(playerShape === 'rect'){
    ctx.fillRect(player.x, player.y, player.w, player.h);
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.strokeRect(player.x, player.y, player.w, player.h);
  } else if(playerShape === 'circle'){
    ctx.beginPath();
    ctx.arc(player.x + player.w/2, player.y + player.h/2, player.w/2, 0, Math.PI*2);
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
  } else if(playerShape === 'triangle'){
    ctx.beginPath();
    ctx.moveTo(player.x + player.w/2, player.y);
    ctx.lineTo(player.x, player.y + player.h);
    ctx.lineTo(player.x + player.w, player.y + player.h);
    ctx.closePath();
    ctx.fill();
    ctx.strokeStyle = '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
  } else if(playerShape === 'emoji'){
    ctx.font = '32px sans-serif';
    ctx.fillText('😎', player.x, player.y + player.h);
  } else if(playerShape === 'heart'){
    ctx.font = '32px sans-serif';
    ctx.fillText('❤️', player.x, player.y + player.h);
  } else if(playerShape === 'alien'){
    ctx.font = '32px sans-serif';
    ctx.fillText('👽', player.x, player.y + player.h);
  } else if(playerShape === 'rocket'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🚀', player.x, player.y + player.h);
  } else if(playerShape === 'butterfly'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🦋', player.x, player.y + player.h);
  } else if(playerShape === 'star'){
    ctx.font = '32px sans-serif';
    ctx.fillText('⭐', player.x, player.y + player.h);
  } else if(playerShape === 'alien2'){
    ctx.font = '32px sans-serif';
    ctx.fillText('👾', player.x, player.y + player.h);
  } else if(playerShape === 'controller'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🎮', player.x, player.y + player.h);
  } else if(playerShape === 'crown'){
    ctx.font = '32px sans-serif';
    ctx.fillText('👑', player.x, player.y + player.h);
  } else if(playerShape === 'car'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🏎️', player.x, player.y + player.h);
  } else if(playerShape === 'dragon'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🐉', player.x, player.y + player.h);
  } else if(playerShape === 'unicorn'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🦄', player.x, player.y + player.h);
  } else if(playerShape === 'ninja'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🥷', player.x, player.y + player.h);
  } else if(playerShape === 'monkey'){
    ctx.font = '32px sans-serif';
    ctx.fillText('🐵', player.x, player.y + player.h);
  }

  if(player.shieldActive){
    const pulse = Math.sin(Date.now()/150)*5 + player.w;
    ctx.strokeStyle = 'rgba(0,180,255,0.7)';
    ctx.lineWidth = 3;
    ctx.beginPath();
    ctx.arc(player.x + player.w/2, player.y + player.h/2, pulse, 0, Math.PI*2);
    ctx.stroke();
    ctx.font = '18px sans-serif';
    ctx.fillText('🛡️', player.x + player.w/4, player.y - 8);
  }
  
  // نمایش وضعیت قدرت‌های فعال
  let powerY = 180;
  if (powerUps.doubleStars.active) {
    ctx.fillStyle = '#FFD700';
    ctx.fillText('⚡ ×۲', 10, powerY);
    powerY += 30;
  }
  if (powerUps.magnet.active) {
    ctx.fillStyle = '#00FF00';
    ctx.fillText('🧲 آهنربا', 10, powerY);
    powerY += 30;
  }
  if (powerUps.slowEnemies.active) {
    ctx.fillStyle = '#FF6B6B';
    ctx.fillText('🐌 دشمنان کند', 10, powerY);
  }
}

// رسم دشمنان
function drawEnemies(){ 
  ctx.strokeStyle='red'; 
  ctx.lineWidth=5; 
  enemies.forEach(e=>{ 
    ctx.beginPath(); 
    ctx.moveTo(e.x, e.y); 
    ctx.lineTo(e.x, e.y + e.h); 
    ctx.stroke(); 
    
    // افزودن درخشش به دشمنان
    ctx.shadowColor = 'red';
    ctx.shadowBlur = 10;
    ctx.stroke();
    ctx.shadowBlur = 0;
  }); 
}

// رسم محافظ‌ها
function drawShields(){ 
  ctx.fillStyle='orange'; 
  ctx.font='20px sans-serif'; 
  shields.forEach(s=>{ 
    ctx.fillText('🛡️', s.x, s.y + s.h); 
    
    // افزودن درخشش
    ctx.shadowColor = 'orange';
    ctx.shadowBlur = 10;
    ctx.fillText('🛡️', s.x, s.y + s.h);
    ctx.shadowBlur = 0;
  }); 
}

// رسم ستاره‌ها
function drawBonuses(){ 
  ctx.fillStyle='gold'; 
  ctx.font='20px sans-serif'; 
  bonuses.forEach(b=>{ 
    ctx.fillText('⭐', b.x, b.y + b.h); 
    
    // افزودن درخشش و انیمیشن
    const scale = 1 + Math.sin(Date.now()/200) * 0.2;
    ctx.save();
    ctx.translate(b.x + b.w/2, b.y + b.h/2);
    ctx.scale(scale, scale);
    ctx.fillText('⭐', -b.w/2, b.h/2);
    ctx.restore();
    
    ctx.shadowColor = 'gold';
    ctx.shadowBlur = 15;
    ctx.fillText('⭐', b.x, b.y + b.h);
    ctx.shadowBlur = 0;
  }); 
}

// حرکت دادن اشیاء
function moveEnemies(){ enemies.forEach(e=>e.y+=e.speed); }
function moveShields(){ shields.forEach(s=>s.y+=s.speed); }
function moveBonuses(){ 
  bonuses.forEach(b=>b.y+=b.speed); 
  
  // اعمال اثر آهنربا
  moveBonusesWithMagnet();
}

// تشخیص برخورد
function checkCollision(a,b){ 
  return a.x < b.x + b.w && 
         a.x + a.w > b.x && 
         a.y < b.y + b.h && 
         a.y + a.h > b.y; 
}

// نمایش منوی پایان بازی
function showGameOverMenu() {
  gameActive = false;
  gameOverMenu.style.display = 'flex';
  finalScoreDisplay.textContent = `امتیاز: ${score}`;
  finalScoreDisplay.style.color = 'gold';
  
  // غیرفعال کردن تمام قدرت‌ها
  Object.keys(powerUps).forEach(powerKey => {
    powerUps[powerKey].active = false;
  });
  
  // بررسی رکورد جدید
  if(score > bestScores[currentLevel]){
    bestScores[currentLevel] = score;
    localStorage.setItem('bestScore-' + currentLevel, score);
    newRecordDisplay.style.display = 'block';
    newRecordDisplay.style.animation = 'pulse 1s infinite';
    
    // اضافه کردن ستاره پاداش برای رکورد جدید
    const bonusStars = Math.floor(score / 100);
    totalStars += bonusStars;
    localStorage.setItem('totalStars', totalStars);
    
    // نمایش پاداش
    if (bonusStars > 0) {
      const bonusMsg = document.createElement('p');
      bonusMsg.textContent = `🎉 پاداش رکورد جدید: ${bonusStars} ستاره!`;
      bonusMsg.style.color = '#70e000';
      bonusMsg.style.margin = '10px 0';
      bonusMsg.style.fontWeight = 'bold';
      gameOverMenu.querySelector('.game-over-content').appendChild(bonusMsg);
    }
  } else {
    newRecordDisplay.style.display = 'none';
  }
  
  updateShop();
  updatePowerControls();
  updateBestScoresDisplay();
}

// به‌روزرسانی بازی
function update(){
  if(!gameActive) return;
  
  // محاسبه زمان سپری شده از شروع سطح
  let elapsed = (Date.now() - levelTime)/1000;
  
  // به‌روزرسانی محافظ
  if(player.shieldActive && Date.now() - player.shieldTime > 5000){ 
    player.shieldActive = false; 
    player.shieldTime = 0; 
  }

  ctx.clearRect(0, 0, canvas.width, canvas.height);
  
  // رسم پس‌زمینه پویا
  const gradient = ctx.createLinearGradient(0, 0, 0, canvas.height);
  gradient.addColorStop(0, '#1a1a2e');
  gradient.addColorStop(1, '#16213e');
  ctx.fillStyle = gradient;
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  
  // رسم خطوط راهنما
  ctx.strokeStyle = 'rgba(255, 255, 255, 0.1)';
  ctx.lineWidth = 1;
  for(let i = 0; i < canvas.width; i += 40) {
    ctx.beginPath();
    ctx.moveTo(i, 0);
    ctx.lineTo(i, canvas.height);
    ctx.stroke();
  }
  
  moveEnemies(); 
  moveShields(); 
  moveBonuses();
  
  drawPlayer(); 
  drawEnemies(); 
  drawShields(); 
  drawBonuses();
  
  spawnEnemy(); 
  spawnShield(); 
  spawnBonus();

  // برخورد با دشمنان
  enemies = enemies.filter(e=>{
    if(checkCollision(player, e)){
      if(player.shieldActive){ 
        player.shieldActive = false; 
        player.shieldTime = 0; 
        return false; 
      } else { 
        gameOver = true; 
        // تغییر اینجا: حذف تاخیر 500 میلی‌ثانیه
        showGameOverMenu(); // بدون تاخیر
      }
    }
    return e.y <= canvas.height;
  });

  // برخورد با محافظ‌ها
  shields = shields.filter(s=>{ 
    if(checkCollision(player, s)){ 
      player.shieldActive = true; 
      player.shieldTime = Date.now(); 
      return false;
    } 
    return s.y <= canvas.height; 
  });
  
  // برخورد با ستاره‌ها
  bonuses = bonuses.filter(b=>{ 
    if(checkCollision(player, b)){ 
      let starsEarned = 1;
      
      // اعمال اثر دوبرابر شدن ستاره
      if (powerUps.doubleStars.active) {
        starsEarned *= 2;
      }
      
      totalStars += starsEarned;
      localStorage.setItem('totalStars', totalStars);
      updateShop();
      return false;
    } 
    return b.y <= canvas.height; 
  });

  score++;
  
  // نمایش اطلاعات بازی
  ctx.fillStyle='#fff'; 
  ctx.font='20px sans-serif';
  ctx.fillText('🏆 امتیاز: ' + score, 10, 30);
  ctx.fillText('🎯 رکورد: ' + bestScores[currentLevel], 10, 60);
  ctx.fillText('⭐ ستاره‌ها: ' + totalStars, 10, 90);
  
  // نمایش زمان سپری شده
  ctx.fillText('⏱️ زمان: ' + Math.floor(elapsed) + 's', 10, 120);
  
  // نمایش وضعیت محافظ
  if(player.shieldActive) {
    const remaining = Math.max(0, 5 - (Date.now() - player.shieldTime)/1000);
    ctx.fillText('🛡️ محافظ: ' + remaining.toFixed(1) + 's', 10, 150);
  }
}

// کنترل لمسی
canvas.addEventListener('touchmove', e=>{ 
  if(!gameActive) return; 
  e.preventDefault();
  const touch = e.touches[0]; 
  const rect = canvas.getBoundingClientRect(); 
  player.x = Math.max(0, Math.min(canvas.width - player.w, touch.clientX - rect.left - player.w/2)); 
});

canvas.addEventListener('touchstart', e=>{ 
  if(!gameActive) return; 
  e.preventDefault();
  const touch = e.touches[0]; 
  const rect = canvas.getBoundingClientRect(); 
  player.x = Math.max(0, Math.min(canvas.width - player.w, touch.clientX - rect.left - player.w/2)); 
});

// کنترل کیبورد برای دسکتاپ
document.addEventListener('keydown', e=>{
  if(!gameActive) return;
  
  if(e.key === 'ArrowLeft' || e.key === 'a') {
    player.x = Math.max(0, player.x - player.speed);
  } else if(e.key === 'ArrowRight' || e.key === 'd') {
    player.x = Math.min(canvas.width - player.w, player.x + player.speed);
  }
});

// مقداردهی اولیه
createParticles();
updateBestScoresDisplay();
updateShop();
updatePowerControls();

playerShapeSelect.addEventListener('change', (e) => {
  playerShape = e.target.value;
});

// شروع بازه‌های بازی
setInterval(update, 16);

// استایل CSS برای انیمیشن‌ها
const style = document.createElement('style');
style.textContent = `
  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.1); }
    100% { transform: scale(1); }
  }
  
  @keyframes fadeInOut {
    0% { opacity: 0; transform: translateX(-50%) translateY(-20px); }
    15% { opacity: 1; transform: translateX(-50%) translateY(0); }
    85% { opacity: 1; transform: translateX(-50%) translateY(0); }
    100% { opacity: 0; transform: translateX(-50%) translateY(-20px); }
  }
`;
document.head.appendChild(style);
</script>
</body>
</html>
