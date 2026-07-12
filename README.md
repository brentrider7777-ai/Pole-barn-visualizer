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
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: #0f172a; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #334155; border-radius: 10px; }
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
        
        <!-- Left Side: Dynamic Image Visualizer Arena -->
        <div class="lg:col-span-7 space-y-4">
            <div class="bg-slate-900/50 border border-slate-800 rounded-3xl p-6 shadow-2xl relative overflow-hidden backdrop-blur-sm">
                <div class="absolute -top-24 -left-24 w-48 h-48 bg-amber-500/10 blur-3xl rounded-full"></div>
                
                <div class="flex justify-between items-center mb-4 relative z-10">
                    <div>
                        <span class="text-[10px] font-bold tracking-widest text-amber-500 uppercase">Interactive Render Hub</span>
                        <h2 class="text-xl font-black tracking-tight mt-0.5" id="visualizerTitle">Suburban Workshop Setup</h2>
                    </div>
                    <div class="bg-slate-800 border border-slate-700 px-3 py-1 rounded-xl text-xs font-mono text-emerald-400 font-bold flex items-center gap-1.5">
                        <i class="fa-solid fa-circle-check"></i> Live Preview
                    </div>
                </div>
                
                <!-- HD Dynamic Display Image Container -->
                <div class="bg-slate-950/80 rounded-2xl border border-slate-800/80 overflow-hidden relative shadow-inner min-h-[350px] lg:min-h-[420px] flex items-center justify-center">
                    <img id="mainBarnVisual" src="https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=800&q=80" class="w-full h-full min-h-[350px] lg:min-h-[420px] object-cover transition-all duration-500 filter brightness-90" alt="Main Barn Preview">
                    <div class="absolute inset-0 bg-gradient-to-t from-slate-950/80 via-transparent to-transparent"></div>
                    
                    <!-- Dynamic floating specification badge overlay -->
                    <div class="absolute bottom-4 left-4 bg-slate-900/90 backdrop-blur-md border border-slate-700 p-3 rounded-xl text-xs space-y-1">
                        <p class="text-slate-400">Current Model Specs:</p>
                        <p class="font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-vector-square text-amber-500"></i> <span id="overlayDimensions">30' x 40' Truss System</span>
                        </p>
                    </div>
                </div>

                <!-- Dynamic Pricing Panel -->
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

        <!-- Right Side: Control Hub & Options Panel -->
        <div class="lg:col-span-5 space-y-6">
            
            <div class="grid grid-cols-2 gap-2 bg-slate-900 p-1.5 rounded-2xl border border-slate-800">
                <button onclick="switchTab('design')" id="tab-design" class="w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl bg-amber-500 text-slate-950 shadow-lg transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-sliders"></i> 1. Customize
                </button>
                <button onclick="switchTab('quote')" id="tab-quote" class="w-full py-3 text-xs font-black tracking-wider uppercase rounded-xl text-slate-400 hover:text-white transition-all flex items-center justify-center gap-2">
                    <i class="fa-solid fa-lock"></i> 2. Claim Kit
                </button>
            </div>

            <!-- Panel 1: Configuration Form -->
            <div id="panel-design" class="bg-slate-900/60 border border-slate-800 p-6 rounded-3xl shadow-2xl space-y-6 overflow-y-auto max-h-[75vh] custom-scrollbar">
                
                <!-- NEW OPTION: Primary Structural Use Case (Changes Main Visual Image) -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">1. Select Framing Blueprint Style</label>
                    <div class="grid grid-cols-2 gap-3">
                        <div onclick="selectModel('workshop', 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=800&q=80', this)" class="model-card border-2 border-amber-500 bg-slate-950/80 p-3 rounded-xl cursor-pointer hover:border-slate-600 transition-all">
                            <p class="text-xs font-black text-white flex items-center gap-2">
                                <i class="fa-solid fa-screwdriver-wrench text-amber-500"></i> Suburban Workshop
                            </p>
                            <p class="text-[10px] text-slate-500 mt-1">Standard residential shop style rooflines</p>
                        </div>
                        <div onclick="selectModel('ag', 'https://images.unsplash.com/photo-1500937386664-56d1dfef3854?auto=format&fit=crop&w=800&q=80', this)" class="model-card border border-slate-800 bg-slate-950/80 p-3 rounded-xl cursor-pointer hover:border-slate-600 transition-all">
                            <p class="text-xs font-black text-white flex items-center gap-2">
                                <i class="fa-solid fa-tractor text-slate-400"></i> Agricultural Storage
                            </p>
                            <p class="text-[10px] text-slate-500 mt-1">High-clearance bays for farm machinery</p>
                        </div>
                    </div>
                </div>

                <!-- Footprint Dimensions Options -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3 flex items-center justify-between">
                        <span>2. System Dimensions</span>
                        <span class="text-amber-500 normal-case font-medium font-mono text-[11px]" id="sqftDisplay">1200 Sq Ft</span>
                    </label>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <select id="barnWidth" onchange="updateConfig()" class="w-full appearance-none bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white focus:outline-none focus:border-amber-500 cursor-pointer">
                                <option value="24">24 Ft Wide</option>
                                <option value="30" selected>30 Ft Wide</option>
                                <option value="40">40 Ft Wide</option>
                            </select>
                            <i class="fa-solid fa-chevron-down absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 text-xs pointer-events-none"></i>
                        </div>
                        <div class="relative">
                            <select id="barnLength" onchange="updateConfig()" class="w-full appearance-none bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm font-bold text-white focus:outline-none focus:border-amber-500 cursor-pointer">
                                <option value="30">30 Ft Long</option>
                                <option value="40" selected>40 Ft Long</option>
                                <option value="60">60 Ft Long</option>
                            </select>
                            <i class="fa-solid fa-chevron-down absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 text-xs pointer-events-none"></i>
                        </div>
                    </div>
                </div>

                <!-- Siding Selector Tiles with Real Texture Backgrounds -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">3. Heavy-Duty Steel Siding Texture</label>
                    <div class="grid grid-cols-2 gap-3">
                        <!-- Red Option -->
                        <div onclick="selectSiding('#991b1b', this)" class="siding-card border-2 border-amber-500 bg-slate-950/80 rounded-xl overflow-hidden cursor-pointer shadow-md group relative h-20">
                            <img src="https://images.unsplash.com/photo-1589939705384-5185137a7f0f?auto=format&fit=crop&w=250&q=80" class="absolute inset-0 w-full h-full object-cover opacity-35 group-hover:scale-105 transition-all" alt="Red Siding">
                            <div class="absolute inset-0 bg-gradient-to-t from-slate-950 via-slate-950/40 to-transparent p-2.5 flex flex-col justify-end">
                                <p class="text-xs font-black text-white">Crimson Red</p>
                                <p class="text-[10px] text-slate-400">29-Gauge Classic</p>
                            </div>
                        </div>
                        <!-- Charcoal Option -->
                        <div onclick="selectSiding('#1e293b', this)" class="siding-card border border-slate-800 bg-slate-950/80 rounded-xl overflow-hidden cursor-pointer shadow-md group relative h-20">
                            <img src="https://images.unsplash.com/photo-1531834685988-c388d2c019ec?auto=format&fit=crop&w=250&q=80" class="absolute inset-0 w-full h-full object-cover opacity-35 group-hover:scale-105 transition-all" alt="Charcoal Siding">
                            <div class="absolute inset-0 bg-gradient-to-t from-slate-950 via-slate-950/40 to-transparent p-2.5 flex flex-col justify-end">
                                <p class="text-xs font-black text-white">Charcoal Grey</p>
                                <p class="text-[10px] text-slate-400">Architectural Slate</p>
                            </div>
                        </div>
                    </div>
                    <input type="hidden" id="wallColor" value="#991b1b">
                </div>

                <!-- NEW OPTION: Door & Window Packages with UI Images -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">4. Fenestration & Light Package</label>
                    <div class="grid grid-cols-2 gap-3">
                        <label class="flex flex-col bg-slate-950/50 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer overflow-hidden transition-all">
                            <img src="https://images.unsplash.com/photo-1600607687939-ce8a6c25118c?auto=format&fit=crop&w=300&q=80" class="w-full h-20 object-cover opacity-60" alt="Window Panel">
                            <div class="p-3 space-y-1">
                                <div class="flex items-center gap-2">
                                    <input type="checkbox" id="addWindows" onchange="updateConfig()" class="w-3.5 h-3.5 text-amber-500 accent-amber-500">
                                    <span class="text-xs font-bold text-white">Triple Window Array</span>
                                </div>
                                <p class="text-[9px] text-slate-500">3'x4' insulated thermo-panes</p>
                                <p class="text-xs font-mono font-bold text-emerald-400 mt-1">+$1,150</p>
                            </div>
                        </label>

                        <label class="flex flex-col bg-slate-950/50 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer overflow-hidden transition-all">
                            <img src="https://images.unsplash.com/photo-1513694203232-719a280e022f?auto=format&fit=crop&w=300&q=80" class="w-full h-20 object-cover opacity-60" alt="Slider Track">
                            <div class="p-3 space-y-1">
                                <div class="flex items-center gap-2">
                                    <input type="checkbox" id="addDoor" checked onchange="updateConfig()" class="w-3.5 h-3.5 text-amber-500 accent-amber-500">
                                    <span class="text-xs font-bold text-white">12x12 Slider Door</span>
                                </div>
                                <p class="text-[9px] text-slate-500">Heavy duty dual-track rollers</p>
                                <p class="text-xs font-mono font-bold text-emerald-400 mt-1">+$1,200</p>
                            </div>
                        </label>
                    </div>
                </div>

                <!-- Foundation Anchor Configuration Options -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">5. Foundation Engineering</label>
                    <div class="grid grid-cols-2 gap-3">
                        <label class="flex items-center gap-3 bg-slate-950/50 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer">
                            <input type="radio" name="postFoundation" value="dirt" checked onchange="updateConfig()" class="w-4 h-4 text-amber-500 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-white">In-Ground Posts</p>
                                <p class="text-[10px] text-slate-500">Gravel boot footing</p>
                            </div>
                        </label>
                        <label class="flex items-center gap-3 bg-slate-950/50 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer">
                            <input type="radio" name="postFoundation" value="pier" onchange="updateConfig()" class="w-4 h-4 text-amber-500 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-white">Wet-Slip Piers</p>
                                <p class="text-[10px] text-slate-500">Perma-Column brackets (+$1,400)</p>
                            </div>
                        </label>
                    </div>
                </div>

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
                        <input type="text" required placeholder="First and Last Name" class="w-full bg-slate-950/80 border border-slate-800 rounded-xl px-4 py-3 text-sm faditional Matte</p>
                            </div>
                        </div>
                        <!-- Option 2 -->
                        <div onclick="selectSiding('#1e293b', this)" class="siding-card border border-slate-800 bg-slate-950/80 rounded-xl overflow-hidden cursor-pointer shadow-md group relative h-24">
                            <img src="https://images.unsplash.com/photo-1531834685988-c388d2c019ec?auto=format&fit=crop&w=250&q=80" class="absolute inset-0 w-full h-full object-cover opacity-40 group-hover:scale-105 transition-all" alt="Charcoal Steel">
                            <div class="absolute inset-0 bg-gradient-to-t from-slate-950 via-slate-950/40 to-transparent p-2 flex flex-col justify-end">
                                <p class="text-xs font-black text-white">Charcoal Grey</p>
                                <p class="text-[10px] text-slate-400">Modern Textures</p>
                            </div>
                        </div>
                    </div>
                    <input type="hidden" id="wallColor" value="#991b1b">
                </div>

                <!-- NEW OPTION: Engineered Pitch Roof Slopes -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">Structural Roof Pitch</label>
                    <div class="grid grid-cols-2 gap-3">
                        <label class="flex items-center gap-3 bg-slate-950/50 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer">
                            <input type="radio" name="roofPitch" value="4" checked onchange="updateConfig()" class="w-4 h-4 text-amber-500 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-white">4:12 Standard Pitch</p>
                                <p class="text-[10px] text-slate-500">Perfect for standard storage</p>
                            </div>
                        </label>
                        <label class="flex items-center gap-3 bg-slate-950/50 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer">
                            <input type="radio" name="roofPitch" value="6" onchange="updateConfig()" class="w-4 h-4 text-amber-500 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-white">6:12 High Pitch</p>
                                <p class="text-[10px] text-slate-500">Snow shedding engineered</p>
                            </div>
                        </label>
                    </div>
                </div>

                <!-- NEW OPTION: Sub-Surface Foundation Anchor Profiles -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">Foundation Engineering</label>
                    <div class="grid grid-cols-2 gap-3">
                        <label class="flex items-center gap-3 bg-slate-950/50 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer">
                            <input type="radio" name="postFoundation" value="dirt" checked onchange="updateConfig()" class="w-4 h-4 text-amber-500 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-white">In-Ground Posts</p>
                                <p class="text-[10px] text-slate-500">Embedded with gravel boot</p>
                            </div>
                        </label>
                        <label class="flex items-center gap-3 bg-slate-950/50 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer">
                            <input type="radio" name="postFoundation" value="pier" onchange="updateConfig()" class="w-4 h-4 text-amber-500 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-white">Concrete Wet-Piers</p>
                                <p class="text-[10px] text-slate-500">Perma-Column brackets (+$1,400)</p>
                            </div>
                        </label>
                    </div>
                </div>

                <!-- System Add-ons / Upsells -->
                <div>
                    <label class="block text-xs font-black uppercase tracking-widest text-slate-400 mb-3">Premium Architecture Upgrades</label>
                    <div class="space-y-2.5">
                        <label class="flex items-center justify-between bg-slate-950/50 p-3.5 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer transition-all">
                            <div class="flex items-center gap-3">
                                <input type="checkbox" id="addWainscot" onchange="updateConfig()" class="w-4 h-4 rounded bg-slate-900 border-slate-700 text-amber-500 accent-amber-500">
                                <div>
                                    <p class="text-xs font-bold text-white">Two-Tone Wainscoting</p>
                                    <p class="text-[10px] text-slate-500">Premium 3-foot color breakup</p>
                                </div>
                            </div>
                            <span class="text-xs font-mono font-bold text-emerald-400">+$850</span>
                        </label>

                        <label class="flex items-center justify-between bg-slate-950/50 p-3.5 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer transition-all">
                            <div class="flex items-center gap-3">
                                <input type="checkbox" id="addDoor" onchange="updateConfig()" checked class="w-4 h-4 rounded bg-slate-900 border-slate-700 text-amber-500 accent-amber-500">
                                <div>
                                    <p class="text-xs font-bold text-white">Double Sliding Structural Door</p>
                                    <p class="text-[10px] text-slate-500">12'x12' heavy duty structural track</p>
                                </div>
                            </div>
                            <span class="text-xs font-mono font-bold text-emerald-400">+$1,200</span>
                        </label>
                    </div>
                </div>

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
                    ← Change Dimensi                          <p class="text-xs font-bold text-white">Two-Tone Wainscoting</p>
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
