// === 1. FIREBASE & LICENSE LOCK SYSTEM (Put this at Line 1) ===
const firebaseConfig = {
    apiKey: "AIzaSyBNr8FAOI1FV7cUnoCsCbM1NYQ9a8JAjwY",
    authDomain: "mypersonalcompcal.firebaseapp.com",
    databaseURL: "https://mypersonalcompcal-default-rtdb.firebaseio.com",
    projectId: "mypersonalcompcal",
    storageBucket: "mypersonalcompcal.firebasestorage.app",
    messagingSenderId: "490315939778",
    appId: "1:490315939778:web:d57844886d6f8ffb99605b",
    measurementId: "G-JTYGCQM7ZD"
};

if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
}
const db = firebase.database();

function getBrowserFingerprint() {
    let deviceId = localStorage.getItem('app_device_id');
    if (!deviceId) {
        deviceId = 'DEV_' + Math.random().toString(36).substring(2) + Date.now().toString(36);
        localStorage.setItem('app_device_id', deviceId);
    }
    return deviceId;
}

async function verifyLicense() {
    let userKey = localStorage.getItem('app_license_key');

    if (!userKey) {
        userKey = prompt("Enter your Client License Key to unlock the application:");
        if (!userKey) {
            denyAccess("A valid license key is required to use this software.");
            return false;
        }
        userKey = userKey.trim().toUpperCase();
    }

    try {
        const snapshot = await db.ref('licenses/' + userKey).once('value');
        const licenseData = snapshot.val();

        if (!licenseData || !licenseData.active) {
            localStorage.removeItem('app_license_key');
            denyAccess("Invalid or revoked license key.");
            return false;
        }

        const currentDevice = getBrowserFingerprint();

        if (!licenseData.deviceId) {
            await db.ref('licenses/' + userKey).update({ deviceId: currentDevice });
        } else if (licenseData.deviceId !== currentDevice) {
            denyAccess("This license key is already locked to another device.");
            return false;
        }

        localStorage.setItem('app_license_key', userKey);
        return true;

    } catch (err) {
        denyAccess("Unable to verify license key online.");
        return false;
    }
}

function denyAccess(message) {
    document.body.innerHTML = `
        <div style="background:#0f172a; color:#ffffff; font-family:sans-serif; height:100vh; display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center; padding:20px;">
            <h1 style="color:#ef4444; font-size:28px; margin-bottom:10px;">Access Denied</h1>
            <p style="color:#94a3b8; max-width:400px; margin-bottom:20px;">${message}</p>
            <button onclick="localStorage.removeItem('app_license_key'); location.reload();" style="background:#6366f1; color:white; border:none; padding:10px 20px; border-radius:8px; cursor:pointer; font-weight:bold;">
                Try Another License Key
            </button>
        </div>
    `;
}
// === END LICENSE LOCK SYSTEM ===
// // Core Application State
window.appState = {
    borrowers: [],
    loans: [],
    payments: [],
    theme: 'coffee',
    currency: '₱',
    particlesEnabled: true,
    particleSpeed: 1,
    defaultInterestRate: 5,
    defaultPenalty: 100,
    currentTourStep: 1
};

// Relative Exchange Rates Table (Base: USD 1.00)
const exchangeRates = {
    '$': 1.00,    // US Dollar
    '₱': 56.00,   // Philippine Peso
    '€': 0.92,    // Euro
    '£': 0.78,    // British Pound
    '¥': 150.00,  // Japanese Yen
    '₹': 83.00    // Indian Rupee
};

// Interactive Spotlight Tour Steps
const tourSteps = [
    {
        targetId: null, // No spotlight box on initial welcome modal
        title: 'Welcome to your Personal Loan Management System & Debtor Ledger!',
        subtitle: 'Financial Command Center',
        description: 'Your Loan Management System & Debtor Ledger automatically calculates compound interest, tracks debtor balances, and generates printable receipts.',
        highlight: 'Everything is saved safely inside your web browser.',
        arrowPos: 'bottom'
    },
    {
        targetId: 'btn-add-debtor-header',
        title: '1. Add & Manage Debtors',
        subtitle: 'Debtor Directory & Custom Notes',
        description: 'Click "Add Debtor" here to register new debtors with phone numbers, addresses, or payment notes.',
        highlight: 'View, add, or edit debtor details anytime.',
        arrowPos: 'bottom'
    },
    {
        targetId: 'btn-issue-loan-header',
        title: '2. Issue Compound Loans',
        subtitle: 'Flexible Interest & Auto Late Fees',
        description: 'Click "Issue Loan" to issue capital with compound interest rates, weekly/monthly cycles, and auto late penalties.',
        highlight: 'Interest and penalties accrue automatically up to today.',
        arrowPos: 'bottom'
    },
    {
        targetId: 'chart-section-container',
        title: '3. Growth Trajectory Chart',
        subtitle: 'Visual Financial Analytics',
        description: 'This is where you can view or see your debtor portfolio performance, debt growth projections, and active vs settled ratios.',
        highlight: 'Select custom time horizons from 1 week up to 24 months!',
        arrowPos: 'top'
    },
    {
        targetId: 'active-debtors-summary-table',
        title: '4. Debtor Summary & Receipts',
        subtitle: 'Live Balances & Repayments',
        description: 'This is where you can view your active debtor balances, log repayments, and print 1-click official receipts or audit statements.',
        highlight: 'Log repayments here to keep debtor balances up to date.',
        arrowPos: 'top'
    }
];

window.debtGrowthChartInstance = null;
window.loanStatusPieInstance = null;

// Interactive Particle Engine State
const particleEngine = {
    canvas: null,
    ctx: null,
    particles: [],
    particleCount: 75,
    mouse: { x: null, y: null, radius: 140 },
    colorRGB: [212, 154, 106]
};

window.addEventListener('DOMContentLoaded', async () => {
    const isAuthorized = await verifyLicense();
    if (!isAuthorized) return; // Stop if license is invalid

    loadFromLocalStorage();
    applyTheme(window.appState.theme);
    syncSettingsToUI();
    initHeaderGreeting();
    initParticleCanvas();
    initDefaultDates();
    updateCalcEngine();
    renderAll();
    checkFirstTimeOnboarding();
});

// Check if user is opening for the first time
function checkFirstTimeOnboarding() {
    const tourCompleted = localStorage.getItem('frequator_tour_completed');
    if (!tourCompleted) {
        setTimeout(() => {
            startTour();
        }, 600);
    }
}

function startTour() {
    window.appState.currentTourStep = 1;
    const tourContainer = document.getElementById('onboardingModal');
    if (tourContainer) tourContainer.classList.remove('hidden');
    renderTourStep();
}

function renderTourStep() {
    const stepIdx = window.appState.currentTourStep - 1;
    const stepData = tourSteps[stepIdx];
    if (!stepData) return;

    document.getElementById('tour-step-counter').textContent = `Step ${window.appState.currentTourStep} of ${tourSteps.length}`;
    document.getElementById('tour-title').textContent = stepData.title;
    document.getElementById('tour-subtitle').textContent = stepData.subtitle;
    document.getElementById('tour-description').textContent = stepData.description;
    document.getElementById('tour-highlight').textContent = stepData.highlight;

    // Render progress dots
    const dotsContainer = document.getElementById('tour-dots-container');
    if (dotsContainer) {
        dotsContainer.innerHTML = tourSteps.map((_, idx) => `
            <span class="w-2 h-2 rounded-full transition-all duration-300 ${idx === stepIdx ? 'bg-indigo-400 w-5' : 'bg-slate-700'}"></span>
        `).join('');
    }

    // Toggle Back / Next / Finish Button State
    const backBtn = document.getElementById('tour-back-btn');
    const nextBtn = document.getElementById('tour-next-btn');

    if (backBtn) {
        if (window.appState.currentTourStep === 1) {
            backBtn.classList.add('hidden');
        } else {
            backBtn.classList.remove('hidden');
        }
    }

    if (nextBtn) {
        if (window.appState.currentTourStep === tourSteps.length) {
            nextBtn.innerHTML = `<span>Finish</span> <i data-lucide="check-circle" class="w-3.5 h-3.5"></i>`;
        } else {
            nextBtn.innerHTML = `<span>Next</span> <i data-lucide="arrow-right" class="w-3.5 h-3.5"></i>`;
        }
    }

    positionSpotlightAndArrow(stepData.targetId, stepData.arrowPos);
    lucide.createIcons();
}

function positionSpotlightAndArrow(targetId, arrowPos) {
    const spotlight = document.getElementById('tour-spotlight');
    const arrow = document.getElementById('tour-arrow');

    if (!spotlight) return;

    if (!targetId) {
        spotlight.style.opacity = '0';
        if (arrow) arrow.style.opacity = '0';
        return;
    }

    const target = document.getElementById(targetId);
    if (!target) {
        spotlight.style.opacity = '0';
        if (arrow) arrow.style.opacity = '0';
        return;
    }

    target.scrollIntoView({ behavior: 'smooth', block: 'center' });

    setTimeout(() => {
        const rect = target.getBoundingClientRect();
        const padding = 10;

        spotlight.style.top = `${rect.top - padding}px`;
        spotlight.style.left = `${rect.left - padding}px`;
        spotlight.style.width = `${rect.width + padding * 2}px`;
        spotlight.style.height = `${rect.height + padding * 2}px`;
        spotlight.style.opacity = '1';

        if (arrow) {
            arrow.style.opacity = '1';
            if (arrowPos === 'top') {
                arrow.style.top = `${Math.max(10, rect.top - 44)}px`;
                arrow.style.left = `${rect.left + rect.width / 2 - 14}px`;
                arrow.innerHTML = `<i data-lucide="arrow-down" class="w-8 h-8 text-indigo-400 animate-bounce"></i>`;
            } else {
                arrow.style.top = `${rect.bottom + 10}px`;
                arrow.style.left = `${rect.left + rect.width / 2 - 14}px`;
                arrow.innerHTML = `<i data-lucide="arrow-up" class="w-8 h-8 text-indigo-400 animate-bounce"></i>`;
            }
            lucide.createIcons();
        }
    }, 250);
}

function nextTourStep() {
    if (window.appState.currentTourStep < tourSteps.length) {
        window.appState.currentTourStep++;
        renderTourStep();
    } else {
        completeTour();
    }
}

function prevTourStep() {
    if (window.appState.currentTourStep > 1) {
        window.appState.currentTourStep--;
        renderTourStep();
    }
}

function completeTour() {
    localStorage.setItem('frequator_tour_completed', 'true');
    
    const tourContainer = document.getElementById('onboardingModal');
    const spotlight = document.getElementById('tour-spotlight');
    const arrow = document.getElementById('tour-arrow');

    if (tourContainer) tourContainer.classList.add('hidden');
    if (spotlight) spotlight.style.opacity = '0';
    if (arrow) arrow.style.opacity = '0';

    showToast("Welcome to your Personal Loan Management System & Debtor Ledger! You're ready to go.");
}

// Dynamic Header Greetings & Subtitles
function initHeaderGreeting() {
    const hour = new Date().getHours();
    const hasVisited = localStorage.getItem('lendwise_visited');
    let mainGreeting = hasVisited ? "Welcome Back!" : "Welcome!";

    if (!hasVisited) localStorage.setItem('lendwise_visited', 'true');

    let timeOfDay = "Good Day";
    if (hour >= 5 && hour < 12) timeOfDay = "Good Morning";
    else if (hour >= 12 && hour < 18) timeOfDay = "Good Afternoon";
    else if (hour >= 18 && hour < 23) timeOfDay = "Good Evening";
    else timeOfDay = "Late Night Session";

    const subtitles = [
        `${timeOfDay} • Financial Control Center Active`,
        `${timeOfDay} • Portfolio Tracking Engine Online`,
        `${timeOfDay} • Compound Gains & Ledger Updated`,
        `${timeOfDay} • System Operations Ready`
    ];

    const randomSub = subtitles[Math.floor(Math.random() * subtitles.length)];
    
    const headerMain = document.getElementById('header-main-greeting');
    const headerSub = document.getElementById('header-sub-greeting');
    if (headerMain) headerMain.textContent = mainGreeting;
    if (headerSub) headerSub.innerHTML = `<span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span><span>${randomSub}</span>`;
}

// Particle Canvas Engine
function initParticleCanvas() {
    particleEngine.canvas = document.getElementById('particle-canvas');
    if (!particleEngine.canvas) return;
    particleEngine.ctx = particleEngine.canvas.getContext('2d');

    resizeCanvas();
    window.addEventListener('resize', resizeCanvas);

    window.addEventListener('mousemove', (e) => {
        particleEngine.mouse.x = e.clientX;
        particleEngine.mouse.y = e.clientY;
    });

    window.addEventListener('touchmove', (e) => {
        if (e.touches.length > 0) {
            particleEngine.mouse.x = e.touches[0].clientX;
            particleEngine.mouse.y = e.touches[0].clientY;
        }
    });

    particleEngine.particles = [];
    for (let i = 0; i < particleEngine.particleCount; i++) {
        particleEngine.particles.push(createParticle());
    }

    updateParticleThemeColor();
    animateParticleLoop();
}

function resizeCanvas() {
    if (!particleEngine.canvas) return;
    particleEngine.canvas.width = window.innerWidth;
    particleEngine.canvas.height = window.innerHeight;
}

function createParticle() {
    return {
        x: Math.random() * (particleEngine.canvas ? particleEngine.canvas.width : 1000),
        y: Math.random() * (particleEngine.canvas ? particleEngine.canvas.height : 1000),
        radius: Math.random() * 2.4 + 0.6,
        baseAlpha: Math.random() * 0.5 + 0.2,
        vx: (Math.random() - 0.5) * 0.4,
        vy: -(Math.random() * 0.6 + 0.2),
        pulse: Math.random() * Math.PI
    };
}

function updateParticleThemeColor() {
    const style = getComputedStyle(document.documentElement);
    const rawRGB = style.getPropertyValue('--particle-rgb').trim();
    if (rawRGB) {
        const parts = rawRGB.split(',').map(n => parseInt(n.trim()));
        if (parts.length === 3) particleEngine.colorRGB = parts;
    }
}

function animateParticleLoop() {
    const ctx = particleEngine.ctx;
    const canvas = particleEngine.canvas;
    if (!ctx || !canvas) return;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    if (!window.appState.particlesEnabled) {
        requestAnimationFrame(animateParticleLoop);
        return;
    }

    const [r, g, b] = particleEngine.colorRGB;
    const speedMult = window.appState.particleSpeed || 1;

    particleEngine.particles.forEach(p => {
        p.y += p.vy * speedMult;
        p.x += p.vx * speedMult;
        p.pulse += 0.03;

        if (p.y < -10) {
            p.y = canvas.height + 10;
            p.x = Math.random() * canvas.width;
        }
        if (p.x < -10) p.x = canvas.width + 10;
        if (p.x > canvas.width + 10) p.x = -10;

        let dx = particleEngine.mouse.x - p.x;
        let dy = particleEngine.mouse.y - p.y;
        let dist = Math.sqrt(dx * dx + dy * dy);

        let alpha = p.baseAlpha + Math.sin(p.pulse) * 0.15;
        if (dist < particleEngine.mouse.radius) {
            alpha = Math.min(1, alpha + (1 - dist / particleEngine.mouse.radius) * 0.5);
            p.x -= (dx / dist) * 0.6;
            p.y -= (dy / dist) * 0.6;
        }

        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${r}, ${g}, ${b}, ${Math.max(0, alpha)})`;
        ctx.shadowBlur = 8;
        ctx.shadowColor = `rgba(${r}, ${g}, ${b}, 0.8)`;
        ctx.fill();
    });

    requestAnimationFrame(animateParticleLoop);
}

// Local Storage Persistence
function loadFromLocalStorage() {
    try {
        const storedTheme = localStorage.getItem('lendwise_theme');
        const storedCurrency = localStorage.getItem('lendwise_currency');
        const storedParticles = localStorage.getItem('lendwise_particles');
        const storedSpeed = localStorage.getItem('lendwise_speed');
        const storedDefRate = localStorage.getItem('lendwise_defrate');
        const storedDefPen = localStorage.getItem('lendwise_defpen');

        const b = localStorage.getItem('lendwise_b');
        const l = localStorage.getItem('lendwise_l');
        const p = localStorage.getItem('lendwise_p');

        if (storedTheme) window.appState.theme = storedTheme;
        if (storedCurrency) window.appState.currency = storedCurrency;
        if (storedParticles !== null) window.appState.particlesEnabled = storedParticles === 'true';
        if (storedSpeed) window.appState.particleSpeed = parseFloat(storedSpeed);
        if (storedDefRate) window.appState.defaultInterestRate = parseFloat(storedDefRate);
        if (storedDefPen) window.appState.defaultPenalty = parseFloat(storedDefPen);

        if (b) window.appState.borrowers = JSON.parse(b);
        if (l) window.appState.loans = JSON.parse(l);
        if (p) window.appState.payments = JSON.parse(p);
    } catch (e) {
        console.error("Error loading stored data:", e);
    }
}

function saveToLocalStorage() {
    try {
        localStorage.setItem('lendwise_theme', window.appState.theme);
        localStorage.setItem('lendwise_currency', window.appState.currency);
        localStorage.setItem('lendwise_particles', window.appState.particlesEnabled);
        localStorage.setItem('lendwise_speed', window.appState.particleSpeed);
        localStorage.setItem('lendwise_defrate', window.appState.defaultInterestRate);
        localStorage.setItem('lendwise_defpen', window.appState.defaultPenalty);

        localStorage.setItem('lendwise_b', JSON.stringify(window.appState.borrowers));
        localStorage.setItem('lendwise_l', JSON.stringify(window.appState.loans));
        localStorage.setItem('lendwise_p', JSON.stringify(window.appState.payments));
    } catch (e) {
        console.error("Error saving data:", e);
    }
}

function syncSettingsToUI() {
    const currSelect = document.getElementById('setting-currency-select');
    const particleToggle = document.getElementById('setting-particle-toggle');
    const speedSlider = document.getElementById('setting-particle-speed');
    const defRateInput = document.getElementById('setting-default-rate');
    const defPenInput = document.getElementById('setting-default-penalty');

    if (currSelect) currSelect.value = window.appState.currency;
    if (particleToggle) particleToggle.checked = window.appState.particlesEnabled;
    if (speedSlider) speedSlider.value = window.appState.particleSpeed;
    if (defRateInput) defRateInput.value = window.appState.defaultInterestRate;
    if (defPenInput) defPenInput.value = window.appState.defaultPenalty;

    updateCurrencyLabels();
}

function updateCurrencyLabels() {
    const sym = window.appState.currency || '₱';
    document.querySelectorAll('.currency-symbol-label').forEach(el => el.textContent = sym);
}

// Automatic Currency Conversion & Settings Handler
function updateAppSettings() {
    const currSelect = document.getElementById('setting-currency-select');
    const particleToggle = document.getElementById('setting-particle-toggle');
    const speedSlider = document.getElementById('setting-particle-speed');
    const defRateInput = document.getElementById('setting-default-rate');
    const defPenInput = document.getElementById('setting-default-penalty');

    const oldCurrency = window.appState.currency || '₱';
    const newCurrency = currSelect ? currSelect.value : oldCurrency;

    if (oldCurrency !== newCurrency) {
        convertCurrencyValues(oldCurrency, newCurrency);
        window.appState.currency = newCurrency;
        showToast(`Values converted from ${oldCurrency} to ${newCurrency}`);
    }

    if (particleToggle) window.appState.particlesEnabled = particleToggle.checked;
    if (speedSlider) window.appState.particleSpeed = parseFloat(speedSlider.value) || 1;
    if (defRateInput) window.appState.defaultInterestRate = parseFloat(defRateInput.value) || 0;
    if (defPenInput) window.appState.defaultPenalty = parseFloat(defPenInput.value) || 0;

    saveToLocalStorage();
    updateCurrencyLabels();
    renderAll();
}

function convertCurrencyValues(oldCurr, newCurr) {
    const oldRate = exchangeRates[oldCurr] || 1.0;
    const newRate = exchangeRates[newCurr] || 1.0;
    const multiplier = newRate / oldRate;

    (window.appState.loans || []).forEach(loan => {
        if (loan.principal) loan.principal = Math.round((loan.principal * multiplier) * 100) / 100;
        if (loan.latePenalty) loan.latePenalty = Math.round((loan.latePenalty * multiplier) * 100) / 100;
    });

    (window.appState.payments || []).forEach(p => {
        if (p.amount) p.amount = Math.round((p.amount * multiplier) * 100) / 100;
    });

    if (window.appState.defaultPenalty) {
        window.appState.defaultPenalty = Math.round((window.appState.defaultPenalty * multiplier) * 100) / 100;
        const defPenInput = document.getElementById('setting-default-penalty');
        if (defPenInput) defPenInput.value = window.appState.defaultPenalty;
    }
}

function setTheme(themeName) {
    window.appState.theme = themeName;
    applyTheme(themeName);
    saveToLocalStorage();
    updateParticleThemeColor();
    showToast(`Theme switched to ${themeName.toUpperCase().replace('-', ' ')}`);
    renderDashboardCharts();
}

function applyTheme(themeName) {
    document.documentElement.setAttribute('data-theme', themeName);
}

function initDefaultDates() {
    const today = new Date().toISOString().split('T')[0];
    const loanDateInput = document.getElementById('loan-date');
    const paymentDateInput = document.getElementById('payment-date');
    if (loanDateInput) loanDateInput.value = today;
    if (paymentDateInput) paymentDateInput.value = today;
}

function formatCurrency(num) {
    const val = parseFloat(num) || 0;
    const sym = window.appState.currency || '₱';
    return sym + ' ' + val.toLocaleString('en-US', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
    });
}

function showToast(message, type = 'success') {
    const container = document.getElementById('toast-container');
    if (!container) return;

    const toast = document.createElement('div');
    const isSuccess = type === 'success';

    toast.className = `pointer-events-auto px-4 py-3 rounded-xl shadow-2xl text-xs font-bold flex items-center gap-2 border transition-all duration-300 transform translate-y-2 opacity-0 ${
        isSuccess 
        ? 'bg-slate-900 border-emerald-500/50 text-emerald-300' 
        : 'bg-slate-900 border-rose-500/50 text-rose-300'
    }`;

    toast.innerHTML = `
        <i data-lucide="${isSuccess ? 'check-circle' : 'alert-circle'}" class="w-4 h-4"></i>
        <span>${message}</span>
    `;

    container.appendChild(toast);
    lucide.createIcons();

    setTimeout(() => {
        toast.classList.remove('translate-y-2', 'opacity-0');
    }, 10);

    setTimeout(() => {
        toast.classList.add('opacity-0', 'translate-y-2');
        setTimeout(() => toast.remove(), 300);
    }, 3000);
}

function switchTab(tabName) {
    document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
    document.querySelectorAll('.tab-btn').forEach(btn => {
        btn.classList.remove('border-indigo-500', 'text-indigo-400', 'font-bold');
        btn.classList.add('border-transparent', 'theme-muted', 'font-semibold');
    });

    document.getElementById(`content-${tabName}`).classList.remove('hidden');
    const activeBtn = document.getElementById(`tab-${tabName}`);
    activeBtn.classList.remove('border-transparent', 'theme-muted', 'font-semibold');
    activeBtn.classList.add('border-indigo-500', 'text-indigo-400', 'font-bold');

    if (tabName === 'dashboard') renderDashboardCharts();
    if (tabName === 'ledger') renderLedger();
    lucide.createIcons();
}

function openModal(id) {
    if (id === 'addLoanModal') {
        populateBorrowerDropdown();
        if (window.appState.defaultInterestRate) {
            document.getElementById('loan-rate').value = window.appState.defaultInterestRate;
        }
        if (window.appState.defaultPenalty) {
            document.getElementById('loan-penalty').value = window.appState.defaultPenalty;
        }
    }
    document.getElementById(id).classList.remove('hidden');
}

function closeModal(id) {
    document.getElementById(id).classList.add('hidden');
}

function calculateRealtimeLoanBalance(loan) {
    const principal = parseFloat(loan.principal) || 0;
    const rate = parseFloat(loan.rate) || 0;
    const method = loan.method || 'compound';
    const penaltyPerPeriod = parseFloat(loan.latePenalty) || 0;

    const payments = (window.appState.payments || []).filter(p => p.loanId === loan.id);
    const totalPaid = payments.reduce((sum, p) => sum + (parseFloat(p.amount) || 0), 0);

    const startDate = new Date(loan.startDate);
    const today = new Date();
    let elapsedPeriods = 0;

    if (loan.cycle === 'monthly') {
        elapsedPeriods = (today.getFullYear() - startDate.getFullYear()) * 12 + (today.getMonth() - startDate.getMonth());
    } else if (loan.cycle === 'weekly') {
        const diffMs = Math.abs(today - startDate);
        elapsedPeriods = Math.floor(diffMs / (1000 * 60 * 60 * 24 * 7));
    } else {
        elapsedPeriods = today.getFullYear() - startDate.getFullYear();
    }

    if (elapsedPeriods < 0) elapsedPeriods = 0;

    let currentDebt = principal;
    let totalInterestAccrued = 0;
    let totalPenaltiesAccrued = 0;

    for (let i = 0; i < elapsedPeriods; i++) {
        let interestThisPeriod = (method === 'compound') ? (currentDebt * (rate / 100)) : (principal * (rate / 100));
        let penaltyThisPeriod = penaltyPerPeriod;

        totalInterestAccrued += interestThisPeriod;
        totalPenaltiesAccrued += penaltyThisPeriod;
        currentDebt += interestThisPeriod + penaltyThisPeriod;
    }

    const grossDebt = currentDebt;
    const remainingBalance = Math.max(0, grossDebt - totalPaid);

    return {
        principal,
        grossDebt,
        totalPaid,
        remainingBalance,
        elapsedPeriods,
        totalInterestAccrued,
        totalPenaltiesAccrued
    };
}

function resetCalcForm() {
    document.getElementById('calc-principal').value = '0.00';
    document.getElementById('calc-rate').value = '0.00';
    document.getElementById('calc-penalty').value = '0.00';
    document.getElementById('calc-periods').value = '0';
    updateCalcEngine();
    showToast("Calculator reset to 0.00");
}

function updateCalcEngine() {
    const principal = parseFloat(document.getElementById('calc-principal').value) || 0;
    const rate = parseFloat(document.getElementById('calc-rate').value) || 0;
    const cycle = document.getElementById('calc-cycle').value;
    const method = document.getElementById('calc-method').value;
    const penalty = parseFloat(document.getElementById('calc-penalty').value) || 0;
    const periods = parseInt(document.getElementById('calc-periods').value) || 0;

    document.getElementById('calc-horizon-unit').textContent = cycle === 'monthly' ? 'Months' : (cycle === 'weekly' ? 'Weeks' : 'Years');

    let currentBal = principal;
    let totalInterest = 0;
    let totalPenalties = 0;
    const schedule = [];

    for (let i = 1; i <= periods; i++) {
        const startBal = currentBal;
        let interest = method === 'compound' ? (startBal * (rate / 100)) : (principal * (rate / 100));
        let endBal = startBal + interest + penalty;

        totalInterest += interest;
        totalPenalties += penalty;
        currentBal = endBal;

        schedule.push({ period: i, startBal, interest, penalty, endBal });
    }

    document.getElementById('calc-out-interest').textContent = formatCurrency(totalInterest + totalPenalties);
    document.getElementById('calc-out-total').textContent = formatCurrency(currentBal);
    document.getElementById('calc-schedule-count').textContent = `${periods} Periods`;

    const tbody = document.getElementById('calc-schedule-tbody');
    if (!schedule.length) {
        tbody.innerHTML = `<tr><td colspan="5" class="p-6 text-center theme-muted font-sans">Set principal, rate, and duration parameters above to compute schedule.</td></tr>`;
        return;
    }

    const sym = window.appState.currency || '₱';

    tbody.innerHTML = schedule.map(s => `
        <tr class="hover:bg-slate-800/40">
            <td class="p-3 font-bold">Period ${s.period}</td>
            <td class="p-3 theme-muted">${formatCurrency(s.startBal)}</td>
            <td class="p-3 text-amber-400 font-semibold">+${formatCurrency(s.interest)}</td>
            <td class="p-3 text-rose-400 font-semibold">${s.penalty > 0 ? '+' + formatCurrency(s.penalty) : sym + ' 0.00'}</td>
            <td class="p-3 text-right font-black">${formatCurrency(s.endBal)}</td>
        </tr>
    `).join('');
}

function renderAll() {
    renderDashboardMetrics();
    renderDashboardCharts();
    renderLedger();
    updateCalcEngine(); // <-- ADDED: Re-calculates and applies current currency to simulator outputs
    lucide.createIcons();
}

function renderDashboardMetrics() {
    const loans = window.appState.loans || [];
    let totalPrincipal = 0;
    let totalInterest = 0;
    let totalPenalties = 0;
    let totalOutstanding = 0;
    let totalCollected = 0;
    let activeLoansCount = 0;

    loans.forEach(loan => {
        const calc = calculateRealtimeLoanBalance(loan);
        totalPrincipal += calc.principal;
        totalInterest += calc.totalInterestAccrued;
        totalPenalties += calc.totalPenaltiesAccrued;
        totalOutstanding += calc.remainingBalance;
        totalCollected += calc.totalPaid;
        if (calc.remainingBalance > 0) activeLoansCount++;
    });

    const totalProfit = totalInterest + totalPenalties;

    document.getElementById('metric-active-count').textContent = activeLoansCount;
    document.getElementById('metric-borrower-sub').textContent = `${(window.appState.borrowers || []).length} debtors registered`;
    document.getElementById('metric-total-principal').textContent = formatCurrency(totalPrincipal);
    document.getElementById('metric-total-outstanding').textContent = formatCurrency(totalOutstanding);
    document.getElementById('metric-interest-accrued-sub').textContent = `Includes ${formatCurrency(totalInterest + totalPenalties)} interest & fees`;
    document.getElementById('metric-total-collected').textContent = formatCurrency(totalCollected);

    if (document.getElementById('metric-total-profit')) {
        document.getElementById('metric-total-profit').textContent = formatCurrency(totalProfit);
        document.getElementById('metric-profit-breakdown').textContent = `Interest: ${formatCurrency(totalInterest)} | Fee: ${formatCurrency(totalPenalties)}`;
    }

    document.getElementById('dash-total-loans-txt').textContent = loans.length;
    document.getElementById('dash-paid-loans-txt').textContent = loans.length - activeLoansCount;

    const tbody = document.getElementById('dash-recent-tbody');
    if (!loans.length) {
        tbody.innerHTML = `<tr><td colspan="9" class="p-6 text-center theme-muted font-sans">No active loans recorded. Click <strong class="text-indigo-400 font-semibold">"Issue Loan"</strong> to create one.</td></tr>`;
        return;
    }

    tbody.innerHTML = loans.map(loan => {
        const borrower = (window.appState.borrowers || []).find(b => b.id === loan.borrowerId) || { name: 'Unknown' };
        const calc = calculateRealtimeLoanBalance(loan);
        const isPaid = calc.remainingBalance <= 0;

        return `
            <tr class="hover:bg-slate-800/40 transition">
                <td class="p-3.5 font-bold text-main-color">${escapeHtml(borrower.name)}</td>
                <td class="p-3.5 font-semibold text-indigo-400">${loan.startDate || 'N/A'}</td>
                <td class="p-3.5 theme-muted">${formatCurrency(loan.principal)}</td>
                <td class="p-3.5 theme-muted">${loan.rate}% / ${loan.cycle}</td>
                <td class="p-3.5 text-rose-400">${formatCurrency(calc.totalPenaltiesAccrued)}</td>
                <td class="p-3.5 font-black text-rose-400">${formatCurrency(calc.remainingBalance)}</td>
                <td class="p-3.5 text-emerald-400 font-bold">${formatCurrency(calc.totalPaid)}</td>
                <td class="p-3.5">
                    <span class="px-2.5 py-0.5 text-[10px] font-bold rounded-full ${isPaid ? 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/20' : 'bg-rose-500/10 text-rose-400 border border-rose-500/20'}">
                        ${isPaid ? 'PAID OFF' : 'ACTIVE'}
                    </span>
                </td>
                <td class="p-3.5 text-right">
                    <div class="flex items-center justify-end gap-1.5">
                        ${!isPaid ? `
                            <button onclick="openLogPaymentModal('${loan.id}')" class="px-3 py-1 bg-emerald-600/20 hover:bg-emerald-600/30 text-emerald-400 text-xs font-bold rounded-lg border border-emerald-500/30 transition">
                                Log Payment
                            </button>
                        ` : '<span class="text-xs theme-muted mr-1">Cleared</span>'}
                        <button onclick="promptSingleDelete('${loan.id}', 'loan')" class="p-1 theme-muted hover:text-rose-400 transition" title="Delete Loan">
                            <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                        </button>
                    </div>
                </td>
            </tr>
        `;
    }).join('');
}

function renderDashboardCharts() {
    const ctx1 = document.getElementById('debtGrowthChart')?.getContext('2d');
    if (!ctx1) return;

    const selectedHorizon = document.getElementById('chart-horizon-select')?.value || '12m';
    const loans = window.appState.loans || [];

    let labels = [];
    let periodsCount = 12;

    if (selectedHorizon === '1w') {
        periodsCount = 7;
        labels = Array.from({ length: 8 }, (_, i) => i === 0 ? 'Start' : `Day ${i}`);
    } else if (selectedHorizon === '2w') {
        periodsCount = 14;
        labels = Array.from({ length: 15 }, (_, i) => i === 0 ? 'Start' : `Day ${i}`);
    } else if (selectedHorizon === '1m') {
        periodsCount = 4;
        labels = Array.from({ length: 5 }, (_, i) => i === 0 ? 'Start' : `Week ${i}`);
    } else {
        const months = parseInt(selectedHorizon) || 12;
        periodsCount = months;
        labels = Array.from({ length: months + 1 }, (_, i) => i === 0 ? 'Start' : `Month ${i}`);
    }

    const trajectoryData = new Array(periodsCount + 1).fill(0);

    loans.forEach(loan => {
        const principal = parseFloat(loan.principal) || 0;
        const rate = parseFloat(loan.rate) || 0;
        const penalty = parseFloat(loan.latePenalty) || 0;
        const method = loan.method || 'compound';

        let bal = principal;
        trajectoryData[0] += bal;

        for (let p = 1; p <= periodsCount; p++) {
            let interest = 0;
            if (selectedHorizon === '1w' || selectedHorizon === '2w') {
                interest = method === 'compound' ? (bal * (rate / 100 / 30)) : (principal * (rate / 100 / 30));
            } else if (selectedHorizon === '1m') {
                interest = method === 'compound' ? (bal * (rate / 100 / 4)) : (principal * (rate / 100 / 4));
            } else {
                interest = method === 'compound' ? (bal * (rate / 100)) : (principal * (rate / 100));
            }
            bal += interest + (penalty / (selectedHorizon.includes('w') ? 7 : 1));
            trajectoryData[p] += bal;
        }
    });

    if (window.debtGrowthChartInstance) window.debtGrowthChartInstance.destroy();

    const cssStyle = getComputedStyle(document.documentElement);
    const lineCol = cssStyle.getPropertyValue('--chart-line').trim() || '#6366f1';
    const fillCol = cssStyle.getPropertyValue('--chart-fill').trim() || 'rgba(99, 102, 241, 0.12)';
    const textMainCol = cssStyle.getPropertyValue('--text-main').trim() || '#ffffff';
    const textMutedCol = cssStyle.getPropertyValue('--text-muted').trim() || '#94a3b8';
    const sym = window.appState.currency || '₱';

    window.debtGrowthChartInstance = new Chart(ctx1, {
        type: 'line',
        data: {
            labels,
            datasets: [{
                label: `Projected Portfolio Debt (${sym})`,
                data: trajectoryData,
                borderColor: lineCol,
                backgroundColor: fillCol,
                fill: true,
                tension: 0.35,
                borderWidth: 2.5,
                pointRadius: 4
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { labels: { color: textMainCol, font: { size: 11, weight: '600' } } },
                tooltip: { callbacks: { label: (ctx) => ` Total Owed: ${formatCurrency(ctx.raw)}` } }
            },
            scales: {
                x: { grid: { color: 'rgba(255,255,255,0.08)' }, ticks: { color: textMutedCol, font: { size: 10 } } },
                y: { grid: { color: 'rgba(255,255,255,0.08)' }, ticks: { color: textMutedCol, font: { size: 10 }, callback: v => sym + v.toLocaleString() } }
            }
        }
    });

    const ctx2 = document.getElementById('loanStatusPieChart')?.getContext('2d');
    if (!ctx2) return;

    let activeCount = 0;
    let paidCount = 0;

    loans.forEach(loan => {
        const calc = calculateRealtimeLoanBalance(loan);
        if (calc.remainingBalance > 0) activeCount++;
        else paidCount++;
    });

    if (window.loanStatusPieInstance) window.loanStatusPieInstance.destroy();

    window.loanStatusPieInstance = new Chart(ctx2, {
        type: 'doughnut',
        data: {
            labels: ['Active Debt', 'Fully Settled'],
            datasets: [{
                data: [activeCount, paidCount],
                backgroundColor: ['#f43f5e', '#10b981'],
                borderWidth: 0
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { position: 'bottom', labels: { color: textMainCol, font: { size: 11, weight: '600' } } }
            }
        }
    });
}

function renderLedger() {
    const container = document.getElementById('borrowers-container');
    if (!container) return;

    const search = (document.getElementById('ledger-search')?.value || '').toLowerCase();
    const borrowers = window.appState.borrowers || [];

    if (!borrowers.length) {
        container.innerHTML = `
            <div class="col-span-full theme-card border rounded-2xl p-10 text-center space-y-4">
                <i data-lucide="users" class="w-12 h-12 theme-muted mx-auto"></i>
                <h3 class="text-lg font-bold">No Debtors Registered</h3>
                <p class="text-xs theme-muted max-w-sm mx-auto">Register debtors to track compound interest loans, auto penalties, repayments, and detailed audit history.</p>
                <button onclick="openModal('addBorrowerModal')" class="px-5 py-2.5 bg-emerald-600 hover:bg-emerald-500 text-white text-xs font-bold rounded-xl inline-flex items-center gap-2 shadow-md">
                    <i data-lucide="user-plus" class="w-4 h-4"></i> Add First Debtor
                </button>
            </div>
        `;
        lucide.createIcons();
        return;
    }

    const filtered = borrowers.filter(b => b.name.toLowerCase().includes(search) || (b.notes && b.notes.toLowerCase().includes(search)));

    if (!filtered.length) {
        container.innerHTML = `<div class="col-span-full p-8 text-center text-xs theme-muted">No debtors match your search query.</div>`;
        return;
    }

    container.innerHTML = filtered.map(b => {
        const bLoans = (window.appState.loans || []).filter(l => l.borrowerId === b.id);
        let totalOwed = 0;
        let totalPaid = 0;

        bLoans.forEach(l => {
            const calc = calculateRealtimeLoanBalance(l);
            totalOwed += calc.remainingBalance;
            totalPaid += calc.totalPaid;
        });

        const hasActive = totalOwed > 0;

        return `
            <div class="theme-card border rounded-2xl p-6 flex flex-col justify-between space-y-4 transition hover:border-slate-700 shadow-sm">
                <div>
                    <div class="flex items-start justify-between">
                        <div>
                            <h3 class="font-bold text-base text-main-color">${escapeHtml(b.name)}</h3>
                            <p class="text-xs theme-muted flex items-center gap-1 mt-1">
                                <i data-lucide="phone" class="w-3 h-3"></i> ${escapeHtml(b.contact || 'No phone')}
                            </p>
                        </div>
                        <span class="px-2.5 py-1 text-[10px] font-bold rounded-full ${hasActive ? 'bg-rose-500/10 text-rose-400 border border-rose-500/20' : 'bg-emerald-500/10 text-emerald-400 border border-emerald-500/20'}">
                            ${hasActive ? 'ACTIVE DEBT' : 'CLEARED'}
                        </span>
                    </div>

                    ${b.notes ? `<p class="text-xs theme-muted theme-secondary p-2.5 rounded-lg border border-slate-800 mt-3 line-clamp-2">${escapeHtml(b.notes)}</p>` : ''}

                    <div class="grid grid-cols-2 gap-3 mt-4 pt-3 border-t border-slate-800/80 text-xs">
                        <div>
                            <span class="theme-muted text-[11px] block font-medium">Calculated Owed</span>
                            <span class="font-extrabold text-rose-400 text-sm sm:text-base">${formatCurrency(totalOwed)}</span>
                        </div>
                        <div>
                            <span class="theme-muted text-[11px] block font-medium">Total Repaid</span>
                            <span class="font-extrabold text-emerald-400 text-sm sm:text-base">${formatCurrency(totalPaid)}</span>
                        </div>
                    </div>
                </div>

                <div class="flex items-center gap-2 pt-2">
                    <button onclick="viewBorrowerDetail('${b.id}')" class="flex-1 py-2.5 theme-secondary border text-xs font-bold rounded-xl flex items-center justify-center gap-1.5 transition">
                        <i data-lucide="file-text" class="w-3.5 h-3.5 text-indigo-400"></i> Audit Ledger
                    </button>
                    <button onclick="openEditBorrowerModal('${b.id}')" class="p-2.5 theme-muted hover:text-indigo-400 hover:bg-indigo-500/10 rounded-xl transition" title="Edit Debtor & Notes">
                        <i data-lucide="pencil" class="w-4 h-4"></i>
                    </button>
                    <button onclick="promptSingleDelete('${b.id}', 'borrower')" class="p-2.5 theme-muted hover:text-rose-400 hover:bg-rose-500/10 rounded-xl transition" title="Delete Debtor Record">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                    </button>
                </div>
            </div>
        `;
    }).join('');

    lucide.createIcons();
}

function handleAddBorrower(e) {
    e.preventDefault();
    const name = document.getElementById('borrower-name').value.trim();
    const contact = document.getElementById('borrower-contact').value.trim();
    const notes = document.getElementById('borrower-notes').value.trim();

    if (!name) return;

    const borrowerObj = {
        id: 'b_' + Date.now(),
        name,
        contact,
        notes,
        createdAt: new Date().toISOString()
    };

    window.appState.borrowers.push(borrowerObj);
    saveToLocalStorage();
    renderAll();
    closeModal('addBorrowerModal');
    document.getElementById('form-add-borrower').reset();
    showToast(`Debtor "${name}" registered successfully.`);
}

function openEditBorrowerModal(borrowerId) {
    const borrower = (window.appState.borrowers || []).find(b => b.id === borrowerId);
    if (!borrower) return;

    document.getElementById('edit-borrower-id').value = borrower.id;
    document.getElementById('edit-borrower-name').value = borrower.name || '';
    document.getElementById('edit-borrower-contact').value = borrower.contact || '';
    document.getElementById('edit-borrower-notes').value = borrower.notes || '';

    openModal('editBorrowerModal');
}

function handleEditBorrower(e) {
    e.preventDefault();
    const id = document.getElementById('edit-borrower-id').value;
    const name = document.getElementById('edit-borrower-name').value.trim();
    const contact = document.getElementById('edit-borrower-contact').value.trim();
    const notes = document.getElementById('edit-borrower-notes').value.trim();

    if (!name) return;

    const borrowerIndex = (window.appState.borrowers || []).findIndex(b => b.id === id);
    if (borrowerIndex !== -1) {
        window.appState.borrowers[borrowerIndex].name = name;
        window.appState.borrowers[borrowerIndex].contact = contact;
        window.appState.borrowers[borrowerIndex].notes = notes;

        saveToLocalStorage();
        renderAll();
        closeModal('editBorrowerModal');
        if (!document.getElementById('borrowerDetailModal').classList.contains('hidden')) {
            viewBorrowerDetail(id);
        }
        showToast(`Debtor notes & profile updated!`);
    }
}

function populateBorrowerDropdown() {
    const select = document.getElementById('loan-borrower-id');
    const borrowers = window.appState.borrowers || [];

    if (!borrowers.length) {
        select.innerHTML = `<option value="">-- No debtors found. Add a debtor first --</option>`;
        return;
    }

    select.innerHTML = borrowers.map(b => `
        <option value="${b.id}">${escapeHtml(b.name)} (${b.contact || 'No contact'})</option>
    `).join('');
}

function handleAddLoan(e) {
    e.preventDefault();
    const borrowerId = document.getElementById('loan-borrower-id').value;
    const principal = parseFloat(document.getElementById('loan-principal').value) || 0;
    const rate = parseFloat(document.getElementById('loan-rate').value) || 0;
    const cycle = document.getElementById('loan-cycle').value;
    const method = document.getElementById('loan-method').value;
    const penalty = parseFloat(document.getElementById('loan-penalty').value) || 0;
    const startDate = document.getElementById('loan-date').value;

    if (!borrowerId) {
        showToast("Please select a debtor", "error");
        return;
    }

    const loanObj = {
        id: 'l_' + Date.now(),
        borrowerId,
        principal,
        rate,
        cycle,
        method,
        latePenalty: penalty,
        startDate,
        createdAt: new Date().toISOString()
    };

    window.appState.loans.push(loanObj);
    saveToLocalStorage();
    renderAll();
    closeModal('addLoanModal');
    document.getElementById('form-add-loan').reset();
    showToast("Loan issued and recorded permanently!");
}

function openLogPaymentModal(loanId) {
    const loan = (window.appState.loans || []).find(l => l.id === loanId);
    if (!loan) return;

    const borrower = (window.appState.borrowers || []).find(b => b.id === loan.borrowerId);
    const calc = calculateRealtimeLoanBalance(loan);

    document.getElementById('payment-loan-id').value = loan.id;
    document.getElementById('pay-modal-borrower-name').textContent = borrower ? borrower.name : 'Unknown';
    document.getElementById('pay-modal-current-balance').textContent = formatCurrency(calc.remainingBalance);
    document.getElementById('payment-amount').value = calc.remainingBalance.toFixed(2);

    openModal('logPaymentModal');
}

function handleLogPayment(e) {
    e.preventDefault();
    const loanId = document.getElementById('payment-loan-id').value;
    const amount = parseFloat(document.getElementById('payment-amount').value) || 0;
    const date = document.getElementById('payment-date').value;
    const notes = document.getElementById('payment-notes').value.trim();

    if (amount <= 0) {
        showToast("Please enter a valid amount", "error");
        return;
    }

    const loan = (window.appState.loans || []).find(l => l.id === loanId);
    if (!loan) return;

    const paymentObj = {
        id: 'p_' + Date.now(),
        loanId,
        borrowerId: loan.borrowerId,
        amount,
        date,
        notes,
        createdAt: new Date().toISOString()
    };

    window.appState.payments.push(paymentObj);
    saveToLocalStorage();
    renderAll();
    closeModal('logPaymentModal');
    document.getElementById('form-log-payment').reset();
    showToast(`Logged repayment of ${formatCurrency(amount)}!`);
}

function viewBorrowerDetail(borrowerId) {
    const borrower = (window.appState.borrowers || []).find(b => b.id === borrowerId);
    if (!borrower) return;

    document.getElementById('detail-borrower-name').textContent = borrower.name;
    document.getElementById('detail-borrower-contact').textContent = borrower.contact || 'No contact info';
    
    const auditBtnContainer = document.getElementById('detail-borrower-actions');
    if (auditBtnContainer) {
        auditBtnContainer.innerHTML = `
            <button onclick="openEditBorrowerModal('${borrower.id}')" class="px-3 py-1.5 theme-secondary border text-xs font-semibold rounded-xl flex items-center gap-1.5 hover:border-indigo-400 transition">
                <i data-lucide="pencil" class="w-3.5 h-3.5 text-indigo-400"></i> Edit Notes
            </button>
            <button onclick="printBorrowerAudit('${borrower.id}')" class="px-3 py-1.5 bg-emerald-600 hover:bg-emerald-500 text-white text-xs font-bold rounded-xl flex items-center gap-1.5 shadow-md transition">
                <i data-lucide="printer" class="w-3.5 h-3.5"></i> Print Audit Record
            </button>
        `;
    }

    const bLoans = (window.appState.loans || []).filter(l => l.borrowerId === borrowerId);
    const container = document.getElementById('detail-loan-cards-container');

    if (!bLoans.length) {
        container.innerHTML = `<div class="p-6 text-center text-xs theme-muted">No active or historical loans recorded for this debtor.</div>`;
    } else {
        container.innerHTML = bLoans.map((loan, idx) => {
            const calc = calculateRealtimeLoanBalance(loan);
            const payments = (window.appState.payments || []).filter(p => p.loanId === loan.id);

            return `
                <div class="theme-secondary border rounded-xl p-5 space-y-4">
                    <div class="flex justify-between items-start border-b border-slate-800/80 pb-3">
                        <div>
                            <span class="text-xs font-bold text-indigo-400">Loan Record #${idx + 1}</span>
                            <div class="text-[11px] theme-muted mt-0.5">Borrowed Date: <strong class="text-main-color">${loan.startDate}</strong> | ${loan.method.toUpperCase()} (${loan.rate}% / ${loan.cycle})</div>
                        </div>
                        <div class="flex items-center gap-2">
                            <span class="text-xs font-extrabold ${calc.remainingBalance <= 0 ? 'text-emerald-400' : 'text-rose-400'}">
                                Balance: ${formatCurrency(calc.remainingBalance)}
                            </span>
                            <button onclick="promptSingleDelete('${loan.id}', 'loan')" class="theme-muted hover:text-rose-400 p-1" title="Delete Loan">
                                <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                            </button>
                        </div>
                    </div>

                    <div class="grid grid-cols-4 gap-3 text-xs">
                        <div><span class="theme-muted block font-medium">Principal:</span> ${formatCurrency(loan.principal)}</div>
                        <div><span class="theme-muted block font-medium">Interest Profit:</span> ${formatCurrency(calc.totalInterestAccrued)}</div>
                        <div><span class="theme-muted block font-medium">Late Penalties:</span> ${formatCurrency(calc.totalPenaltiesAccrued)}</div>
                        <div><span class="theme-muted block font-medium">Total Paid:</span> ${formatCurrency(calc.totalPaid)}</div>
                    </div>

                    <div class="pt-2 border-t border-slate-800/60">
                        <h5 class="text-xs font-bold theme-muted mb-2 flex items-center gap-1.5">
                            <i data-lucide="receipt" class="w-3.5 h-3.5 text-emerald-400"></i> Repayments Logged (${payments.length})
                        </h5>
                        ${!payments.length ? '<p class="text-xs theme-muted italic">No repayments recorded yet.</p>' : `
                            <div class="space-y-2 max-h-40 overflow-y-auto pr-1">
                                ${payments.map(p => `
                                    <div class="flex justify-between items-center text-xs theme-card p-2.5 rounded-lg border border-slate-800">
                                        <div>
                                            <span class="font-bold text-main-color">${p.date}</span>
                                            <span class="theme-muted ml-2">${escapeHtml(p.notes || 'Repayment')}</span>
                                        </div>
                                        <div class="flex items-center gap-2">
                                            <span class="text-emerald-400 font-bold">+${formatCurrency(p.amount)}</span>
                                            <button onclick="printPaymentReceipt('${p.id}')" class="p-1 theme-muted hover:text-emerald-400" title="Print Receipt">
                                                <i data-lucide="printer" class="w-3.5 h-3.5"></i>
                                            </button>
                                            <button onclick="promptSingleDelete('${p.id}', 'payment')" class="p-1 theme-muted hover:text-rose-400" title="Delete Payment">
                                                <i data-lucide="trash-2" class="w-3.5 h-3.5"></i>
                                            </button>
                                        </div>
                                    </div>
                                `).join('')}
                            </div>
                        `}
                    </div>
                </div>
            `;
        }).join('');
    }

    openModal('borrowerDetailModal');
    lucide.createIcons();
}

// Print Receipt Feature
function printPaymentReceipt(paymentId) {
    const payment = (window.appState.payments || []).find(p => p.id === paymentId);
    if (!payment) return;

    const borrower = (window.appState.borrowers || []).find(b => b.id === payment.borrowerId) || { name: 'Valued Debtor', contact: 'N/A' };
    const loan = (window.appState.loans || []).find(l => l.id === payment.loanId) || { principal: 0, startDate: 'N/A' };
    const calc = calculateRealtimeLoanBalance(loan);

    const printWin = window.open('', '_blank', 'width=650,height=700');
    if (!printWin) {
        showToast("Please allow popups to print receipt", "error");
        return;
    }

    printWin.document.write(`
        <!DOCTYPE html>
        <html>
        <head>
            <title>Official Receipt - Personal Loan Management System & Debtor Ledger</title>
            <style>
                body { font-family: 'Helvetica Neue', Arial, sans-serif; padding: 30px; color: #1e293b; max-width: 500px; margin: 0 auto; line-height: 1.5; }
                .receipt-box { border: 2px dashed #94a3b8; padding: 25px; border-radius: 12px; }
                .header { text-align: center; border-bottom: 2px solid #e2e8f0; padding-bottom: 15px; margin-bottom: 20px; }
                .header h1 { margin: 0; font-size: 24px; color: #0f172a; text-transform: uppercase; letter-spacing: 1px; }
                .header p { margin: 4px 0 0; font-size: 12px; color: #64748b; }
                .row { display: flex; justify-content: space-between; font-size: 13px; margin-bottom: 8px; }
                .row.bold { font-weight: bold; font-size: 15px; color: #0f172a; }
                .divider { border-top: 1px solid #e2e8f0; margin: 15px 0; }
                .stamp { text-align: center; margin-top: 25px; font-weight: bold; color: #059669; border: 2px solid #059669; padding: 6px 12px; display: inline-block; border-radius: 6px; transform: rotate(-3deg); }
                .footer { text-align: center; font-size: 11px; color: #94a3b8; margin-top: 25px; }
            </style>
        </head>
        <body>
            <div class="receipt-box">
                <div class="header">
                    <h1>Loan Management System & Debtor Ledger</h1>
                    <p>OFFICIAL REPAYMENT RECEIPT</p>
                    <p>Ref: ${payment.id.toUpperCase()}</p>
                </div>

                <div class="row"><span>Date Logged:</span> <strong>${payment.date}</strong></div>
                <div class="row"><span>Debtor Name:</span> <strong>${borrower.name}</strong></div>
                <div class="row"><span>Contact:</span> <strong>${borrower.contact || 'N/A'}</strong></div>
                
                <div class="divider"></div>

                <div class="row"><span>Original Principal:</span> <strong>${formatCurrency(loan.principal)}</strong></div>
                <div class="row"><span>Payment Reference/Note:</span> <strong>${payment.notes || 'Cash Settlement'}</strong></div>
                
                <div class="divider"></div>

                <div class="row bold"><span>AMOUNT PAID:</span> <span style="color: #059669;">+${formatCurrency(payment.amount)}</span></div>
                <div class="row"><span>Remaining Owed:</span> <strong>${formatCurrency(calc.remainingBalance)}</strong></div>

                <div style="text-align: center;">
                    <div class="stamp">PAYMENT CONFIRMED</div>
                </div>

                <div class="footer">
                    Thank you for your payment.<br>
                    &copy; Ian Uozoia 2026.
                </div>
            </div>
            <script>
                window.onload = function() { window.print(); }
            </script>
        </body>
        </html>
    `);
    printWin.document.close();
}

// Print Full Debtor Audit Record
function printBorrowerAudit(borrowerId) {
    const borrower = (window.appState.borrowers || []).find(b => b.id === borrowerId);
    if (!borrower) return;

    const bLoans = (window.appState.loans || []).filter(l => l.borrowerId === borrowerId);

    const printWin = window.open('', '_blank', 'width=800,height=900');
    if (!printWin) {
        showToast("Please allow popups to print audit record", "error");
        return;
    }

    let loansHTML = '';
    let totalOwedAll = 0;
    let totalPaidAll = 0;

    bLoans.forEach((loan, idx) => {
        const calc = calculateRealtimeLoanBalance(loan);
        const payments = (window.appState.payments || []).filter(p => p.loanId === loan.id);
        totalOwedAll += calc.remainingBalance;
        totalPaidAll += calc.totalPaid;

        loansHTML += `
            <div style="border: 1px solid #cbd5e1; border-radius: 8px; padding: 15px; margin-bottom: 15px;">
                <h3 style="margin: 0 0 10px; font-size: 14px; color: #1e293b;">Loan Entry #${idx + 1} (${loan.startDate})</h3>
                <div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; font-size: 12px; margin-bottom: 10px;">
                    <div><strong>Principal:</strong> ${formatCurrency(loan.principal)}</div>
                    <div><strong>Rate:</strong> ${loan.rate}% (${loan.cycle})</div>
                    <div><strong>Interest Accrued:</strong> ${formatCurrency(calc.totalInterestAccrued)}</div>
                    <div><strong>Remaining Owed:</strong> <span style="color: #e11d48; font-weight: bold;">${formatCurrency(calc.remainingBalance)}</span></div>
                </div>

                <h4 style="font-size: 12px; margin: 10px 0 5px; color: #475569;">Repayment History (${payments.length}):</h4>
                <table style="width: 100%; font-size: 11px; border-collapse: collapse;">
                    <thead>
                        <tr style="background: #f1f5f9; text-align: left;">
                            <th style="padding: 4px;">Date</th>
                            <th style="padding: 4px;">Reference / Note</th>
                            <th style="padding: 4px; text-align: right;">Amount Paid</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${!payments.length ? '<tr><td colspan="3" style="padding: 4px; font-style: italic; color: #94a3b8;">No repayments logged yet.</td></tr>' : 
                            payments.map(p => `
                                <tr style="border-bottom: 1px solid #f1f5f9;">
                                    <td style="padding: 4px;">${p.date}</td>
                                    <td style="padding: 4px;">${p.notes || '-'}</td>
                                    <td style="padding: 4px; text-align: right; font-weight: bold; color: #059669;">+${formatCurrency(p.amount)}</td>
                                </tr>
                            `).join('')
                        }
                    </tbody>
                </table>
            </div>
        `;
    });

    printWin.document.write(`
        <!DOCTYPE html>
        <html>
        <head>
            <title>Audit Record - ${borrower.name}</title>
            <style>
                body { font-family: 'Helvetica Neue', Arial, sans-serif; padding: 30px; color: #1e293b; max-width: 750px; margin: 0 auto; line-height: 1.5; }
                .header { border-bottom: 2px solid #0f172a; padding-bottom: 10px; margin-bottom: 20px; display: flex; justify-content: space-between; align-items: flex-end; }
                .header h1 { margin: 0; font-size: 26px; color: #0f172a; }
                .summary { background: #f8fafc; border: 1px solid #e2e8f0; padding: 15px; border-radius: 8px; margin-bottom: 20px; font-size: 13px; }
            </style>
        </head>
        <body>
            <div class="header">
                <div>
                    <h1>Personal Loan Management System & Debtor Ledger</h1>
                    <div style="font-size: 12px; color: #64748b;">DEBTOR AUDIT & PORTFOLIO RECORD</div>
                </div>
                <div style="font-size: 12px; text-align: right;">Date Generated:<br><strong>${new Date().toISOString().split('T')[0]}</strong></div>
            </div>

            <div class="summary">
                <div style="font-size: 16px; font-weight: bold; color: #0f172a; margin-bottom: 4px;">${borrower.name}</div>
                <div>Phone / Contact: ${borrower.contact || 'N/A'}</div>
                <div>Notes: ${borrower.notes || 'None recorded'}</div>
                <hr style="border: 0; border-top: 1px solid #e2e8f0; margin: 10px 0;">
                <div style="display: flex; justify-content: space-between; font-weight: bold; font-size: 14px;">
                    <span>Total Outstanding Debt: <span style="color: #e11d48;">${formatCurrency(totalOwedAll)}</span></span>
                    <span>Total Settled Repayments: <span style="color: #059669;">${formatCurrency(totalPaidAll)}</span></span>
                </div>
            </div>

            <h2 style="font-size: 16px; color: #0f172a;">Associated Loan Statements</h2>
            ${loansHTML || '<p style="font-size: 12px; color: #64748b;">No loans recorded for this debtor.</p>'}

            <div style="text-align: center; margin-top: 30px; font-size: 11px; color: #94a3b8; border-top: 1px solid #e2e8f0; padding-top: 10px;">
                &copy; Ian Uozoia 2026. Confidential Financial Ledger Document.
            </div>
            <script>
                window.onload = function() { window.print(); }
            </script>
        </body>
        </html>
    `);
    printWin.document.close();
}

function exportLedgerCSV() {
    const loans = window.appState.loans || [];
    if (!loans.length) {
        showToast("No loan data available to export", "error");
        return;
    }

    let csvContent = "data:text/csv;charset=utf-8,Debtor Name,Contact,Start Date,Principal,Rate (%),Cycle,Interest Method,Penalties,Total Paid,Current Owed,Status\n";

    loans.forEach(loan => {
        const borrower = (window.appState.borrowers || []).find(b => b.id === loan.borrowerId) || { name: 'Unknown', contact: '' };
        const calc = calculateRealtimeLoanBalance(loan);
        const status = calc.remainingBalance <= 0 ? 'PAID OFF' : 'ACTIVE';

        const row = [
            `"${borrower.name}"`,
            `"${borrower.contact || ''}"`,
            `"${loan.startDate}"`,
            loan.principal,
            loan.rate,
            `"${loan.cycle}"`,
            `"${loan.method}"`,
            calc.totalPenaltiesAccrued,
            calc.totalPaid,
            calc.remainingBalance,
            `"${status}"`
        ].join(",");
        csvContent += row + "\n";
    });

    const encodedUri = encodeURI(csvContent);
    const link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", `Debtors_Report_${new Date().toISOString().split('T')[0]}.csv`);
    document.body.appendChild(link);
    link.click();
    link.remove();
    showToast("CSV Ledger report downloaded!");
}

function promptSingleDelete(id, type) {
    document.getElementById('single-delete-id').value = id;
    document.getElementById('single-delete-type').value = type;

    const titleEl = document.getElementById('single-delete-title');
    const msgEl = document.getElementById('single-delete-msg');

    if (type === 'borrower') {
        titleEl.textContent = "Delete Debtor Record";
        msgEl.textContent = "This will remove the borrower along with all associated loan records and payment logs.";
    } else if (type === 'loan') {
        titleEl.textContent = "Delete Loan Record";
        msgEl.textContent = "This will remove this specific loan entry and its payment history.";
    } else if (type === 'payment') {
        titleEl.textContent = "Delete Payment Entry";
        msgEl.textContent = "This will remove this repayment record and recalculate remaining debt balance.";
    }

    openModal('singleDeleteModal');
}

function executeSingleDelete() {
    const id = document.getElementById('single-delete-id').value;
    const type = document.getElementById('single-delete-type').value;

    if (type === 'borrower') {
        window.appState.borrowers = window.appState.borrowers.filter(b => b.id !== id);
        window.appState.loans = window.appState.loans.filter(l => l.borrowerId !== id);
        window.appState.payments = window.appState.payments.filter(p => p.borrowerId !== id);
        showToast("Debtor and associated records deleted.");
        closeModal('borrowerDetailModal');
    } else if (type === 'loan') {
        window.appState.loans = window.appState.loans.filter(l => l.id !== id);
        window.appState.payments = window.appState.payments.filter(p => p.loanId !== id);
        showToast("Loan record deleted.");
        closeModal('borrowerDetailModal');
    } else if (type === 'payment') {
        window.appState.payments = window.appState.payments.filter(p => p.id !== id);
        showToast("Payment entry deleted.");
        closeModal('borrowerDetailModal');
    }

    saveToLocalStorage();
    renderAll();
    closeModal('singleDeleteModal');
}

function triggerResetConfirmationModal() {
    closeModal('settingsModal');
    document.getElementById('reset-confirm-input').value = '';
    openModal('dangerConfirmModal');
}

function executeSystemReset() {
    const confirmVal = document.getElementById('reset-confirm-input').value.trim();

    if (confirmVal !== 'RESET') {
        showToast("Please type RESET to confirm wiping system data", "error");
        return;
    }

    window.appState.borrowers = [];
    window.appState.loans = [];
    window.appState.payments = [];

    localStorage.removeItem('lendwise_b');
    localStorage.removeItem('lendwise_l');
    localStorage.removeItem('lendwise_p');

    renderAll();
    closeModal('dangerConfirmModal');
    showToast("SYSTEM WIPED SUCCESSFULLY. All metrics reset to 0.00.");
}

function escapeHtml(str) {
    if (!str) return '';
    return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#039;");
}
