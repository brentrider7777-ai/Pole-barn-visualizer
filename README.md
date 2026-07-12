<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Next-Gen Pole Barn Configurator</title>
    <!-- Tailwind CSS with modern UI variables -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <!-- FontAwesome for flashy icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @keyframes pulse-fast { 0%, 100% { opacity: 1; } 50% { opacity: 0.3; } }
        .live-glow { animation: pulse-fast 1.5s infinite; }
    </style>
</head>
<body class="bg-slate-950 text-white font-sans selection:bg-amber-500 selection:text-slate-900">

    <!-- Flashy Urgency Banner -->
    <div class="bg-gradient-to-r from-amber-600 via-orange-500 to-yellow-500 text-slate-950 text-center py-2 px-4 text-xs font-black tracking-widest uppercase shadow-lg flex items-center justify-center gap-2">
        <i class="fa-solid fa-fire text-sm animate-bounce"></i>
        <span>Steel Lock-In Event: Save up to 15% on All Kits reserved in the next <span id="countdown">14:59</span></span>
    </div>

    <!-- Main Navigation -->
    <header class="border-b border-slate-800 bg-slate-900/80 backdrop-blur-md sticky top-0 z-50 py-4 px-6 flex justify-between items-center">
        <div class="flex items-center gap-2">
            <div class="bg-amber-500 p-2 rounded-xl text-slate-950 shadow-[0_0_15px_rgba(245,158,11,0.5)]">
                <i class="fa-solid fa-campground text-lg"></i>
            </div>
            <span class="text-xl font-black tracking-tighter bg-gradient-to-r from-white to-slate-400 bg-clip-text text-transparent">IRONCLAD<span class="text-amber-500">BARNS</span></span>
        </div>
        <div class="flex items-center gap-4">
            <span class="hidden md:inline-flex items-center gap-1.5 text-xs text-emerald-400 bg-emerald-500/10 px-2.5 py-1 rounded-full border border-emerald-500/20 font-mono">
                <span class="w-2 h-2 rounded-full bg-emerald-400 live-glow"></span> 42 Designers Active Live
            </span>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4 lg:p-8 grid grid-cols-1 lg:grid-cols-12 gap-8">
        
        <!-- Left Side: Interactive Engine Viewport -->
        <div class="lg:col-span-7 space-y-4">
            <div class="bg-slate-900/50 border border-slate-800 rounded-3xl p-6 shadow-2xl relative overflow-hidden backdrop-blur-sm">
                <!-- Absolute background accents -->
                <div class="absolute -top-24 -left-24 w-48 h-48 bg-amber-500/10 blur-3xl rounded-full"></div>
                
                <div class="flex justify-between items-center mb-4 relative z-10">
                    <div>
                        <span class="text-[10px] font-bold tracking-widest text-amber-500 uppercase">3D Perspective Arena</span>
                        <h2 class="text-xl font-black tracking-tight mt-0.5">Your Blueprint Draft</h2>
                    </div>
                    <!-- Real-time dynamic feature badge -->
                    <div class="bg-slate-800 border border-slate-700 px-3 py-1 rounded-xl text-xs font-mono text-slate-300">
                        FPS: <span class="text-emerald-400 font-bold">60</span>
                    </div>
                </div>
                
                <!-- Canvas Stage -->
                <div class="bg-slate-950/80 rounded-2xl border border-slate-800/80 overflow-hidden flex items-center justify-center min-h-[350px] lg:min-h-[420px] relative shadow-inner">
                    <canvas id="barnCanvas" width="600" height="400" class="w-full h-auto max-w-full drop-shadow-[0_20px_50px_rgba(0,0,0,0.8)]"></canvas>
                </div>

                <!-- Dynamic Pricing Panel with Massive Contrast -->
                <div class="mt-6 p-5 bg-gradient-to-br from-slate-900 to-slate-950 rounded-2xl border border-slate-800 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                    <div>
                        <p class="text-xs text-slate-400 font-bold uppercase tracking-wider">Estimated Materials Total</p>
                        <div class="flex items-baseline gap-2 mt-1">
                            <span class="text-3xl font-black tracking-tight text-white" id="priceDisplay">$12,400</span>
                            <span class="text-xs text-emerald-400 font-medium line-through decoration-red-500/80" id="retailPrice">$14,580</span>
                        </div>
                    </div>
                    <div class="bg-amber-500/10 border border-amber-500/20 px-4 py-2.5 rounded-xl flex items-center gap-3 w-full sm:w-auto">
                        <i class="fa-solid fa-bolt text-amber-400 text-lg"></i>
                        <div class="text-left">
                            <p class="text-[10px] uppercase font-black text-amber-400 tracking-wider">Instant Bonus Activated</p>
                            <p class="text-xs text-slate-300 font-semibold">Free Trim Upgrade Included</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Live Conversion Proof Ticker -->
            <div class="bg-slate-900/30 border border-slate-800/50 rounded-2xl px-4 py-3 flex items-center justify-between text-xs text-slate-400">
                <div class="flex items-center gap-2">
                    <div class="w-2 h-2 rounded-full bg-amber-500 animate-ping"></div>
                    <span><strong class="text-white">Marcus T.</strong> from Zip 48827 just locked in a 30x40 kit price</span>
                </div>
                <span class="text-slate-500 hidden sm:inline">2 mins ago</span>
            </div>
        </div>

        <!-- Right Side: Control Hub & Psychology Lead Funnel -->
        <div class="lg:col-span-5 space-y-6">
            
            <!-- Modern Gamified Stepper Navigation -->
            <div class="grid grid-cols-2 gap-2 bg-slate-900 p-1.5 rounded-2xl border border-slate-800">
                <button onclick="switchTab('design')" id="tab-design" class="w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl bg-amber-500 text-slate-950 shadow-lg transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-sliders"></i> 1. Customize
                </button>
                <button onclick="switchTab('quote')" id="tab-quote" class="w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl text-slate-400 hover:text-white transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-lock"></i> 2. Claim Kit
                </button>
            </div>

            <!-- Panel 1: Configuration Form -->
            <div id="panel-design" class="bg-slate-900/60 border border-slate-800 p-6 rounded-3xl shadow-2xl space-y-6">
                <!-- Dimensions Configuration Area -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3 flex items-center justify-between">
                        <span>Select Footprint Size</span>
                        <span class="text-amber-500 normal-case font-medium font-mono text-[11px]" id="sqftDisplay">1200 Sq Ft</span>
                    </label>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <select id="barnWidth" onchange="updateConfig()" class="w-full appearance-none bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white focus:outline-none focus:border-amber-500 focus:ring-1 focus:ring-amber-500 transition-all cursor-pointer">
                                <option value="24">24 Ft Wide</option>
                                <option value="30" selected>30 Ft Wide</option>
                                <option value="40">40 Ft Wide</option>
                            </select>
                            <i class="fa-solid fa-chevron-down absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 text-xs pointer-events-none"></i>
                        </div>
                        <div class="relative">
                            <select id="barnLength" onchange="updateConfig()" class="w-full appearance-none bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white focus:outline-none focus:border-amber-500 focus:ring-1 focus:ring-amber-500 transition-all cursor-pointer">
                                <option value="30">30 Ft Long</option>
                                <option value="40" selected>40 Ft Long</option>
                                <option value="60">60 Ft Long</option>
                            </select>
                            <i class="fa-solid fa-chevron-down absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 text-xs pointer-events-none"></i>
                        </div>
                    </div>
                </div>

                <!-- Commercial Color Profiles -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">Siding & Trim Aesthetics</label>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <select id="wallColor" onchange="updateConfig()" class="w-full appearance-none bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-xs font-bold text-white focus:outline-none focus:border-amber-500 transition-all cursor-pointer">
                                <option value="#991b1b" selected>🔴 Crimson Red</option>
                                <option value="#1e293b">⚫ Charcoal Grey</option>
                                <option value="#14532d">🟢 Forest Green</option>
                                <option value="#f8fafc">⚪ Bright White</option>
                            </select>
                            <i class="fa-solid fa-palette absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 text-xs pointer-events-none"></i>
                        </div>
                        <div class="relative">
                            <select id="roofColor" onchange="updateConfig()" class="w-full appearance-none bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-xs font-bold text-white focus:outline-none focus:border-amber-500 transition-all cursor-pointer">
                                <option value="#1e293b" selected>⚫ Charcoal Trim</option>
                                <option value="#ffffff">⚪ White Trim</option>
                                <option value="#7f1d1d">🟤 Burgundy Trim</option>
                            </select>
                            <i class="fa-solid fa-palette absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 text-xs pointer-events-none"></i>
                        </div>
                    </div>
                </div>

                <!-- System Add-ons / Upsells -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">Premium Architecture Upgrades</label>
                    <div class="space-y-2.5">
                        <label class="flex items-center justify-between bg-slate-950/50 p-3.5 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer transition-all">
                            <div class="flex items-center gap-3">
                                <input type="checkbox" id="addWainscot" onchange="updateConfig()" class="w-4 h-4 rounded bg-slate-900 border-slate-700 text-amber-500 accent-amber-500 focus:ring-0">
                                <div>
                                    <p class="text-xs font-bold text-white">Two-Tone Wainscoting</p>
                                    <p class="text-[10px] text-slate-500">Premium 3-foot color breakup</p>
                                </div>
                            </div>
                            <span class="text-xs font-mono font-bold text-emerald-400">+$850</span>
                        </label>

                        <label class="flex items-center justify-between bg-slate-950/50 p-3.5 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer transition-all">
                            <div class="flex items-center gap-3">
                                <input type="checkbox" id="addDoor" onchange="updateConfig()" checked class="w-4 h-4 rounded bg-slate-900 border-slate-700 text-amber-500 accent-amber-500 focus:ring-0">
                                <div>
                                    <p class="text-xs font-bold text-white">Double Sliding Structural Door</p>
                                    <p class="text-[10px] text-slate-500">12'x12' heavy duty structural track</p>
                                </div>
                            </div>
                            <span class="text-xs font-mono font-bold text-emerald-400">+$1,200</span>
                        </label>
                    </div>
                </div>

                <!-- Call to Action button with glowing pulses -->
                <button onclick="switchTab('quote')" class="w-full relative group overflow-hidden bg-gradient-to-r from-amber-500 to-orange-500 text-slate-950 font-black text-xs uppercase tracking-widest py-4 rounded-xl shadow-[0_4px_20px_rgba(245,158,11,0.3)] hover:shadow-[0_4px_25px_rgba(245,158,11,0.5)] transition-all transform hover:-translate-y-0.5">
                    <span class="relative z-10 flex items-center justify-center gap-2">
                        Lock In Promotional Price <i class="fa-solid fa-arrow-right text-sm"></i>
                    </span>
                    <div class="absolute inset-0 w-full h-full bg-white/20 transform -skew-x-12 -translate-x-full group-hover:translate-x-full transition-transform duration-1000"></div>
                </button>
            </div>

            <!-- Panel 2: Marketing Lead Form -->
            <div id="panel-quote" class="bg-slate-900/60 border border-slate-800 p-6 rounded-3xl shadow-2xl space-y-5 hidden">
                <div class="text-center pb-3 border-b border-slate-800">
                    <span class="bg-amber-500/10 text-amber-400 text-[10px] font-black uppercase px-2.5 py-1 rounded-md border border-amber-500/20 tracking-wider">Final Step</span>
                    <h3 class="text-lg font-black tracking-tight mt-2">Generate Complete Material Manifest</h3>
                    <p class="text-xs text-slate-400 mt-1">We will send your system specs, engineering layouts, and regional delivery routing costs.</p>
                </div>

                <form id="leadForm" onsubmit="handleLeadSubmit(event)" class="space-y-4">
                    <div>
                        <label class="block text-[10px] uppercase font-black tracking-wider text-slate-400 mb-1">Your Direct Contact Name</label>
                        <input type="text" required placeholder="First and Last Name" class="w-full bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white placeholder-slate-600 focus:outline-none focus:border-amber-500 transition-all">
                    </div>
                    <div>
                        <label class="block text-[10px] uppercase font-black tracking-wider text-slate-400 mb-1">Secure Delivery Email Address</label>
                        <input type="email" required placeholder="name@domain.com" class="w-full bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white placeholder-slate-600 focus:outline-none focus:border-amber-500 transition-all">
                    </div>
                    <div>
                        <label class="block text-[10px] uppercase font-black tracking-wider text-slate-400 mb-1">Target Build Zip Code</label>
                        <input type="text" required placeholder="e.g. 48827" class="w-full bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white placeholder-slate-600 focus:outline-none focus:border-amber-500 transition-all">
                    </div>

                    <button type="submit" class="w-full bg-gradient-to-r from-emerald-500 to-teal-500 text-slate-950 font-black text-xs uppercase tracking-widest py-4 rounded-xl shadow-[0_4px_20px_rgba(16,185,129,0.3)] hover:shadow-[0_4px_25px_rgba(16,185,129,0.5)] transition-all transform hover:-translate-y-0.5">
                        <i class="fa-solid fa-cloud-arrow-down mr-1"></i> Send Instant Blueprints
                    </button>
                </form>
                
                <button onclick="switchTab('design')" class="w-full text-center text-[11px] font-bold text-slate-500 hover:text-slate-300 transition-all underline">
                    ← Change Dimensions or Colors
                </button>
            </div>

        </div>
    </main>

    <!-- Marketing Engine Logic Routine -->
    <script>
        const canvas = document.getElementById('barnCanvas');
        const ctx = canvas.getContext('2d');

        // Scarcity Countdown Engine Logic
        let duration = 60 * 15 - 1; 
        const countdownElement = document.getElementById('countdown');
        setInterval(() => {
            let minutes = parseInt(duration / 60, 10);
            let seconds = parseInt(duration % 60, 10);
            minutes = minutes < 10 ? "0" + minutes : minutes;
            seconds = seconds < 10 ? "0" + seconds : seconds;
            countdownElement.textContent = minutes + ":" + seconds;
            if (--duration < 0) duration = 60 * 15 - 1; 
        }, 1000);

        function switchTab(tab) {
            document.getElementById('panel-design').classList.toggle('hidden', tab !== 'design');
            document.getElementById('panel-quote').classList.toggle('hidden', tab !== 'quote');
            
            const btnDesign = document.getElementById('tab-design');
            const btnQuote = document.getElementById('tab-quote');
            
            if (tab === 'design') {
                btnDesign.className = "w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl bg-amber-500 text-slate-950 shadow-lg transition-all flex items-center justify-center gap-2";
                btnQuote.className = "w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl text-slate-400 hover:text-white transition-all flex items-center justify-center gap-2";
            } else {
                btnQuote.className = "w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl bg-amber-500 text-slate-950 shadow-lg transition-all flex items-center justify-center gap-2";
                btnDesign.className = "w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl text-slate-400 hover:text-white transition-all flex items-center justify-center gap-2";
            }
        }

        function updateConfig() {
            const width = parseInt(document.getElementById('barnWidth').value);
            const length = parseInt(document.getElementById('barnLength').value);
            const wallColor = document.getElementById('wallColor').value;
            const roofColor = document.getElementById('roofColor').value;
            const hasWainscot = document.getElementById('addWainscot').checked;
            const hasDoor = document.getElementById('addDoor').checked;

            // Update marketing calculations UI elements
            document.getElementById('sqftDisplay').innerText = `${(width * length).toLocaleString()} Sq Ft`;

            // Core Formula Engine
            let calculatedBase = width * length * 16.5; 
            if (hasWainscot) calculatedBase += 850;
            if (hasDoor) calculatedBase += 1200;

            document.getElementById('priceDisplay').innerText = `$${Math.round(calculatedBase).toL
