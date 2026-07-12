<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pole Barn Customizer</title>
  <style>
    /* Global Canvas Layout */
    body {
      background-color: #141517;
      background-image: 
        radial-gradient(at 50% 0%, rgba(38, 42, 49, 0.4) 0px, transparent 75%),
        linear-gradient(rgba(255, 255, 255, 0.02) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255, 255, 255, 0.02) 1px, transparent 1px);
      background-size: 100% 100%, 24px 24px, 24px 24px;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      color: #f5f5f7;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
      padding: 20px;
      box-sizing: border-box;
    }

    /* Main Visualizer App Container */
    .visualizer-container {
      background: #202226;
      border-radius: 20px;
      width: 100%;
      max-width: 480px;
      box-shadow: 
        0 0 1px 1px rgba(255, 255, 255, 0.08) inset,
        0 30px 60px rgba(0, 0, 0, 0.6);
      border: 1px solid #111214;
      overflow: hidden;
    }

    /* Application Header */
    .visualizer-header {
      padding: 24px 24px 16px 24px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
      background: linear-gradient(to bottom, rgba(255,255,255,0.02), transparent);
    }

    .badge {
      display: inline-block;
      background: rgba(255, 159, 67, 0.15);
      color: #ff9f43;
      font-size: 0.7rem;
      font-weight: 700;
      padding: 4px 8px;
      border-radius: 4px;
      letter-spacing: 1px;
      text-transform: uppercase;
      margin-bottom: 8px;
      border: 1px solid rgba(255, 159, 67, 0.2);
    }

    h1 {
      margin: 0;
      font-size: 1.5rem;
      font-weight: 700;
      letter-spacing: -0.5px;
      background: linear-gradient(180deg, #ffffff 0%, #bdc3c7 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }

    p.subtitle {
      margin: 4px 0 0 0;
      font-size: 0.85rem;
      color: #8a909c;
    }

    /* Main Render Display Box */
    .render-preview-window {
      margin: 0 24px;
      height: 160px;
      background: #17181c;
      border-radius: 12px;
      border: 1px solid rgba(0, 0, 0, 0.4);
      box-shadow: inset 0 2px 8px rgba(0,0,0,0.5);
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      overflow: hidden;
    }

    /* Image layers within the display box */
    .steel-panel-preview {
      width: 100%;
      height: 100%;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      transition: background-image 0.25s ease-in-out;
      display: flex;
      align-items: flex-end;
      justify-content: center;
    }

    .preview-overlay-text {
      width: 100%;
      background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
      padding: 10px 0;
      text-align: center;
      font-size: 0.75rem;
      font-weight: 700;
      letter-spacing: 2px;
      color: rgba(255, 255, 255, 0.6);
    }

    /* Control Panel Content Area */
    .control-panel {
      padding: 24px;
    }

    h3 {
      margin-top: 0;
      margin-bottom: 16px;
      font-size: 0.8rem;
      letter-spacing: 1px;
      color: #8a909c;
      text-transform: uppercase;
    }

    /* --- 3D OPTIONS GRID SYSTEM --- */
    .visualizer-options-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
      gap: 20px;
    }

    /* The actual 3D option card container */
    .color-option-card {
      position: relative;
      border-radius: 12px;
      cursor: pointer;
      background: #0d0e10; /* Solid dark underside depth */
      border: none;
      padding: 0;
      outline: none;
      width: 100%;
      
      /* Multi-layered shadows render the sharp physical extrusion */
      box-shadow: 
        0 1px 0px #0d0e10,
        0 2px 0px #0d0e10,
        0 3px 0px #0d0e10,
        0 4px 0px #0d0e10,
        0 5px 0px #0d0e10,
        0 8px 16px rgba(0, 0, 0, 0.5); 
        
      transform: translateY(-5px); 
      transition: transform 0.1s cubic-bezier(0.25, 1, 0.5, 1), box-shadow 0.1s cubic-bezier(0.25, 1, 0.5, 1);
      -webkit-tap-highlight-color: transparent;
    }

    /* Face layer surface of the card */
    .color-option-card .surface {
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      align-items: flex-start;
      height: 90px;
      padding: 14px;
      border-radius: 12px;
      border: 1px solid rgba(255, 255, 255, 0.1); /* Metal rim highlight */
      color: #ffffff;
      font-weight: 600;
      font-size: 0.9rem;
      text-shadow: 0 2px 4px rgba(0,0,0,0.8);
      box-sizing: border-box;
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
    }

    /* Micro-text swatches info */
    .color-option-card .surface::after {
      content: "Texture Finish";
      font-size: 0.65rem;
      font-weight: 400;
      color: rgba(255, 255, 255, 0.7);
      text-shadow: 0 1px 2px rgba(0,0,0,0.8);
      margin-top: 2px;
    }

    /* --- STATES & INTERACTIONS --- */
    .color-option-card:hover {
      transform: translateY(-6px);
    }

    /* Flatten extrusion instantly on selection or click hold */
    .color-option-card:active,
    .color-option-card.selected {
      transform: translateY(0px);
      box-shadow: 
        0 1px 0px #0d0e10,
        0 2px 4px rgba(0, 0, 0, 0.6);
    }

    /* Accent color border for active selection */
    .color-option-card.selected .surface {
      border-color: #ff9f43; 
      box-shadow: inset 0 0 0 2px rgba(255, 159, 67, 0.4);
    }
  </style>
</head>
<body>

  <div class="visualizer-container">
    
    <!-- Configurator Branding Section -->
    <div class="visualizer-header">
      <span class="badge">Live Material Builder</span>
      <h1>Configure Exterior</h1>
      <p class="subtitle">Select options below to update your building panel texture.</p>
    </div>

    <!-- Dynamic Material Render Window -->
    <div class="render-preview-window">
      <div id="materialPreview" class="steel-panel-preview">
        <div class="preview-overlay-text">3D MATERIAL VIEW</div>
      </div>
    </div>

    <!-- Controls Selector Grid -->
    <div class="control-panel">
      <h3>Metal Siding Shell</h3>
      
      <div class="visualizer-options-grid">
        
        <!-- 
          DIRECTIONS: Change the data-img attribute value below to your exact file paths 
          Example: data-img="images/coffee-panel.jpg" or data-img="assets/espresso.png"
        -->

        <!-- Coffee Block Selection -->
        <button class="color-option-card selected" 
                data-img="https://placehold.co/400x300/463025/ffffff?text=Coffee+Metal" 
                data-fallback="#463025"
                onclick="updateVisualizer(this, 'Coffee')">
          <span class="surface" style="background-image: url('https://placehold.co/150x150/463025/ffffff?text=Coffee')">Coffee</span>
        </button>

        <!-- Espresso Block Selection -->
        <button class="color-option-card" 
                data-img="https://placehold.co/400x300/2d2b2a/ffffff?text=Espresso+Metal" 
                data-fallback="#2d2b2a"
                onclick="updateVisualizer(this, 'Espresso')">
          <span class="surface" style="background-image: url('https://placehold.co/150x150/2d2b2a/ffffff?text=Espresso')">Espresso</span>
        </button>

        <!-- Charcoal Block Selection -->
        <button class="color-option-card" 
                data-img="https://placehold.co/400x300/3a3d40/ffffff?text=Charcoal+Metal" 
                data-fallback="#3a3d40"
                onclick="updateVisualizer(this, 'Charcoal')">
          <span class="surface" style="background-image: url('https://placehold.co/150x150/3a3d40/ffffff?text=Charcoal')">Charcoal</span>
        </button>

      </div>
    </div>
    
  </div>

  <script>
    function updateVisualizer(element, labelText) {
      // 1. Handle selection toggle state across cards
      const cards = element.parentElement.querySelectorAll('.color-option-card');
      cards.forEach(card => card.classList.remove('selected'));
      element.classList.add('selected');
      
      // 2. Fetch image paths and hex backups directly from element attributes
      const newImageUrl = element.getAttribute('data-img');
      const fallbackColor = element.getAttribute('data-fallback');
      
      const previewWindow = document.getElementById('materialPreview');
      
      // 3. Render image to the display panel window smoothly
      previewWindow.style.backgroundImage = `url('${newImageUrl}')`;
      previewWindow.style.backgroundColor = fallbackColor;
    }

    // Set initial layout state on load execution
    document.addEventListener("DOMContentLoaded", function() {
      const activeCard = document.querySelector('.color-option-card.selected');
      if(activeCard) {
        updateVisualizer(activeCard, 'Coffee');
      }
    });
  </script>

</body>
</html>
gap: 1rem;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            cursor: pointer;
            background-color: var(--bg-light);
            user-select: none;
        }

        .preview-canvas-wrapper {
            background: #ffffff;
            width: 100%;
            height: 440px;
            border-radius: var(--radius);
            box-shadow: inset 0 2px 8px 0 rgba(0,0,0,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
        }

        .price-display {
            width: 100%;
            background: var(--primary);
            color: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            margin-top: 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .price-value {
            font-size: 1.65rem;
            font-weight: 700;
            color: #f97316;
        }

        /* Portfolio Cards */
        .filter-bar { display: flex; gap: 0.5rem; margin-bottom: 2rem; }
        .filter-btn {
            background: var(--white); border: 1px solid var(--border);
            padding: 0.5rem 1.25rem; border-radius: 20px; cursor: pointer;
            font-weight: 600; transition: all 0.2s ease;
        }
        .filter-btn.active, .filter-btn:hover { background: var(--primary); color: var(--white); border-color: var(--primary); }
        
        .gallery-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1.5rem; }
        .gallery-card { 
            background: var(--white); 
            border-radius: var(--radius); 
            overflow: hidden; 
            box-shadow: var(--shadow); 
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .gallery-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1);
        }
        .gallery-img-container { height: 210px; background-color: #cbd5e1; display: flex; align-items: center; justify-content: center; position: relative; overflow: hidden; }
        .gallery-info { padding: 1.5rem; }
        .gallery-info h3 { margin-bottom: 0.25rem; color: var(--primary); }

        /* Consultation Onboarding Form */
        .quote-layout { max-width: 650px; margin: 0 auto; background: var(--white); padding: 3rem; border-radius: var(--radius); box-shadow: var(--shadow); }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

        /* Interactive Detail Overlays (Modal) */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(15, 23, 42, 0.6);
            backdrop-filter: blur(4px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            padding: 1.5rem;
        }
        .modal-overlay.active { display: flex; }
        .modal-card {
            background: var(--white);
            max-width: 550px;
            width: 100%;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            animation: modalScale 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        @keyframes modalScale {
            from { transform: scale(0.95); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }
        .modal-header {
            background-color: var(--primary);
            color: var(--white);
            padding: 1.25rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .modal-close {
            background: none; border: none; color: #94a3b8; font-size: 1.25rem; cursor: pointer;
        }
        .modal-close:hover { color: var(--white); }
        .modal-body { padding: 1.5rem; }
        .modal-meta-row { display: flex; justify-content: space-between; padding: 0.5rem 0; border-bottom: 1px solid var(--border); }
    </style>
</head>
<body>

    <div class="contractor-bar">
        <div><i class="fa-solid fa-microchip"></i> Powered by Kilbourne Proprietary Technology Systems</div>
        <div><i class="fa-solid fa-phone"></i> Engineering Desk: (555) 839-2041</div>
    </div>

    <header>
        <div class="nav-container">
            <div class="logo" onclick="switchTab('home')">
                <i class="fa-solid fa-shield-halved"></i> KILBOURNE <span>BUILDS</span>
            </div>
            <nav>
                <button class="tab-btn active" id="btn-home" onclick="switchTab('home')">Proprietary Standards</button>
                <button class="tab-btn" id="btn-configurator" onclick="switchTab('configurator')">Estimate Workspace</button>
                <button class="tab-btn" id="btn-gallery" onclick="switchTab('gallery')">Recent Builds</button>
                <button class="tab-btn" id="btn-quote" onclick="switchTab('quote')">Consultation</button>
            </nav>
        </div>
    </header>

    <main>
        <!-- TAB 1: SERVICE STANDARDS -->
        <section id="home" class="tab-content active">
            <div class="hero">
                <div class="hero-text">
                    <h1>Engineered Infrastructure via Proprietary Technology</h1>
                    <p>Kilbourne Builds transforms structural framing concepts into permanent assets. Utilizing our advanced, proprietary wind-load deflection tech, multi-ply column engineering, and weather-sealed interlocking panels, we construct high-performance agricultural spaces built for generations.</p>
                    <div class="cta-group">
                        <button class="btn btn-primary" onclick="switchTab('configurator')"><i class="fa-solid fa-gears"></i> Open Configurator Workspace</button>
                        <button class="btn btn-secondary" onclick="switchTab('gallery')">Explore Completed Works</button>
                    </div>
                </div>
                <div class="badge-grid">
                    <div class="badge-item">
                        <i class="fa-solid fa-microchip"></i>
                        <div>
                            <strong>Proprietary Frame Dynamics</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">Our engineered connection brackets and multi-laminated truss systems use proprietary technology blueprints to resist up to 145 MPH wind loads comfortably.</p>
                        </div>
                    </div>
                    <div class="badge-item">
                        <i class="fa-solid fa-layer-group"></i>
                        <div>
                            <strong>Advanced Siding Matrices</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">From 29-gauge high-tensile steel rollings to treated cedar lap arrays, we deploy optimized material barriers against moisture intrusion.</p>
                        </div>
                    </div>
                </div>
            </div>

            <h2 class="section-title">Our Rigorous Build Workflow</h2>
            <div class="process-flow">
                <div class="process-step">
                    <div class="step-num">01</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Site Prep & Leveling</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Excavation teams clear brush, level terrain, and grade slope configurations for concrete pad allocations.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">02</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Post Foundations</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Setting structural 3-ply laminated columns past the regional frost line with mixed base collars.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">03</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Truss Framework</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Lifting custom engineered, high wind-load roof systems safely anchored using direct heavy brackets.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">04</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Trim & Envelope</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Installing architectural siding arrays, proper ridge venting layouts, and commercial door assemblies.</p>
                </div>
            </div>
        </section>

        <!-- TAB 2: ESTIMATOR -->
        <section id="configurator" class="tab-content">
            <div class="config-container">
                <div class="config-panel">
                    <h2 style="margin-bottom: 0.5rem;">Select Build Type</h2>
                    <p style="color: var(--text-light); margin-bottom: 1.5rem; font-size: 0.9rem;">Adjust layout options to calculate structural crew and material contract metrics.</p>
                    
                    <div class="config-group">
                        <label>Structural Building Services</label>
                        <select class="select-input" id="struct-type" onchange="updateConfiguratorOptions()">
                            <option value="barn">Substantial Post-Frame Barn</option>
                            <option value="gazebo">Architectural Open-Air Gazebo</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Footprint Allocations</label>
                        <select class="select-input" id="struct-size" onchange="updateConfigurator()">
                            <!-- Generated via State Engine -->
                        </select>
                    </div>

                    <div class="config-group" id="height-container">
                        <label>Interior Eave Ceiling Clearance</label>
                        <select class="select-input" id="struct-height" onchange="updateConfigurator()">
                            <option value="10">10 Foot Eave Clearance</option>
                            <option value="12" selected>12 Foot Eave Clearance</option>
                            <option value="14">14 Foot Eave Clearance</option>
                            <option value="16">16 Foot Eave Clearance</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Siding Profile Material</label>
                        <select class="select-input" id="siding-profile" onchange="updateConfigurator()">
                            <option value="steel">29-Gauge Ribbed Steel Panels</option>
                            <option value="lap">Premium Cedar Horizontal Lap Board</option>
                            <option value="board">Traditional Board & Batten Timber</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Wall Panel Siding Tones</label>
                        <div class="swatch-grid" id="wall-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Roof Panel Sheeting Tones</label>
                        <div class="swatch-grid" id="roof-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Contract Add-ons & Turn-key Additions</label>
                        <div class="checkbox-tile-group" id="upgrades-container"></div>
                    </div>
                </div>

                <div class="config-preview-panel">
                    <div class="preview-canvas-wrapper">
                        <!-- Advanced Interactive Dynamic Vector Blueprint Engine -->
                        <svg id="preview-svg" width="380" height="380" viewBox="0 0 380 380" style="background: transparent;">
                            <defs>
                                <!-- Steel Profile Pattern Leaf -->
                                <pattern id="pat-steel" width="12" height="20" patternUnits="userSpaceOnUse">
                                    <rect width="6" height="20" fill="rgba(0,0,0,0.06)"/>
                                    <rect x="6" width="6" height="20" fill="rgba(255,255,255,0.06)"/>
                                    <line x1="0" y1="0" x2="0" y2="20" stroke="rgba(0,0,0,0.15)" stroke-width="1"/>
                                    <line x1="6" y1="0" x2="6" y2="20" stroke="rgba(255,255,255,0.1)" stroke-width="1"/>
                                </pattern>
                                <!-- Lap Siding Profile Pattern Leaf -->
                                <pattern id="pat-lap" width="40" height="12" patternUnits="userSpaceOnUse">
                                    <line x1="0" y1="11" x2="40" y2="11" stroke="rgba(0,0,0,0.2)" stroke-width="1.5"/>
                                    <line x1="0" y1="12" x2="40" y2="12" stroke="rgba(255,255,255,0.15)" stroke-width="1"/>
                                </pattern>
                                <!-- Board & Batten Profile Pattern Leaf -->
                                <pattern id="pat-board" width="24" height="40" patternUnits="userSpaceOnUse">
                                    <rect x="0" y="0" width="4" height="40" fill="rgba(0,0,0,0.15)"/>
                                    <line x1="2" y1="0" x2="2" y2="40" stroke="rgba(255,255,255,0.1)" stroke-width="1"/>
                                </pattern>
                            </defs>
                            <g id="svg-render-group"></g>
                        </svg>
                    </div>

                    <div class="price-display">
                        <div>
                            <p style="font-size: 0.85rem; color: #94a3b8; font-weight: 600; text-transform: uppercase;">Turn-Key Range (Est)</p>
                            <span class="price-value" id="live-price">$0.00</span>
                        </div>
                        <button class="btn btn-primary" onclick="proceedToQuote()"><i class="fa-solid fa-calendar-check"></i> Consult On Layout</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: PORTFOLIO -->
        <section id="gallery" class="tab-content">
            <h2 style="margin-bottom: 0.5rem;">Completed Service Portfolio</h2>
            <p style="color: var(--text-light); margin-bottom: 2rem;">Real job sites engineered via proprietary technology built over the last 12 months.</p>
            
            <div class="filter-bar">
                <button class="filter-btn active" id="f-all" onclick="filterGallery('all')">All Sites</button>
                <button class="filter-btn" id="f-barn" onclick="filterGallery('barn')">Post-Frame Barns</button>
                <button class="filter-btn" id="f-gazebo" onclick="filterGallery('gazebo')">Gazebos & Pavilions</button>
            </div>

            <div class="gallery-grid" id="gallery-wrapper"></div>
        </section>

        <!-- TAB 4: CONSULTATION REQUEST -->
        <section id="quote" class="tab-content">
            <div class="quote-layout">
                <h2 style="margin-bottom: 0.5rem;">Schedule Site Consultation</h2>
                <p style="color: var(--text-light); margin-bottom: 2rem;">Confirm your parameters below to route data directly to our operational estimation foreman.</p>
                
                <form id="quote-form" onsubmit="handleFormSubmit(event)">
                    <div class="config-group">
                        <label>Selected Estimator Layout Scope</label>
                        <textarea class="text-input" id="quote-summary" rows="4" readonly style="resize: none; background-color: #f1f5f9; font-size: 0.9rem;"></textarea>
                    </div>

                    <div class="form-row">
                        <div class="config-group">
                            <label>Full Contact Name</label>
                            <input type="text" class="text-input" id="client-name" required placeholder="Marcus Vance">
                        </div>
                        <div class="config-group">
                            <label>Direct Phone Number</label>
                            <input type="tel" class="text-input" id="client-phone" required placeholder="(555) 019-2831">
                        </div>
                    </div>

                    <div class="form-row">
                        <div class="config-group">
                            <label>Target Site ZIP Code</label>
                            <input type="text" class="text-input" id="client-zip" required placeholder="48827">
                        </div>
                        <div class="config-group">
                            <label>Current Permit/Zoning Phase</label>
                            <select class="select-input" id="client-zoning">
                                <option>Researching zoning rules</option>
                                <option>Permit documents ready</option>
                                <option>Need builder to file permits</option>
                            </select>
                        </div>
                    </div>

                    <div class="config-group">
                        <label>Site Terrain Status</label>
                        <select class="select-input" id="client-terrain">
                            <option>Perfectly level / Clear of trees</option>
                            <option>Minor clearing or slope corrections needed</option>
                            <option>Requires major excavation & tree clearing</option>
                        </select>
                    </div>

                    <button type="submit" class="btn btn-primary" style="width: 100%; justify-content: center; margin-top: 1rem;">
                        <i class="fa-solid fa-file-invoice"></i> Book Project Analysis
                    </button>
                </form>
            </div>
        </section>
    </main>

    <!-- PORTFOLIO PREVIEW MODAL FRAME -->
    <div class="modal-overlay" id="portfolio-modal" onclick="closeModal(event)">
        <div class="modal-card">
            <div class="modal-header">
                <h3 id="modal-title">Project Details</h3>
                <button class="modal-close" onclick="document.getElementById('portfolio-modal').classList.remove('active')"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="modal-body">
                <div id="modal-specs"></div>
                <div style="margin-top: 1.5rem; display: flex; gap: 1rem;">
                    <button class="btn btn-primary" id="modal-load-btn" style="flex: 1;">Load This Framework in Estimator</button>
                    <button class="btn btn-secondary" onclick="document.getElementById('portfolio-modal').classList.remove('active')">Close</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const colorPalette = [
            { name: 'Slate Gray', hex: '#64748b' },
            { name: 'Barn Red', hex: '#991b1b' },
            { name: 'Forest Green', hex: '#166534' },
            { name: 'Charcoal Metal', hex: '#334155' },
            { name: 'Desert Sand', hex: '#d97706' },
            { name: 'Polar White', hex: '#f8fafc' }
        ];

        const portfolioItems = [
            { id: 0, type: 'barn', title: '40x60 Insulated Commercial Workshop', locale: 'Eaton Rapids, MI', sizeIdx: 2, wallColor: '#334155', roofColor: '#f8fafc', height: '14', profile: 'steel' },
            { id: 1, type: 'barn', title: '30x40 Agricultural Equipment Shed', locale: 'Lansing, MI', sizeIdx: 1, wallColor: '#991b1b', roofColor: '#334155', height: '12', profile: 'steel' },
            { id: 2, type: 'gazebo', title: '16ft Premium Cedar Pavilion Outfit', locale: 'Grand Rapids, MI', sizeIdx: 2, wallColor: '#d97706', roofColor: '#166534', height: '10', profile: 'lap' },
            { id: 3, type: 'barn', title: '60x120 High-Volume Equestrian Arena', locale: 'Charlotte, MI', sizeIdx: 5, wallColor: '#f8fafc', roofColor: '#64748b', height: '16', profile: 'board' },
            { id: 4, type: 'gazebo', title: '12x24 Screened Open-Air Outpost', locale: 'Mason, MI', sizeIdx: 0, wallColor: '#64748b', roofColor: '#334155', height: '10', profile: 'board' },
            { id: 5, type: 'barn', title: '50x80 Mega-Fleet Storage Warehouse', locale: 'Eaton Rapids, MI', sizeIdx: 4, wallColor: '#166534', roofColor: '#f8fafc', height: '16', profile: 'steel' }
        ];

        let state = {
            activeType: 'barn',
            wallColor: '#64748b',
            roofColor: '#334155',
            selectedUpgrades: []
        };

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
            
            document.getElementById(tabId).classList.add('active');
            document.getElementById(`btn-${tabId}`).classList.add('active');
            window.scrollTo(0,0);
        }

        window.onload = function() {
            renderSwatches();
            updateConfiguratorOptions();
            filterGallery('all');
        };

        function updateConfiguratorOptions() {
            const typeSelect = document.getElementById('struct-type').value;
            state.activeType = typeSelect;

            const sizeSelect = document.getElementById('struct-size');
            sizeSelect.innerHTML = '';

            const heightContainer = document.getElementById('height-container');
            const upgradesContainer = document.getElementById('upgrades-container');

            if (typeSelect === 'barn') {
                heightContainer.style.display = 'block';
                const sizes = [
                    { label: '24 x 32 Compact Suburban (Workshop Base)', base: 22500 },
                    { label: '30 x 40 Commercial Footprint (Heavy Storage)', base: 31000 },
                    { label: '40 x 60 Agricultural Blueprint (Dual Fleet)', base: 49500 },
                    { label: '40 x 80 Transport Extended Array (Deep Storage)', base: 62000 },
                    { label: '50 x 80 Commercial Clearance Wing (Industrial Base)', base: 74000 },
                    { label: '60 x 120 Massive Clear-Span Arena (Premium Utility)', base: 108000 }
                ];
                sizes.forEach(s => sizeSelect.options[sizeSelect.options.length] = new Option(s.label, s.base));

                upgradesContainer.innerHTML = `
                    <label class="checkbox-tile"><input type="checkbox" id="upg-0" value="6500" onchange="toggleUpgrade(this, 'Pour 4-Inch Concrete Floor')"> Pour 4" Reinforced Concrete Floor (+$6,500)</label>
                    <label class="checkbox-tile"><input type="checkbox" id="upg-1" value="5200" onchange="toggleUpgrade(this, '12-Foot Side Lean-To Porch')"> 12ft Integrated Side Lean-To Porch (+$5,200)</label>
                    <label class="checkbox-tile"><input type="checkbox" id="upg-2" value="3800" onchange="toggleUpgrade(this, 'Double Commercial Roller Bay Doors')"> Dual Premium Insulated Overhead Bay Doors (+$3,800)</label>
                `;
            } else {
                heightContainer.style.display = 'none';
                const sizes = [
                    { label: '12 Foot Heavy-Frame Octagonal Traditional', base: 6900 },
                    { label: '14 Foot Heavy-Frame Octagonal Traditional', base: 8400 },
                    { label: '16 x 24 Foot Structural Gable Pavilion', base: 12200 }
                ];
                sizes.forEach(s => sizeSelect.options[sizeSelect.options.length] = new Option(s.label, s.base));

                upgradesContainer.innerHTML = `
                    <label class="checkbox-tile"><input type="checkbox" id="upg-0" value="1500" onchange="toggleUpgrade(this, 'High-Weave Inset Screening Frame')"> Heavy Duty Perimeter Mesh Screen System (+$1,500)</label>
                    <label class="checkbox-tile"><input type="checkbox" id="upg-1" value="1100" onchange="toggleUpgrade(this, 'Concealed Electrical Package')"> Code-Approved Hidden Electrical Box Package (+$1,100)</label>
                `;
            }
            state.selectedUpgrades = [];
            updateConfigurator();
        }

        function renderSwatches() {
            const wallDiv = document.getElementById('wall-swatches');
            const roofDiv = document.getElementById('roof-swatches');
            wallDiv.innerHTML = ''; roofDiv.innerHTML = '';
            
            colorPalette.forEach((color) => {
                wallDiv.innerHTML += `<div class="swatch" data-hex="${color.hex}" style="background-color: ${color.hex}" onclick="selectSwatch(this, 'wall', '${color.hex}')"></div>`;
                roofDiv.innerHTML += `<div class="swatch" data-hex="${color.hex}" style="background-color: ${color.hex}" onclick="selectSwatch(this, 'roof', '${color.hex}')"></div>`;
            });
            
            wallDiv.children[0].classList.add('selected');
            roofDiv.children[3].classList.add('selected');
        }

        function selectSwatch(element, target, hex) {
            element.parentElement.querySelectorAll('.swatch').forEach(s => s.classList.remove('selected'));
            element.classList.add('selected');
            if(target === 'wall') state.wallColor = hex;
            if(target === 'roof') state.roofColor = hex;
            updateConfigurator();
        }

        function toggleUpgrade(cb, name) {
            const val = parseFloat(cb.value);
            if(cb.checked) {
                state.selectedUpgrades.push({ name: name, cost: val });
            } else {
                state.selectedUpgrades = state.selectedUpgrades.filter(u => u.name !== name);
            }
            updateConfigurator();
        }

        // High-fidelity graphic visualization matrix that renders materials and dimensions dynamically
        function updateConfigurator() {
            const baseValue = parseFloat(document.getElementById('struct-size').value) || 0;
            const heightInput = document.getElementById('struct-height') ? parseInt(document.getElementById('struct-height').value) : 12;
            const heightMultiplier = state.activeType === 'barn' ? (heightInput - 12) * 1250 : 0;
            
            const sidingType = document.getElementById('siding-profile').value;
            let sidingSurcharge = 0;
            if (sidingType === 'lap') sidingSurcharge = baseValue * 0.18;
            if (sidingType === 'board') sidingSurcharge = baseValue * 0.10;

            const upgradesTotal = state.selectedUpgrades.reduce((sum, item) => sum + item.cost, 0);
            const totalProjectCost = baseValue + heightMultiplier + sidingSurcharge + upgradesTotal;

            const variantMin = totalProjectCost * 0.95;
            const variantMax = totalProjectCost * 1.05;
            const formatCurr = (v) => new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(v);
            
            document.getElementById('live-price').innerText = `${formatCurr(variantMin)} - ${formatCurr(variantMax)}`;
            
            const renderGroup = document.getElementById('svg-render-group');
            const sizeIndex = document.getElementById('struct-size').selectedIndex || 0;

            // Dynamically adjust drawing dimensions to visually reflect option scale choices
            let scaleWidth = 220 + (sizeIndex * 15);
            let scaleHeight = 110 + (state.activeType === 'barn' ? (heightInput - 12) * 8 : 0);
            
            let startX = (380 - scaleWidth) / 2;
            let startY = 320 - scaleHeight;

            if (state.activeType === 'barn') {
                const hasLeanTo = state.selectedUpgrades.some(u => u.name.includes('Lean-To'));
                const hasDualDoors = state.selectedUpgrades.some(u => u.name.includes('Bay Doors'));

                renderGroup.innerHTML = `
                    <!-- Sky Background Element Inside Canvas Frame -->
                    <rect x="10" y="10" width="360" height="360" fill="#f8fafc" rx="6" />
                    <line x1="10" y1="320" x2="370" y2="320" stroke="#94a3b8" stroke-width="4" />

                    <!-- Lean-To Underlayment Array option visual -->
                    ${hasLeanTo ? `
                        <polygon points="${startX - 40},320 ${startX},${startY + 40} ${startX},320" fill="#cbd5e1" stroke="#475569" stroke-width="2"/>
                        <polygon points="${startX - 45},320 ${startX},${startY + 35} ${startX},${startY + 40}" fill="${state.roofColor}" stroke="#0f172a" stroke-width="1.5"/>
                    ` : ''}

                    <!-- Core Outer Structural Enclosure -->
                    <rect x="${startX}" y="${startY}" width="${scaleWidth}" height="${scaleHeight}" fill="${state.wallColor}" stroke="#0f172a" stroke-width="2.5"/>
                    
                    <!-- Dynamic Texture Layer for Selected Siding Material -->
                    <rect x="${startX}" y="${startY}" width="${scaleWidth}" height="${scaleHeight}" fill="url(#pat-${sidingType})" pointer-events="none"/>
                    
                    <!-- Structural Engineering Double Truss Roof Cap -->
                    <polygon points="${startX - 15},${startY} ${startX + (scaleWidth/2)},${startY - 65} ${startX + scaleWidth + 15},${startY}" fill="${state.roofColor}" stroke="#0f172a" stroke-width="2.5"/>
                    <polygon points="${startX - 15},${startY} ${startX + (scaleWidth/2)},${startY - 65} ${startX + scaleWidth + 15},${startY}" fill="url(#pat-steel)" opacity="0.35" pointer-events="none"/>
                    
                    <!-- Trim Eave Presentation Layer -->
                    <line x1="${startX}" y1="${startY}" x2="${startX + scaleWidth}" y2="${startY}" stroke="#0f172a" stroke-width="3" />

                    <!-- Dynamic Entry Bay Configurations -->
                    ${hasDualDoors ? `
                        <!-- Left Specialized Bay Door -->
                        <rect x="${startX + (scaleWidth * 0.15)}" y="220" width="${scaleWidth * 0.3}" height="100" fill="#e2e8f0" stroke="#334155" stroke-width="2"/>
                        <path d="M ${startX + (scaleWidth * 0.15)} 245 L ${startX + (scaleWidth * 0.45)} 245 M ${startX + (scaleWidth * 0.15)} 270 L ${startX + (scaleWidth * 0.45)} 270 M ${startX + (scaleWidth * 0.15)} 295 L ${startX + (scaleWidth * 0.45)} 295" stroke="#94a3b8" stroke-width="1"/>
                        <!-- Right Specialized Bay Door -->
                        <rect x="${startX + (scaleWidth * 0.55)}" y="220" width="${scaleWidth * 0.3}" height="100" fill="#e2e8f0" stroke="#334155" stroke-width="2"/>
                        <path d="M ${startX + (scaleWidth * 0.55)} 245 L ${startX + (scaleWidth * 0.85)} 245 M ${startX + (scaleWidth * 0.55)} 270 L ${startX + (scaleWidth * 0.85)} 270 M ${startX + (scaleWidth * 0.55)} 295 L ${startX + (scaleWidth * 0.85)} 295" stroke="#94a3b8" stroke-width="1"/>
                    ` : `
                        <!-- Centered Single Standard Entry Option -->
                        <rect x="${startX + (scaleWidth/2) - 45}" y="220" width="90" height="100" fill="#cbd5e1" stroke="#334155" stroke-width="2"/>
                        <path d="M ${startX + (scaleWidth/2) - 45} 245 L ${startX + (scaleWidth/2) + 45} 245 M ${startX + (scaleWidth/2) - 45} 270 L ${startX + (scaleWidth/2) + 45} 270 M ${startX + (scaleWidth/2) - 45} 295 L ${startX + (scaleWidth/2) + 45} 295" stroke="#94a3b8" stroke-width="1"/>
                    `}
                `;
            } else {
                const hasScreen = state.selectedUpgrades.some(u => u.name.includes('Screening'));
                
                renderGroup.innerHTML = `
                    <rect x="10" y="10" width="360" height="360" fill="#f8fafc" rx="6" />
                    <line x1="10" y1="320" x2="370" y2="320" stroke="#94a3b8" stroke-width="4" />

                    <!-- Base Platform Support Slab -->
                    <rect x="${startX - 10}" y="305" width="${scaleWidth + 20}" height="15" fill="#cbd5e1" stroke="#64748b" stroke-width="1.5"/>

                    <!-- Screening Wireframe Visual Modifier option -->
                    ${hasScreen ? `
                        <rect x="${startX}" y="${startY}" width="${scaleWidth}" height="195" fill="rgba(51,65,85,0.15)" stroke="none"/>
                        <pattern id="pat-mesh" width="6" height="6" patternUnits="userSpaceOnUse">
                            <path d="M 6 0 L 0 6 M 0 0 L 6 6" stroke="rgba(15,23,42,0.12)" stroke-width="0.75"/>
                        </pattern>
                        <rect x="${startX}" y="${startY}" width="${scaleWidth}" height="195" fill="url(#pat-mesh)"/>
                    ` : ''}

                    <!-- Timber Corner Posts and Material Texture Base Wraps -->
                    <rect x="${startX}" y="${startY}" width="${scaleWidth}" height="195" fill="none" stroke="${state.wallColor}" stroke-width="8"/>
                    <rect x="${startX}" y="${startY}" width="${scaleWidth}" height="195" fill="url(#pat-${sidingType})" opacity="0.3" pointer-events="none"/>

                    <!-- Internal Structural Balusters -->
                    <line x1="${startX + (scaleWidth * 0.25)}" y1="${startY}" x2="${startX + (scaleWidth * 0.25)}" y2="305" stroke="#475569" stroke-width="4"/>
                    <line x1="${startX + (scaleWidth * 0.5)}" y1="${startY}" x2="${startX + (scaleWidth * 0.5)}" y2="305" stroke="#475569" stroke-width="4"/>
                    <line x1="${startX + (scaleWidth * 0.75)}" y1="${startY}" x2="${startX + (scaleWidth * 0.75)}" y2="305" stroke="#475569" stroke-width="4"/>

                    <!-- Architectural Gable Structural Roof Crown -->
                    <polygon points="${startX - 20},${startY} ${startX + (scaleWidth/2)},${startY - 70} ${startX + scaleWidth + 20},${startY}" fill="${state.roofColor}" stroke="#0f172a" stroke-width="2.5"/>
                    <polygon points="${startX - 20},${startY} ${startX + (scaleWidth/2)},${startY - 70} ${startX + scaleWidth + 20},${startY}" fill="url(#pat-steel)" opacity="0.35" pointer-events="none"/>
                `;
            }
        }

        function filterGallery(category) {
            const container = document.getElementById('gallery-wrapper');
            container.innerHTML = '';
            
            document.querySelectorAll('.filter-bar').forEach(btn => btn.classList.remove('active'));
            document.getElementById(`f-${category}`).classList.add('active');

            const items = category === 'all' ? portfolioItems : portfolioItems.filter(i => i.type === category);
            
            items.forEach(item => {
                container.innerHTML += `
                    <div class="gallery-card" onclick="openPortfolioItem(${item.id})">
                        <div class="gallery-img-container">
                            <svg width="100%" height="100%" viewBox="0 0 200 140" style="background:#f1f5f9;">
                                <rect x="40" y="60" width="120" height="60" fill="${item.wallColor}"/>
                                <rect x="40" y="60" width="120" height="60" fill="url(#pat-${item.profile})" opacity="0.4"/>
                                <polygon points="30,60 100,20 170,60" fill="${item.roofColor}"/>
                                <polygon points="30,60 100,20 170,60" fill="url(#pat-steel)" opacity="0.2"/>
                            </svg>
                        </div>
                        <div class="gallery-info">
                            <h3>${item.title}</h3>
                            <p style="color: var(--text-light); font-size:0.9rem;"><i class="fa-solid fa-location-dot"></i> Region: ${item.locale}</p>
                            <span style="display:inline-block; margin-top:0.5rem; font-size:0.8rem; font-weight:700; color:var(--accent)">VIEW PROPRIETARY BLUEPRINTS &rarr;</span>
                        </div>
                    </div>
                `;
            });
        }

        function openPortfolioItem(id) {
            const item = portfolioItems.find(p => p.id === id);
            if (!item) return;

            document.getElementById('modal-title').innerText = item.title;
            
            const matchWall = colorPalette.find(c => c.hex === item.wallColor)?.name || 'Custom';
            const matchRoof = colorPalette.find(c => c.hex === item.roofColor)?.name || 'Custom';
            
            let profileLabel = '29-Gauge Ribbed Steel';
            if (item.profile === 'lap') profileLabel = 'Premium Cedar Horizontal Lap';
            if (item.profile === 'board') profileLabel = 'Traditional Board & Batten';

            document.getElementById('modal-specs').innerHTML = `
                <div class="modal-meta-row"><strong>Structure Profile:</strong> <span>${item.type === 'barn' ? 'Post-Frame Barn Array' : 'Open-Air Timber Pavilion'}</span></div>
                <div class="modal-meta-row"><strong>Technology System:</strong> <span>Kilbourne Proprietary Technology Framework</span></div>
                <div class="modal-meta-row"><strong>Location Boundary:</strong> <span>${item.locale}</span></div>
                <div class="modal-meta-row"><strong>Siding Profile Material:</strong> <span>${profileLabel}</span></div>
                <div class="modal-meta-row"><strong>Exterior Siding Tone:</strong> <span>${matchWall}</span></div>
                <div class="modal-meta-row"><strong>Roof System Coating:</strong> <span>${matchRoof}</span></div>
                ${item.type === 'barn' ? `<div class="modal-meta-row"><strong>Ceiling Clearance:</strong> <span>${item.height} Foot Eaves</span></div>` : ''}
            `;

            const loadBtn = document.getElementById('modal-load-btn');
            loadBtn.onclick = function() {
                loadFrameworkIntoWorkspace(item);
            };

            document.getElementById('portfolio-modal').classList.add('active');
        }

        function loadFrameworkIntoWorkspace(item) {
            document.getElementById('struct-type').value = item.type;
            updateConfiguratorOptions();
            
            document.getElementById('struct-size').selectedIndex = item.sizeIdx;
            document.getElementById('siding-profile').value = item.profile;
            
            if (item.type === 'barn') {
                document.getElementById('struct-height').value = item.height;
            }

            state.wallColor = item.wallColor;
            state.roofColor = item.roofColor;

            document.querySelectorAll('#wall-swatches .swatch').forEach(s => s.classList.toggle('selected', s.getAttribute('data-hex') === item.wallColor));
            document.querySelectorAll('#roof-swatches .swatch').forEach(s => s.classList.toggle('selected', s.getAttribute('data-hex') === item.roofColor));

            updateConfigurator();
            document.getElementById('portfolio-modal').classList.remove('active');
            switchTab('configurator');
        }

        function closeModal(e) {
            if (e.target.id === 'portfolio-modal') {
                document.getElementById('portfolio-modal').classList.remove('active');
            }
        }

        function proceedToQuote() {
            const sizeSelect = document.getElementById('struct-size');
            const sizeText = sizeSelect.options[sizeSelect.selectedIndex]?.text || '';
            const profileSelect = document.getElementById('siding-profile');
            const profileText = profileSelect.options[profileSelect.selectedIndex]?.text || '';
            
            let summary = `Project System Scope: Custom Kilbourne Builds Post-Frame Utility\nProprietary Engineering: Wind-Deflecting Connection Matrix\nSiding Profile Selected: ${profileText}\nPlanned Matrix Footprint: ${sizeText}\nEst Color Profiles: Walls(${state.wallColor}) / Sheet Roof System(${state.roofColor})`;
            
            if (state.activeType === 'barn') {
                summary += `\nEave Wall Height: ${document.getElementById('struct-height').value} FT`;
            }
            if (state.selectedUpgrades.length > 0) {
                summary += `\nIncluded Add-ons: ${state.selectedUpgrades.map(u => u.name).join(', ')}`;
            }
            
            document.getElementById('quote-summary').value = summary;
            switchTab('quote');
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('client-name').value;
            alert(`Thank you, ${name}! Your Kilbourne Builds structural configuration profile has been securely compiled using our proprietary technology workspace. An expert construction analyst will reach out shortly.`);
            document.getElementById('quote-form').reset();
            switchTab('home');
        }
    </script>
</body>
</html>
;
        }

        body {
            background-color: var(--bg-light);
            color: var(--text-main);
            line-height: 1.6;
        }

        .contractor-bar {
            background-color: #1e293b;
            color: #94a3b8;
            font-size: 0.85rem;
            padding: 0.5rem 2rem;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid #334155;
        }

        header {
            background-color: var(--primary);
            color: var(--white);
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: var(--shadow);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 2rem;
        }

        .logo {
            font-size: 1.4rem;
            font-weight: 700;
            letter-spacing: -0.5px;
            cursor: pointer;
        }

        .logo span {
            color: #f97316;
        }

        nav {
            display: flex;
            gap: 0.75rem;
        }

        .tab-btn {
            background: none;
            border: none;
            color: #94a3b8;
            padding: 0.6rem 1.2rem;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            border-radius: var(--radius);
        }

        .tab-btn:hover {
            color: var(--white);
            background-color: rgba(255,255,255,0.05);
        }

        .tab-btn.active {
            color: var(--white);
            background-color: var(--accent);
        }

        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 2rem;
            min-height: 75vh;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(4px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Home Tab Content */
        .hero {
            display: grid;
            grid-template-columns: 1.2fr 0.8fr;
            gap: 3rem;
            align-items: center;
            padding: 2.5rem 0;
        }

        .hero-text h1 {
            font-size: 2.75rem;
            color: var(--primary);
            line-height: 1.15;
            margin-bottom: 1.25rem;
        }

        .hero-text p {
            font-size: 1.1rem;
            color: var(--text-light);
            margin-bottom: 2rem;
        }

        .cta-group {
            display: flex;
            gap: 1rem;
        }

        .btn {
            padding: 0.75rem 1.5rem;
            font-size: 1rem;
            font-weight: 600;
            border-radius: var(--radius);
            cursor: pointer;
            transition: all 0.2s ease;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }

        .btn-primary { background-color: var(--accent); color: var(--white); border: none; }
        .btn-primary:hover { background-color: var(--accent-hover); }
        .btn-secondary { background-color: transparent; color: var(--primary); border: 2px solid var(--primary); }
        .btn-secondary:hover { background-color: var(--primary); color: var(--white); }

        .badge-grid {
            background: #f1f5f9;
            padding: 2rem;
            border-radius: var(--radius);
            display: flex;
            flex-direction: column;
            gap: 1.25rem;
            border-left: 4px solid var(--accent);
        }

        .badge-item {
            display: flex;
            gap: 1rem;
            align-items: flex-start;
        }

        .badge-item i {
            color: var(--accent);
            font-size: 1.25rem;
            margin-top: 0.2rem;
        }

        .section-title {
            font-size: 1.75rem;
            color: var(--primary);
            margin: 3rem 0 1.5rem 0;
            text-align: center;
        }

        .process-flow {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1.5rem;
            margin-bottom: 4rem;
        }

        .process-step {
            background: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
        }

        .step-num {
            position: absolute;
            top: -15px;
            right: 15px;
            font-size: 2.5rem;
            font-weight: 800;
            color: #e2e8f0;
            line-height: 1;
        }

        /* Design Configurator */
        .config-container {
            display: grid;
            grid-template-columns: 460px 1fr;
            gap: 2rem;
            background: var(--white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
        }

        .config-panel {
            padding: 2rem;
            border-right: 1px solid var(--border);
            max-height: 820px;
            overflow-y: auto;
        }

        .config-preview-panel {
            background: #f1f5f9;
            padding: 2rem;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
        }

        .config-group {
            margin-bottom: 1.5rem;
        }

        .config-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 0.5rem;
            font-size: 0.95rem;
        }

        .select-input, .text-input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            background-color: var(--bg-light);
            font-size: 1rem;
        }

        .swatch-grid {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .swatch {
            width: 34px;
            height: 34px;
            border-radius: 4px;
            border: 2px solid var(--white);
            outline: 1px solid var(--border);
            cursor: pointer;
        }

        .swatch.selected { outline: 2px solid var(--accent); }

        .checkbox-tile-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .checkbox-tile {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            cursor: pointer;
            background-color: var(--bg-light);
            user-select: none;
        }

        .preview-canvas-wrapper {
            background: var(--white);
            width: 100%;
            height: 440px;
            border-radius: var(--radius);
            box-shadow: inset 0 2px 4px 0 rgba(0,0,0,0.05);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .price-display {
            width: 100%;
            background: var(--primary);
            color: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            margin-top: 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .price-value {
            font-size: 1.65rem;
            font-weight: 700;
            color: #f97316;
        }

        /* Portfolio Cards */
        .filter-bar { display: flex; gap: 0.5rem; margin-bottom: 2rem; }
        .filter-btn {
            background: var(--white); border: 1px solid var(--border);
            padding: 0.5rem 1.25rem; border-radius: 20px; cursor: pointer;
            font-weight: 600; transition: all 0.2s ease;
        }
        .filter-btn.active, .filter-btn:hover { background: var(--primary); color: var(--white); border-color: var(--primary); }
        
        .gallery-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1.5rem; }
        .gallery-card { 
            background: var(--white); 
            border-radius: var(--radius); 
            overflow: hidden; 
            box-shadow: var(--shadow); 
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .gallery-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1);
        }
        .gallery-img-container { height: 210px; background-color: #cbd5e1; display: flex; align-items: center; justify-content: center; position: relative; overflow: hidden; }
        .gallery-info { padding: 1.5rem; }
        .gallery-info h3 { margin-bottom: 0.25rem; color: var(--primary); }

        /* Consultation Onboarding Form */
        .quote-layout { max-width: 650px; margin: 0 auto; background: var(--white); padding: 3rem; border-radius: var(--radius); box-shadow: var(--shadow); }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

        /* Interactive Detail Overlays (Modal) */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(15, 23, 42, 0.6);
            backdrop-filter: blur(4px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            padding: 1.5rem;
        }
        .modal-overlay.active { display: flex; }
        .modal-card {
            background: var(--white);
            max-width: 550px;
            width: 100%;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            animation: modalScale 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        @keyframes modalScale {
            from { transform: scale(0.95); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }
        .modal-header {
            background-color: var(--primary);
            color: var(--white);
            padding: 1.25rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .modal-close {
            background: none; border: none; color: #94a3b8; font-size: 1.25rem; cursor: pointer;
        }
        .modal-close:hover { color: var(--white); }
        .modal-body { padding: 1.5rem; }
        .modal-meta-row { display: flex; justify-content: space-between; padding: 0.5rem 0; border-bottom: 1px solid var(--border); }
        
        /* Dynamic SVG patterns for textures */
        .wood-grain-pattern { fill: #b45309; opacity: 0.15; }
    </style>
</head>
<body>

    <div class="contractor-bar">
        <div><i class="fa-solid fa-microchip"></i> Powered by Kilbourne Proprietary Technology Systems</div>
        <div><i class="fa-solid fa-phone"></i> Engineering Desk: (555) 839-2041</div>
    </div>

    <header>
        <div class="nav-container">
            <div class="logo" onclick="switchTab('home')">
                <i class="fa-solid fa-shield-halved"></i> KILBOURNE <span>BUILDS</span>
            </div>
            <nav>
                <button class="tab-btn active" id="btn-home" onclick="switchTab('home')">Proprietary Standards</button>
                <button class="tab-btn" id="btn-configurator" onclick="switchTab('configurator')">Estimate Workspace</button>
                <button class="tab-btn" id="btn-gallery" onclick="switchTab('gallery')">Recent Builds</button>
                <button class="tab-btn" id="btn-quote" onclick="switchTab('quote')">Consultation</button>
            </nav>
        </div>
    </header>

    <main>
        <!-- TAB 1: SERVICE STANDARDS -->
        <section id="home" class="tab-content active">
            <div class="hero">
                <div class="hero-text">
                    <h1>Engineered Infrastructure via Proprietary Technology</h1>
                    <p>Kilbourne Builds transforms structural framing concepts into permanent assets. Utilizing our advanced, proprietary wind-load deflection tech, multi-ply column engineering, and weather-sealed interlocking panels, we construct high-performance agricultural spaces built for generations.</p>
                    <div class="cta-group">
                        <button class="btn btn-primary" onclick="switchTab('configurator')"><i class="fa-solid fa-gears"></i> Open Configurator Workspace</button>
                        <button class="btn btn-secondary" onclick="switchTab('gallery')">Explore Completed Works</button>
                    </div>
                </div>
                <div class="badge-grid">
                    <div class="badge-item">
                        <i class="fa-solid fa-microchip"></i>
                        <div>
                            <strong>Proprietary Frame Dynamics</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">Our engineered connection brackets and multi-laminated truss systems use proprietary technology blueprints to resist up to 145 MPH wind loads comfortably.</p>
                        </div>
                    </div>
                    <div class="badge-item">
                        <i class="fa-solid fa-layer-group"></i>
                        <div>
                            <strong>Advanced Siding Matrices</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">From 29-gauge high-tensile steel rollings to treated cedar lap arrays, we deploy optimized material barriers against moisture intrusion.</p>
                        </div>
                    </div>
                </div>
            </div>

            <h2 class="section-title">Our Rigorous Build Workflow</h2>
            <div class="process-flow">
                <div class="process-step">
                    <div class="step-num">01</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Site Prep & Leveling</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Excavation teams clear brush, level terrain, and grade slope configurations for concrete pad allocations.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">02</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Post Foundations</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Setting structural 3-ply laminated columns past the regional frost line with mixed base collars.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">03</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Truss Framework</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Lifting custom engineered, high wind-load roof systems safely anchored using direct heavy brackets.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">04</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Trim & Envelope</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Installing architectural siding arrays, proper ridge venting layouts, and commercial door assemblies.</p>
                </div>
            </div>
        </section>

        <!-- TAB 2: ESTIMATOR -->
        <section id="configurator" class="tab-content">
            <div class="config-container">
                <div class="config-panel">
                    <h2 style="margin-bottom: 0.5rem;">Select Build Type</h2>
                    <p style="color: var(--text-light); margin-bottom: 1.5rem; font-size: 0.9rem;">Adjust layout options to calculate structural crew and material contract metrics.</p>
                    
                    <div class="config-group">
                        <label>Structural Building Services</label>
                        <select class="select-input" id="struct-type" onchange="updateConfiguratorOptions()">
                            <option value="barn">Substantial Post-Frame Barn</option>
                            <option value="gazebo">Architectural Open-Air Gazebo</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Footprint Allocations</label>
                        <select class="select-input" id="struct-size" onchange="updateConfigurator()">
                            <!-- Generated via State Engine -->
                        </select>
                    </div>

                    <div class="config-group" id="height-container">
                        <label>Interior Eave Ceiling Clearance</label>
                        <select class="select-input" id="struct-height" onchange="updateConfigurator()">
                            <option value="10">10 Foot Eave Clearance</option>
                            <option value="12" selected>12 Foot Eave Clearance</option>
                            <option value="14">14 Foot Eave Clearance</option>
                            <option value="16">16 Foot Eave Clearance</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Siding Profile Material</label>
                        <select class="select-input" id="siding-profile" onchange="updateConfigurator()">
                            <option value="steel">29-Gauge Ribbed Steel Panels</option>
                            <option value="lap">Premium Cedar Horizontal Lap Board</option>
                            <option value="board">Traditional Board & Batten Timber</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Wall Panel Siding Tones</label>
                        <div class="swatch-grid" id="wall-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Roof Panel Sheeting Tones</label>
                        <div class="swatch-grid" id="roof-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Contract Add-ons & Turn-key Additions</label>
                        <div class="checkbox-tile-group" id="upgrades-container"></div>
                    </div>
                </div>

                <div class="config-preview-panel">
                    <div class="preview-canvas-            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-light);
            color: var(--text-main);
            line-height: 1.6;
        }

        .contractor-bar {
            background-color: #1e293b;
            color: #94a3b8;
            font-size: 0.85rem;
            padding: 0.5rem 2rem;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid #334155;
        }

        header {
            background-color: var(--primary);
            color: var(--white);
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: var(--shadow);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 2rem;
        }

        .logo {
            font-size: 1.4rem;
            font-weight: 700;
            letter-spacing: -0.5px;
            cursor: pointer;
        }

        .logo span {
            color: #f97316;
        }

        nav {
            display: flex;
            gap: 0.75rem;
        }

        .tab-btn {
            background: none;
            border: none;
            color: #94a3b8;
            padding: 0.6rem 1.2rem;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            border-radius: var(--radius);
        }

        .tab-btn:hover {
            color: var(--white);
            background-color: rgba(255,255,255,0.05);
        }

        .tab-btn.active {
            color: var(--white);
            background-color: var(--accent);
        }

        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 2rem;
            min-height: 75vh;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(4px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Home Tab Content */
        .hero {
            display: grid;
            grid-template-columns: 1.2fr 0.8fr;
            gap: 3rem;
            align-items: center;
            padding: 2.5rem 0;
        }

        .hero-text h1 {
            font-size: 2.75rem;
            color: var(--primary);
            line-height: 1.15;
            margin-bottom: 1.25rem;
        }

        .hero-text p {
            font-size: 1.1rem;
            color: var(--text-light);
            margin-bottom: 2rem;
        }

        .cta-group {
            display: flex;
            gap: 1rem;
        }

        .btn {
            padding: 0.75rem 1.5rem;
            font-size: 1rem;
            font-weight: 600;
            border-radius: var(--radius);
            cursor: pointer;
            transition: all 0.2s ease;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }

        .btn-primary { background-color: var(--accent); color: var(--white); border: none; }
        .btn-primary:hover { background-color: var(--accent-hover); }
        .btn-secondary { background-color: transparent; color: var(--primary); border: 2px solid var(--primary); }
        .btn-secondary:hover { background-color: var(--primary); color: var(--white); }

        .badge-grid {
            background: #f1f5f9;
            padding: 2rem;
            border-radius: var(--radius);
            display: flex;
            flex-direction: column;
            gap: 1.25rem;
            border-left: 4px solid var(--accent);
        }

        .badge-item {
            display: flex;
            gap: 1rem;
            align-items: flex-start;
        }

        .badge-item i {
            color: var(--accent);
            font-size: 1.25rem;
            margin-top: 0.2rem;
        }

        .section-title {
            font-size: 1.75rem;
            color: var(--primary);
            margin: 3rem 0 1.5rem 0;
            text-align: center;
        }

        .process-flow {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1.5rem;
            margin-bottom: 4rem;
        }

        .process-step {
            background: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
        }

        .step-num {
            position: absolute;
            top: -15px;
            right: 15px;
            font-size: 2.5rem;
            font-weight: 800;
            color: #e2e8f0;
            line-height: 1;
        }

        /* Design Configurator */
        .config-container {
            display: grid;
            grid-template-columns: 440px 1fr;
            gap: 2rem;
            background: var(--white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
        }

        .config-panel {
            padding: 2rem;
            border-right: 1px solid var(--border);
            max-height: 780px;
            overflow-y: auto;
        }

        .config-preview-panel {
            background: #f1f5f9;
            padding: 2rem;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
        }

        .config-group {
            margin-bottom: 1.5rem;
        }

        .config-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 0.5rem;
            font-size: 0.95rem;
        }

        .select-input, .text-input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            background-color: var(--bg-light);
            font-size: 1rem;
        }

        .swatch-grid {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .swatch {
            width: 34px;
            height: 34px;
            border-radius: 4px;
            border: 2px solid var(--white);
            outline: 1px solid var(--border);
            cursor: pointer;
        }

        .swatch.selected { outline: 2px solid var(--accent); }

        .checkbox-tile-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .checkbox-tile {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            cursor: pointer;
            background-color: var(--bg-light);
            user-select: none;
        }

        .preview-canvas-wrapper {
            background: var(--white);
            width: 100%;
            height: 420px;
            border-radius: var(--radius);
            box-shadow: inset 0 2px 4px 0 rgba(0,0,0,0.05);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .price-display {
            width: 100%;
            background: var(--primary);
            color: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            margin-top: 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .price-value {
            font-size: 1.65rem;
            font-weight: 700;
            color: #f97316;
        }

        /* Portfolio Cards */
        .filter-bar { display: flex; gap: 0.5rem; margin-bottom: 2rem; }
        .filter-btn {
            background: var(--white); border: 1px solid var(--border);
            padding: 0.5rem 1.25rem; border-radius: 20px; cursor: pointer;
            font-weight: 600; transition: all 0.2s ease;
        }
        .filter-btn.active, .filter-btn:hover { background: var(--primary); color: var(--white); border-color: var(--primary); }
        
        .gallery-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1.5rem; }
        .gallery-card { 
            background: var(--white); 
            border-radius: var(--radius); 
            overflow: hidden; 
            box-shadow: var(--shadow); 
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .gallery-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1);
        }
        .gallery-img-placeholder { height: 210px; background-color: #cbd5e1; display: flex; align-items: center; justify-content: center; color: var(--text-light); font-size: 3rem; }
        .gallery-info { padding: 1.5rem; }
        .gallery-info h3 { margin-bottom: 0.25rem; color: var(--primary); }

        /* Consultation Onboarding Form */
        .quote-layout { max-width: 650px; margin: 0 auto; background: var(--white); padding: 3rem; border-radius: var(--radius); box-shadow: var(--shadow); }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

        /* Interactive Detail Overlays (Modal) */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(15, 23, 42, 0.6);
            backdrop-filter: blur(4px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            padding: 1.5rem;
        }
        .modal-overlay.active { display: flex; }
        .modal-card {
            background: var(--white);
            max-width: 550px;
            width: 100%;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            animation: modalScale 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        @keyframes modalScale {
            from { transform: scale(0.95); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }
        .modal-header {
            background-color: var(--primary);
            color: var(--white);
            padding: 1.25rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .modal-close {
            background: none; border: none; color: #94a3b8; font-size: 1.25rem; cursor: pointer;
        }
        .modal-close:hover { color: var(--white); }
        .modal-body { padding: 1.5rem; }
        .modal-meta-row { display: flex; justify-content: space-between; padding: 0.5rem 0; border-bottom: 1px solid var(--border); }
    </style>
</head>
<body>

    <div class="contractor-bar">
        <div><i class="fa-solid fa-file-signature"></i> Licensed General Contractor & Insured Builder</div>
        <div><i class="fa-solid fa-phone"></i> Call Crew Desk: (555) 839-2041</div>
    </div>

    <header>
        <div class="nav-container">
            <div class="logo" onclick="switchTab('home')">
                <i class="fa-solid fa-compass-drafting"></i> APEX <span>CONSTRUCTION</span>
            </div>
            <nav>
                <button class="tab-btn active" id="btn-home" onclick="switchTab('home')">Our Standards</button>
                <button class="tab-btn" id="btn-configurator" onclick="switchTab('configurator')">Estimate Workspace</button>
                <button class="tab-btn" id="btn-gallery" onclick="switchTab('gallery')">Recent Builds</button>
                <button class="tab-btn" id="btn-quote" onclick="switchTab('quote')">Consultation</button>
            </nav>
        </div>
    </header>

    <main>
        <!-- TAB 1: SERVICE STANDARDS -->
        <section id="home" class="tab-content active">
            <div class="hero">
                <div class="hero-text">
                    <h1>We Turn Structural Blueprints Into Asset Realities</h1>
                    <p>Apex Construction specializes in turn-key post-frame buildings and timber framing services. From site grading and engineering verification to final exterior framing trim, we execute the full building lifecycle.</p>
                    <div class="cta-group">
                        <button class="btn btn-primary" onclick="switchTab('configurator')"><i class="fa-solid fa-calculator"></i> Run Dynamic Estimator</button>
                        <button class="btn btn-secondary" onclick="switchTab('gallery')">Browse Portfolio</button>
                    </div>
                </div>
                <div class="badge-grid">
                    <div class="badge-item">
                        <i class="fa-solid fa-certificate"></i>
                        <div>
                            <strong>Master Carpentry Crews</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">No sub-contracted framing squads. Every structural framing layout is built by direct crew employees.</p>
                        </div>
                    </div>
                    <div class="badge-item">
                        <i class="fa-solid fa-gavel"></i>
                        <div>
                            <strong>Zoning & Permit Assistance</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">We process structural plan submissions, wind-load data sheets, and local building inspector requests.</p>
                        </div>
                    </div>
                </div>
            </div>

            <h2 class="section-title">Our Rigorous Build Workflow</h2>
            <div class="process-flow">
                <div class="process-step">
                    <div class="step-num">01</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Site Prep & Leveling</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Excavation teams clear brush, level terrain, and grade slope configurations for concrete pad allocations.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">02</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Post Foundations</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Setting structural 3-ply laminated columns past the regional frost line with mixed base collars.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">03</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Truss Framework</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Lifting custom engineered, high wind-load roof systems safely anchored using direct heavy brackets.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">04</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Trim & Envelope</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Installing 29-gauge steel siding arrays, proper ridge venting layouts, and commercial door assemblies.</p>
                </div>
            </div>
        </section>

        <!-- TAB 2: ESTIMATOR -->
        <section id="configurator" class="tab-content">
            <div class="config-container">
                <div class="config-panel">
                    <h2 style="margin-bottom: 0.5rem;">Select Build Type</h2>
                    <p style="color: var(--text-light); margin-bottom: 1.5rem; font-size: 0.9rem;">Adjust layout options to calculate structural crew and material contract metrics.</p>
                    
                    <div class="config-group">
                        <label>Structural Building Services</label>
                        <select class="select-input" id="struct-type" onchange="updateConfiguratorOptions()">
                            <option value="barn">Substantial Post-Frame Barn</option>
                            <option value="gazebo">Architectural Open-Air Gazebo</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Footprint Allocations</label>
                        <select class="select-input" id="struct-size" onchange="updateConfigurator()">
                            <!-- Generated via State Engine -->
                        </select>
                    </div>

                    <div class="config-group" id="height-container">
                        <label>Interior Eave Ceiling Clearance</label>
                        <select class="select-input" id="struct-height" onchange="updateConfigurator()">
                            <option value="10">10 Foot Eave Clearance</option>
                            <option value="12" selected>12 Foot Eave Clearance</option>
                            <option value="14">14 Foot Eave Clearance</option>
                            <option value="16">16 Foot Eave Clearance</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Wall Panel Siding Tones</label>
                        <div class="swatch-grid" id="wall-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Roof Panel Sheeting Tones</label>
                        <div class="swatch-grid" id="roof-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Contract Add-ons & Turn-key Additions</label>
                        <div class="checkbox-tile-group" id="upgrades-container"></div>
                    </div>
                </div>

                <div class="config-preview-panel">
                    <div class="preview-canvas-wrapper">
                        <svg id="preview-svg" width="320" height="280" viewBox="0 0 320 280"></svg>
                    </div>

                    <div class="price-display">
                        <div>
                            <p style="font-size: 0.85rem; color: #94a3b8; font-weight: 600; text-transform: uppercase;">Turn-Key Range (Est)</p>
                            <span class="price-value" id="live-price">$0.00</span>
                        </div>
                        <button class="btn btn-primary" onclick="proceedToQuote()"><i class="fa-solid fa-calendar-check"></i> Consult On Layout</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: PORTFOLIO -->
        <section id="gallery" class="tab-content">
            <h2 style="margin-bottom: 0.5rem;">Completed Service Portfolio</h2>
            <p style="color: var(--text-light); margin-bottom: 2rem;">Real job sites built across our delivery boundaries over the last 12 months.</p>
            
            <div class="filter-bar">
                <button class="filter-btn active" id="f-all" onclick="filterGallery('all')">All Sites</button>
                <button class="filter-btn" id="f-barn" onclick="filterGallery('barn')">Post-Frame Barns</button>
                <button class="filter-btn" id="f-gazebo" onclick="filterGallery('gazebo')">Gazebos & Pavilions</button>
            </div>

            <div class="gallery-grid" id="gallery-wrapper"></div>
        </section>

        <!-- TAB 4: CONSULTATION REQUEST -->
        <section id="quote" class="tab-content">
            <div class="quote-layout">
                <h2 style="margin-bottom: 0.5rem;">Schedule Site Consultation</h2>
                <p style="color: var(--text-light); margin-bottom: 2rem;">Confirm your parameters below to route data directly to our operational estimation foreman.</p>
                
                <form id="quote-form" onsubmit="handleFormSubmit(event)">
                    <div class="config-group">
                        <label>Selected Estimator Layout Scope</label>
                        <textarea class="text-input" id="quote-summary" rows="3" readonly style="resize: none; background-color: #f1f5f9; font-size: 0.9rem;"></textarea>
                    </div>

                    <div class="form-row">
                        <div class="config-group">
                            <label>Full Contact Name</label>
                            <input type="text" class="text-input" id="client-name" required placeholder="Marcus Vance">
                        </div>
                        <div class="config-group">
                            <label>Direct Phone Number</label>
                            <input type="tel" class="text-input" id="client-phone" required placeholder="(555) 019-2831">
                        </div>
                    </div>

                    <div class="form-row">
                        <div class="config-group">
                            <label>Target Site ZIP Code</label>
                            <input type="text" class="text-input" id="client-zip" required placeholder="48827">
                        </div>
                        <div class="config-group">
                            <label>Current Permit/Zoning Phase</label>
                            <select class="select-input" id="client-zoning">
                                <option>Researching zoning rules</option>
                                <option>Permit documents ready</option>
                                <option>Need builder to file permits</option>
                            </select>
                        </div>
                    </div>

                    <div class="config-group">
                        <label>Site Terrain Status</label>
                        <select class="select-input" id="client-terrain">
                            <option>Perfectly level / Clear of trees</option>
                            <option>Minor clearing or slope corrections needed</option>
                            <option>Requires major commercial excavation & tree clearing</option>
                        </select>
                    </div>

                    <button type="submit" class="btn btn-primary" style="width: 100%; justify-content: center; margin-top: 1rem;">
                        <i class="fa-solid fa-file-invoice"></i> Book Builder Project Analysis
                    </button>
                </form>
            </div>
        </section>
    </main>

    <!-- PORTFOLIO PREVIEW MODAL FRAME -->
    <div class="modal-overlay" id="portfolio-modal" onclick="closeModal(event)">
        <div class="modal-card">
            <div class="modal-header">
                <h3 id="modal-title">Project Details</h3>
                <button class="modal-close" onclick="document.getElementById('portfolio-modal').classList.remove('active')"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="modal-body">
                <div id="modal-specs"></div>
                <div style="margin-top: 1.5rem; display: flex; gap: 1rem;">
                    <button class="btn btn-primary" id="modal-load-btn" style="flex: 1;">Load This Framework in Estimator</button>
                    <button class="btn btn-secondary" onclick="document.getElementById('portfolio-modal').classList.remove('active')">Close</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const colorPalette = [
            { name: 'Slate Gray', hex: '#64748b' },
            { name: 'Barn Red', hex: '#991b1b' },
            { name: 'Forest Green', hex: '#166534' },
            { name: 'Charcoal Metal', hex: '#334155' },
            { name: 'Desert Sand', hex: '#d97706' },
            { name: 'Polar White', hex: '#f8fafc' }
        ];

        const portfolioItems = [
            { id: 0, type: 'barn', title: '40x60 Insulated Commercial Workshop', locale: 'Eaton Rapids, MI', icon: 'fa-warehouse', sizeIdx: 2, wallColor: '#334155', roofColor: '#f8fafc', height: '14' },
            { id: 1, type: 'barn', title: '30x45 Agricultural Equipment Shed', locale: 'Lansing, MI', icon: 'fa-tractor', sizeIdx: 1, wallColor: '#991b1b', roofColor: '#334155', height: '12' },
            { id: 2, type: 'gazebo', title: '16ft Premium Cedar Pavilion Outfit', locale: 'Grand Rapids, MI', icon: 'fa-tree', sizeIdx: 2, wallColor: '#d97706', roofColor: '#166534', height: '10' },
            { id: 3, type: 'barn', title: '60x120 Horse Arena & Stall Wing', locale: 'Charlotte, MI', icon: 'fa-horse', sizeIdx: 2, wallColor: '#f8fafc', roofColor: '#64748b', height: '16' },
            { id: 4, type: 'gazebo', title: '12x24 Screened Open-Air Outpost', locale: 'Mason, MI', icon: 'fa-campground', sizeIdx: 0, wallColor: '#64748b', roofColor: '#334155', height: '10' },
        ];

        let state = {
            activeType: 'barn',
            wallColor: '#64748b',
            roofColor: '#334155',
            selectedUpgrades: []
        };

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
            
            document.getElementById(tabId).classList.add('active');
            document.getElementById(`btn-${tabId}`).classList.add('active');
            window.scrollTo(0,0);
        }

        window.onload = function() {
            renderSwatches();
            updateConfiguratorOptions();
            filterGallery('all');
        };

        function updateConfiguratorOptions() {
            const typeSelect = document.getElementById('struct-type').value;
            state.activeType = typeSelect;

            const sizeSelect = document.getElementById('struct-size');
            sizeSelect.innerHTML = '';

            const heightContainer = document.getElementById('height-container');
            const upgradesContainer = document.getElementById('upgrades-container');

            if (typeSelect === 'barn') {
                heightContainer.style.display = 'block';
                const sizes = [
                    { label: '24 x 32 Base Layout (Suburban Workshop)', base: 21000 },
                    { label: '30 x 40 Commercial Layout (Heavy Storage)', base: 29500 },
                    { label: '40 x 60 Agricultural Layout (Large Fleet)', base: 48000 }
                ];
                sizes.forEach(s => sizeSelect.options[sizeSelect.options.length] = new Option(s.label, s.base));

                upgradesContainer.innerHTML = `
                    <label class="checkbox-tile"><input type="checkbox" id="upg-0" value="6200" onchange="toggleUpgrade(this, 'Pour 4-Inch Concrete Floor')"> Pour 4" Reinforced Concrete Floor (+$6,200)</label>
                    <label class="checkbox-tile"><input type="checkbox" id="upg-1" value="4800" onchange="toggleUpgrade(this, '12-Foot Side Lean-To Porch')"> 12ft Integrated Side Lean-To Porch (+$4,800)</label>
                `;
            } else {
                heightContainer.style.display = 'none';
                const sizes = [
                    { label: '12 Foot Heavy-Frame Octagonal Traditional', base: 6400 },
                    { label: '14 Foot Heavy-Frame Octagonal Traditional', base: 7900 },
                    { label: '16 x 24 Foot Structural Gable Pavilion', base: 11500 }
                ];
                sizes.forEach(s => sizeSelect.options[sizeSelect.options.length] = new Option(s.label, s.base));

                upgradesContainer.innerHTML = `
                    <label class="checkbox-tile"><input type="checkbox" id="upg-0" value="1400" onchange="toggleUpgrade(this, 'High-Weave Inset Screening Frame')"> Heavy Duty Perimeter Mesh Screen System (+$1,400)</label>
                    <label class="checkbox-tile"><input type="checkbox" id="upg-1" value="950" onchange="toggleUpgrade(this, 'Concealed Electrical Package')"> Code-Approved Hidden Electrical Box Package (+$950)</label>
                `;
            }
            state.selectedUpgrades = [];
            updateConfigurator();
        }

        function renderSwatches() {
            const wallDiv = document.getElementById('wall-swatches');
            const roofDiv = document.getElementById('roof-swatches');
            wallDiv.innerHTML = ''; roofDiv.innerHTML = '';
            
            colorPalette.forEach((color) => {
                wallDiv.innerHTML += `<div class="swatch" data-hex="${color.hex}" style="background-color: ${color.hex}" onclick="selectSwatch(this, 'wall', '${color.hex}')"></div>`;
                roofDiv.innerHTML += `<div class="swatch" data-hex="${color.hex}" style="background-color: ${color.hex}" onclick="selectSwatch(this, 'roof', '${color.hex}')"></div>`;
            });
            
            // Apply default programmatic classes
            wallDiv.children[0].classList.add('selected');
            roofDiv.children[3].classList.add('selected');
        }

        function selectSwatch(element, target, hex) {
            element.parentElement.querySelectorAll('.swatch').forEach(s => s.classList.remove('selected'));
            element.classList.add('selected');
            if(target === 'wall') state.wallColor = hex;
            if(target === 'roof') state.roofColor = hex;
            updateConfigurator();
        }

        function toggleUpgrade(cb, name) {
            const val = parseFloat(cb.value);
            if(cb.checked) {
                state.selectedUpgrades.push({ name: name, cost: val });
            } else {
                state.selectedUpgrades = state.selectedUpgrades.filter(u => u.name !== name);
            }
            updateConfigurator();
        }

        function updateConfigurator() {
            const baseValue = parseFloat(document.getElementById('struct-size').value) || 0;
            const heightMultiplier = state.activeType === 'barn' ? (parseInt(document.getElementById('struct-height').value) - 12) * 1100 : 0;
            const upgradesTotal = state.selectedUpgrades.reduce((sum, item) => sum + item.cost, 0);
            
            const totalProjectCost = baseValue + heightMultiplier + upgradesTotal;

            const variantMin = totalProjectCost * 0.95;
            const variantMax = totalProjectCost * 1.05;
            const formatCurr = (v) => new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', maximumFractionDigits: 0 }).format(v);
            
            document.getElementById('live-price').innerText = `${formatCurr(variantMin)} - ${formatCurr(variantMax)}`;
            
            const svg = document.getElementById('preview-svg');
            if (state.activeType === 'barn') {
                svg.innerHTML = `
                    <polygon points="40,120 160,50 280,120" fill="${state.roofColor}" stroke="#0f172a" stroke-width="2"/>
                    <rect x="55" y="120" width="210" height="120" fill="${state.wallColor}" stroke="#0f172a" stroke-width="2"/>
                    <rect x="120" y="150" width="80" height="90" fill="#cbd5e1" stroke="#334155" stroke-width="2"/>
                    <line x1="160" y1="150" x2="160" y2="240" stroke="#334155" stroke-width="1"/>
                `;
            } else {
                svg.innerHTML = `
                    <polygon points="70,100 160,40 250,100" fill="${state.roofColor}" stroke="#0f172a" stroke-width="2"/>
                    <rect x="80" y="100" width="160" height="110" fill="none" stroke="${state.wallColor}" stroke-width="6"/>
                    <line x1="110" y1="100" x2="110" y2="210" stroke="#475569" stroke-width="3"/>
                    <line x1="140" y1="100" x2="140" y2="210" stroke="#475569" stroke-width="3"/>
                    <line x1="180" y1="100" x2="180" y2="210" stroke="#475569" stroke-width="3"/>
                    <line x1="210" y1="100" x2="210" y2="210" stroke="#475569" stroke-width="3"/>
                    <rect x="80" y="195" width="160" height="15" fill="#cbd5e1" stroke="#475569" stroke-width="2"/>
                `;
            }
        }

        function filterGallery(category) {
            const container = document.getElementById('gallery-wrapper');
            container.innerHTML = '';
            
            document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
            document.getElementById(`f-${category}`).classList.add('active');

            const items = category === 'all' ? portfolioItems : portfolioItems.filter(i => i.type === category);
            
            items.forEach(item => {
                container.innerHTML += `
                    <div class="gallery-card" onclick="openPortfolioItem(${item.id})">
                        <div class="gallery-img-placeholder"><i class="fa-solid ${item.icon}"></i></div>
                        <div class="gallery-info">
                            <h3>${item.title}</h3>
                            <p style="color: var(--text-light); font-size:0.9rem;"><i class="fa-solid fa-location-dot"></i> Location: ${item.locale}</p>
                            <span style="display:inline-block; margin-top:0.5rem; font-size:0.8rem; font-weight:700; color:var(--accent)">VIEW BUILD CONFIGURATION &rarr;</span>
                        </div>
                    </div>
                `;
            });
        }

        function openPortfolioItem(id) {
            const item = portfolioItems.find(p => p.id === id);
            if (!item) return;

            document.getElementById('modal-title').innerText = item.title;
            
            const matchWall = colorPalette.find(c => c.hex === item.wallColor)?.name || 'Custom';
            const matchRoof = colorPalette.find(c => c.hex === item.roofColor)?.name || 'Custom';

            document.getElementById('modal-specs').innerHTML = `
                <div class="modal-meta-row"><strong>Structure Profile:</strong> <span>${item.type === 'barn' ? 'Post-Frame Barn' : 'Gazebo / Pavilion'}</span></div>
                <div class="modal-meta-row"><strong>Location Boundary:</strong> <span>${item.locale}</span></div>
                <div class="modal-meta-row"><strong>Exterior Siding Tone:</strong> <span>${matchWall}</span></div>
                <div class="modal-meta-row"><strong>Roof System Coating:</strong> <span>${matchRoof}</span></div>
                ${item.type === 'barn' ? `<div class="modal-meta-row"><strong>Ceiling Clearance:</strong> <span>${item.height} Foot Eaves</span></div>` : ''}
            `;

            const loadBtn = document.getElementById('modal-load-btn');
            loadBtn.onclick = function() {
                loadFrameworkIntoWorkspace(item);
            };

            document.getElementById('portfolio-modal').classList.add('active');
        }

        function loadFrameworkIntoWorkspace(item) {
            document.getElementById('struct-type').value = item.type;
            updateConfiguratorOptions();
            
            document.getElementById('struct-size').selectedIndex = item.sizeIdx;
            
            if (item.type === 'barn') {
                document.getElementById('struct-height').value = item.height;
            }

            state.wallColor = item.wallColor;
            state.roofColor = item.roofColor;

            // Update visible UI swatch selections
            document.querySelectorAll('#wall-swatches .swatch').forEach(s => s.classList.toggle('selected', s.getAttribute('data-hex') === item.wallColor));
            document.querySelectorAll('#roof-swatches .swatch').forEach(s => s.classList.toggle('selected', s.getAttribute('data-hex') === item.roofColor));

            updateConfigurator();
            document.getElementById('portfolio-modal').classList.remove('active');
            switchTab('configurator');
        }

        function closeModal(e) {
            if (e.target.id === 'portfolio-modal') {
                document.getElementById('portfolio-modal').classList.remove('active');
            }
        }

        function proceedToQuote() {
            const sizeSelect = document.getElementById('struct-size');
            const sizeText = sizeSelect.options[sizeSelect.selectedIndex]?.text || '';
            let summary = `Project Scope: Custom ${state.activeType === 'barn' ? 'Post-Frame Barn' : 'Heavy Timber Pavilion'}\nPlanned Matrix Size: ${sizeText}\nEst Color Profiles: Walls(${state.wallColor}) / Sheet Roof System(${state.roofColor})`;
            
            if (state.activeType === 'barn') {
                summary += `\nEave Wall Height: ${document.getElementById('struct-height').value} FT`;
            }
            if (state.selectedUpgrades.length > 0) {
                summary += `\nIncluded Add-ons: ${state.selectedUpgrades.map(u => u.name).join(', ')}`;
            }
            
            document.getElementById('quote-summary').value = summary;
            switchTab('quote');
        }

        function handleFormSubmit(e) {
            e.preventDefault();
            const name = document.getElementById('client-name').value;
            alert(`Thank you, ${name}! Your project framework data has been captured. An estimation foreman will contact you directly within 24 business hours.`);
            document.getElementById('quote-form').reset();
            switchTab('home');
        }
    </script>
</body>
</html>
         font-weight: 700;
            letter-spacing: -0.5px;
            cursor: pointer;
        }

        .logo span {
            color: #f97316;
        }

        nav {
            display: flex;
            gap: 0.75rem;
        }

        .tab-btn {
            background: none;
            border: none;
            color: #94a3b8;
            padding: 0.6rem 1.2rem;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            border-radius: var(--radius);
        }

        .tab-btn:hover {
            color: var(--white);
            background-color: rgba(255,255,255,0.05);
        }

        .tab-btn.active {
            color: var(--white);
            background-color: var(--accent);
        }

        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 2rem;
            min-height: 75vh;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(4px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Home Tab Content */
        .hero {
            display: grid;
            grid-template-columns: 1.2fr 0.8fr;
            gap: 3rem;
            align-items: center;
            padding: 2.5rem 0;
        }

        .hero-text h1 {
            font-size: 2.75rem;
            color: var(--primary);
            line-height: 1.15;
            margin-bottom: 1.25rem;
        }

        .hero-text p {
            font-size: 1.1rem;
            color: var(--text-light);
            margin-bottom: 2rem;
        }

        .cta-group {
            display: flex;
            gap: 1rem;
        }

        .btn {
            padding: 0.75rem 1.5rem;
            font-size: 1rem;
            font-weight: 600;
            border-radius: var(--radius);
            cursor: pointer;
            transition: all 0.2s ease;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
        }

        .btn-primary { background-color: var(--accent); color: var(--white); border: none; }
        .btn-primary:hover { background-color: var(--accent-hover); }
        .btn-secondary { background-color: transparent; color: var(--primary); border: 2px solid var(--primary); }
        .btn-secondary:hover { background-color: var(--primary); color: var(--white); }

        .badge-grid {
            background: #f1f5f9;
            padding: 2rem;
            border-radius: var(--radius);
            display: flex;
            flex-direction: column;
            gap: 1.25rem;
            border-left: 4px solid var(--accent);
        }

        .badge-item {
            display: flex;
            gap: 1rem;
            align-items: flex-start;
        }

        .badge-item i {
            color: var(--accent);
            font-size: 1.25rem;
            margin-top: 0.2rem;
        }

        .section-title {
            font-size: 1.75rem;
            color: var(--primary);
            margin: 3rem 0 1.5rem 0;
            text-align: center;
        }

        .process-flow {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1.5rem;
            margin-bottom: 4rem;
        }

        .process-step {
            background: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
        }

        .step-num {
            position: absolute;
            top: -15px;
            right: 15px;
            font-size: 2.5rem;
            font-weight: 800;
            color: #e2e8f0;
            line-height: 1;
        }

        /* Design Configurator */
        .config-container {
            display: grid;
            grid-template-columns: 440px 1fr;
            gap: 2rem;
            background: var(--white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
        }

        .config-panel {
            padding: 2rem;
            border-right: 1px solid var(--border);
            max-height: 780px;
            overflow-y: auto;
        }

        .config-preview-panel {
            background: #f1f5f9;
            padding: 2rem;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
        }

        .config-group {
            margin-bottom: 1.5rem;
        }

        .config-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 0.5rem;
            font-size: 0.95rem;
        }

        .select-input, .text-input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            background-color: var(--bg-light);
            font-size: 1rem;
        }

        .swatch-grid {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .swatch {
            width: 34px;
            height: 34px;
            border-radius: 4px;
            border: 2px solid var(--white);
            outline: 1px solid var(--border);
            cursor: pointer;
        }

        .swatch.selected { outline: 2px solid var(--accent); }

        .checkbox-tile-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .checkbox-tile {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            cursor: pointer;
            background-color: var(--bg-light);
            user-select: none;
        }

        .preview-canvas-wrapper {
            background: var(--white);
            width: 100%;
            height: 420px;
            border-radius: var(--radius);
            box-shadow: inset 0 2px 4px 0 rgba(0,0,0,0.05);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .price-display {
            width: 100%;
            background: var(--primary);
            color: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            margin-top: 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .price-value {
            font-size: 1.65rem;
            font-weight: 700;
            color: #f97316;
        }

        /* Portfolio Cards */
        .filter-bar { display: flex; gap: 0.5rem; margin-bottom: 2rem; }
        .filter-btn {
            background: var(--white); border: 1px solid var(--border);
            padding: 0.5rem 1.25rem; border-radius: 20px; cursor: pointer;
            font-weight: 600; transition: all 0.2s ease;
        }
        .filter-btn.active, .filter-btn:hover { background: var(--primary); color: var(--white); border-color: var(--primary); }
        
        .gallery-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1.5rem; }
        .gallery-card { 
            background: var(--white); 
            border-radius: var(--radius); 
            overflow: hidden; 
            box-shadow: var(--shadow); 
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .gallery-card:hover {
            transform: translateY(-4px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1);
        }
        .gallery-img-placeholder { height: 210px; background-color: #cbd5e1; display: flex; align-items: center; justify-content: center; color: var(--text-light); font-size: 3rem; }
        .gallery-info { padding: 1.5rem; }
        .gallery-info h3 { margin-bottom: 0.25rem; color: var(--primary); }

        /* Consultation Onboarding Form */
        .quote-layout { max-width: 650px; margin: 0 auto; background: var(--white); padding: 3rem; border-radius: var(--radius); box-shadow: var(--shadow); }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

        /* Interactive Detail Overlays (Modal) */
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(15, 23, 42, 0.6);
            backdrop-filter: blur(4px);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            padding: 1.5rem;
        }
        .modal-overlay.active { display: flex; }
        .modal-card {
            background: var(--white);
            max-width: 550px;
            width: 100%;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            animation: modalScale 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        @keyframes modalScale {
            from { transform: scale(0.95); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }
        .modal-header {
            background-color: var(--primary);
            color: var(--white);
            padding: 1.25rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .modal-close {
            background: none; border: none; color: #94a3b8; font-size: 1.25rem; cursor: pointer;
        }
        .modal-close:hover { color: var(--white); }
        .modal-body { padding: 1.5rem; }
        .modal-meta-row { display: flex; justify-content: space-between; padding: 0.5rem 0; border-bottom: 1px solid var(--border); }
    </style>
</head>
<body>

    <div class="contractor-bar">
        <div><i class="fa-solid fa-file-signature"></i> Licensed General Contractor & Insured Builder</div>
        <div><i class="fa-solid fa-phone"></i> Call Crew Desk: (555) 839-2041</div>
    </div>

    <header>
        <div class="nav-container">
            <div class="logo" onclick="switchTab('home')">
                <i class="fa-solid fa-compass-drafting"></i> APEX <span>CONSTRUCTION</span>
            </div>
            <nav>
                <button class="tab-btn active" id="btn-home" onclick="switchTab('home')">Our Standards</button>
                <button class="tab-btn" id="btn-configurator" onclick="switchTab('configurator')">Estimate Workspace</button>
                <button class="tab-btn" id="btn-gallery" onclick="switchTab('gallery')">Recent Builds</button>
                <button class="tab-btn" id="btn-quote" onclick="switchTab('quote')">Consultation</button>
            </nav>
        </div>
    </header>

    <main>
        <!-- TAB 1: SERVICE STANDARDS -->
        <section id="home" class="tab-content active">
            <div class="hero">
                <div class="hero-text">
                    <h1>We Turn Structural Blueprints Into Asset Realities</h1>
                    <p>Apex Construction specializes in turn-key post-frame buildings and timber framing services. From site grading and engineering verification to final exterior framing trim, we execute the full building lifecycle.</p>
                    <div class="cta-group">
                        <button class="btn btn-primary" onclick="switchTab('configurator')"><i class="fa-solid fa-calculator"></i> Run Dynamic Estimator</button>
                        <button class="btn btn-secondary" onclick="switchTab('gallery')">Browse Portfolio</button>
                    </div>
                </div>
                <div class="badge-grid">
                    <div class="badge-item">
                        <i class="fa-solid fa-certificate"></i>
                        <div>
                            <strong>Master Carpentry Crews</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">No sub-contracted framing squads. Every structural framing layout is built by direct crew employees.</p>
                        </div>
                    </div>
                    <div class="badge-item">
                        <i class="fa-solid fa-gavel"></i>
                        <div>
                            <strong>Zoning & Permit Assistance</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">We process structural plan submissions, wind-load data sheets, and local building inspector requests.</p>
                        </div>
                    </div>
                </div>
            </div>

            <h2 class="section-title">Our Rigorous Build Workflow</h2>
            <div class="process-flow">
                <div class="process-step">
                    <div class="step-num">01</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Site Prep & Leveling</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Excavation teams clear brush, level terrain, and grade slope configurations for concrete pad allocations.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">02</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Post Foundations</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Setting structural 3-ply laminated columns past the regional frost line with mixed base collars.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">03</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Truss Framework</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Lifting custom engineered, high wind-load roof systems safely anchored using direct heavy brackets.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">04</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Trim & Envelope</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Installing 29-gauge steel siding arrays, proper ridge venting layouts, and commercial door assemblies.</p>
                </div>
            </div>
        </section>

        <!-- TAB 2: ESTIMATOR -->
        <section id="configurator" class="tab-content">
            <div class="config-container">
                <div class="config-panel">
                    <h2 style="margin-bottom: 0.5rem;">Select Build Type</h2>
                    <p style="color: var(--text-light); margin-bottom: 1.5rem; font-size: 0.9rem;">Adjust layout options to calculate structural crew and material contract metrics.</p>
                    
                    <div class="config-group">
                        <label>Structural Building Services</label>
                        <select class="select-input" id="struct-type" onchange="updateConfiguratorOptions()">
                            <option value="barn">Substantial Post-Frame Barn</option>
                            <option value="gazebo">Architectural Open-Air Gazebo</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Footprint Allocations</label>
                        <select class="select-input" id="struct-size" onchange="updateConfigurator()">
                            <!-- Generated via State Engine -->
                        </select>
                    </div>

                    <div class="config-group" id="height-container">
                        <label>Interior Eave Ceiling Clearance</label>
                        <select class="select-input" id="struct-height" onchange="updateConfigurator()">
                            <option value="10">10 Foot Eave Clearance</option>
                            <option value="12" selected>12 Foot Eave Clearance</option>
                            <option value="14">14 Foot Eave Clearance</option>
                            <option value="16">16 Foot Eave Clearance</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Wall Panel Siding Tones</label>
                        <div class="swatch-grid" id="wall-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Roof Panel Sheeting Tones</label>
                        <div class="swatch-grid" id="roof-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Contract Add-ons & Turn-key Additions</label>
                        <div class="checkbox-tile-group" id="upgrades-container"></div>
                    </div>
                </div>

                <div class="config-preview-panel">
                    <div class="preview-canvas-wrapper">
                        <svg id="preview-svg" width="320" height="280" viewBox="0 0 320 280"></svg>
                    </div>

                    <div class="price-display">
                        <div>
                            <p style="font-size: 0.85rem; color: #94a3b8; font-weight: 600; text-transform: uppercase;">Turn-Key Range (Est)</p>
                            <span class="price-value" id="live-price">$0.00</span>
                        </div>
                        <button class="btn btn-primary" onclick="proceedToQuote()"><i class="fa-solid fa-calendar-check"></i> Consult On Layout</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: PORTFOLIO -->
        <section id="gallery" class="tab-content">
            <h2 style="margin-bottom: 0.5rem;"<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Apex Construction Group | Custom Pole Barns & Gazebos</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
    <style>
        :root {
            --primary: #0f172a;
            --accent: #c2410c;
            --accent-hover: #9a3412;
            --text-main: #334155;
            --text-light: #64748b;
            --bg-light: #f8fafc;
            --white: #ffffff;
            --border: #e2e8f0;
            --radius: 8px;
            --shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-light);
            color: var(--text-main);
            line-height: 1.6;
        }

        /* Top Contractor Bar */
        .contractor-bar {
            background-color: #1e293b;
            color: #94a3b8;
            font-size: 0.85rem;
            padding: 0.5rem 2rem;
            display: flex;
            justify-content: space-between;
            border-bottom: 1px solid #334155;
        }

        /* Navigation */
        header {
            background-color: var(--primary);
            color: var(--white);
            position: sticky;
            top: 0;
            z-index: 1000;
            box-shadow: var(--shadow);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 2rem;
        }

        .logo {
            font-size: 1.4rem;
            font-weight: 700;
            letter-spacing: -0.5px;
        }

        .logo span {
            color: #f97316;
        }

        nav {
            display: flex;
            gap: 0.75rem;
        }

        .tab-btn {
            background: none;
            border: none;
            color: #94a3b8;
            padding: 0.6rem 1.2rem;
            font-size: 0.95rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
            border-radius: var(--radius);
        }

        .tab-btn:hover {
            color: var(--white);
            background-color: rgba(255,255,255,0.05);
        }

        .tab-btn.active {
            color: var(--white);
            background-color: var(--accent);
        }

        /* Layout Container */
        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 2rem;
            min-height: 75vh;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.3s ease-in-out;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(4px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Tab 1: Service Overview & Home */
        .hero {
            display: grid;
            grid-template-columns: 1.2fr 0.8fr;
            gap: 3rem;
            align-items: center;
            padding: 2.5rem 0;
        }

        .hero-text h1 {
            font-size: 2.75rem;
            color: var(--primary);
            line-height: 1.15;
            margin-bottom: 1.25rem;
        }

        .hero-text p {
            font-size: 1.1rem;
            color: var(--text-light);
            margin-bottom: 2rem;
        }

        .cta-group {
            display: flex;
            gap: 1rem;
        }

        .btn {
            padding: 0.75rem 1.5rem;
            font-size: 1rem;
            font-weight: 600;
            border-radius: var(--radius);
            cursor: pointer;
            transition: all 0.2s ease;
            text-decoration: none;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
        }

        .btn-primary { background-color: var(--accent); color: var(--white); border: none; }
        .btn-primary:hover { background-color: var(--accent-hover); }
        .btn-secondary { background-color: transparent; color: var(--primary); border: 2px solid var(--primary); }
        .btn-secondary:hover { background-color: var(--primary); color: var(--white); }

        .badge-grid {
            background: #f1f5f9;
            padding: 2rem;
            border-radius: var(--radius);
            display: flex;
            flex-direction: column;
            gap: 1.25rem;
            border-left: 4px solid var(--accent);
        }

        .badge-item {
            display: flex;
            gap: 1rem;
            align-items: flex-start;
        }

        .badge-item i {
            color: var(--accent);
            font-size: 1.25rem;
            margin-top: 0.2rem;
        }

        .section-title {
            font-size: 1.75rem;
            color: var(--primary);
            margin: 3rem 0 1.5rem 0;
            text-align: center;
        }

        .process-flow {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1.5rem;
            margin-bottom: 4rem;
        }

        .process-step {
            background: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
        }

        .step-num {
            position: absolute;
            top: -15px;
            right: 15px;
            font-size: 2.5rem;
            font-weight: 800;
            color: #e2e8f0;
            line-height: 1;
        }

        /* Tab 2: Service Configurator */
        .config-container {
            display: grid;
            grid-template-columns: 440px 1fr;
            gap: 2rem;
            background: var(--white);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
        }

        .config-panel {
            padding: 2rem;
            border-right: 1px solid var(--border);
            max-height: 780px;
            overflow-y: auto;
        }

        .config-preview-panel {
            background: #f1f5f9;
            padding: 2rem;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
        }

        .config-group {
            margin-bottom: 1.5rem;
        }

        .config-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 0.5rem;
            font-size: 0.95rem;
        }

        .select-input, .text-input {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            background-color: var(--bg-light);
            font-size: 1rem;
        }

        .swatch-grid {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .swatch {
            width: 34px;
            height: 34px;
            border-radius: 4px;
            border: 2px solid var(--white);
            outline: 1px solid var(--border);
            cursor: pointer;
        }

        .swatch.selected { outline: 2px solid var(--accent); }

        .checkbox-tile-group {
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .checkbox-tile {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 0.75rem;
            border: 1px solid var(--border);
            border-radius: var(--radius);
            cursor: pointer;
            background-color: var(--bg-light);
        }

        .preview-canvas-wrapper {
            background: var(--white);
            width: 100%;
            height: 420px;
            border-radius: var(--radius);
            box-shadow: inset 0 2px 4px 0 rgba(0,0,0,0.05);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .price-display {
            width: 100%;
            background: var(--primary);
            color: var(--white);
            padding: 1.5rem;
            border-radius: var(--radius);
            margin-top: 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .price-value {
            font-size: 1.75rem;
            font-weight: 700;
            color: #f97316;
        }

        /* Tab 3: Gallery Matrix */
        .filter-bar { display: flex; gap: 0.5rem; margin-bottom: 2rem; }
        .filter-btn {
            background: var(--white); border: 1px solid var(--border);
            padding: 0.5rem 1.25rem; border-radius: 20px; cursor: pointer;
            font-weight: 600; transition: all 0.2s ease;
        }
        .filter-btn.active, .filter-btn:hover { background: var(--primary); color: var(--white); border-color: var(--primary); }
        
        .gallery-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(320px, 1fr)); gap: 1.5rem; }
        .gallery-card { background: var(--white); border-radius: var(--radius); overflow: hidden; box-shadow: var(--shadow); }
        .gallery-img-placeholder { height: 210px; background-color: #cbd5e1; display: flex; align-items: center; justify-content: center; color: var(--text-light); font-size: 3rem; }
        .gallery-info { padding: 1.5rem; }
        .gallery-info h3 { margin-bottom: 0.25rem; color: var(--primary); }

        /* Tab 4: Consultation Form */
        .quote-layout { max-width: 650px; margin: 0 auto; background: var(--white); padding: 3rem; border-radius: var(--radius); box-shadow: var(--shadow); }
        .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

        @media (max-width: 968px) {
            .hero { grid-template-columns: 1fr; gap: 2rem; }
            .config-container { grid-template-columns: 1fr; }
            .process-flow { grid-template-columns: 1fr 1fr; }
        }
    </style>
</head>
<body>

    <div class="contractor-bar">
        <div><i class="fa-solid fa-file-signature"></i> Licensed General Contractor & Insured Builder</div>
        <div><i class="fa-solid fa-phone"></i> Call Crew Desk: (555) 839-2041</div>
    </div>

    <header>
        <div class="nav-container">
            <div class="logo">
                <i class="fa-solid fa-compass-drafting"></i> APEX <span>CONSTRUCTION</span>
            </div>
            <nav>
                <button class="tab-btn active" onclick="switchTab('home')">Our Standards</button>
                <button class="tab-btn" onclick="switchTab('configurator')">Estimate Estimate</button>
                <button class="tab-btn" onclick="switchTab('gallery')">Recent Builds</button>
                <button class="tab-btn" onclick="switchTab('quote')">Consultation</button>
            </nav>
        </div>
    </header>

    <main>
        <!-- TAB 1: SERVICE STANDARDS -->
        <section id="home" class="tab-content active">
            <div class="hero">
                <div class="hero-text">
                    <h1>We Turn Structural Blueprints Into Asset Realities</h1>
                    <p>Apex Construction specializes in turn-key post-frame buildings and timber framing services. From site grading and engineering verification to final exterior framing trim, we execute the full building lifecycle.</p>
                    <div class="cta-group">
                        <button class="btn btn-primary" onclick="switchTab('configurator')"><i class="fa-solid fa-calculator"></i> Run Dynamic Estimator</button>
                        <button class="btn btn-secondary" onclick="switchTab('quote')">Book Site Assessment</button>
                    </div>
                </div>
                <div class="badge-grid">
                    <div class="badge-item">
                        <i class="fa-solid fa-certificate"></i>
                        <div>
                            <strong>Master Carpentry Crews</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">No sub-contracted framing squads. Every structural framing layout is built by direct crew employees.</p>
                        </div>
                    </div>
                    <div class="badge-item">
                        <i class="fa-solid fa-gavel"></i>
                        <div>
                            <strong>Zoning & Permit Assistance</strong>
                            <p style="font-size: 0.9rem; color: var(--text-light)">We process structural plan submissions, wind-load data sheets, and local building inspector requests.</p>
                        </div>
                    </div>
                </div>
            </div>

            <h2 class="section-title">Our Rigorous Build Workflow</h2>
            <div class="process-flow">
                <div class="process-step">
                    <div class="step-num">01</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Site Prep & Leveling</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Excavation teams clear brush, level terrain, and grade slope configurations for concrete pad allocations.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">02</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Post Foundations</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Setting structural 3-ply laminated columns past the regional frost line with mixed base collars.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">03</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Truss Truss Truss</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Lifting custom engineered, high wind-load roof systems safely anchored using direct heavy brackets.</p>
                </div>
                <div class="process-step">
                    <div class="step-num">04</div>
                    <h4 style="margin-bottom: 0.5rem; color: var(--primary);">Trim & Envelope</h4>
                    <p style="font-size: 0.9rem; color: var(--text-light);">Installing 29-gauge steel siding arrays, proper ridge venting layouts, and commercial door assemblies.</p>
                </div>
            </div>
        </section>

        <!-- TAB 2: ESTIMATOR -->
        <section id="configurator" class="tab-content">
            <div class="config-container">
                <div class="config-panel">
                    <h2 style="margin-bottom: 0.5rem;">Select Build Type</h2>
                    <p style="color: var(--text-light); margin-bottom: 1.5rem; font-size: 0.9rem;">Adjust layout options to calculate structural crew and material contract metrics.</p>
                    
                    <div class="config-group">
                        <label>Structural Building Services</label>
                        <select class="select-input" id="struct-type" onchange="updateConfigurator()">
                            <option value="barn">Substantial Post-Frame Barn</option>
                            <option value="gazebo">Architectural Open-Air Gazebo</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Footprint Allocations</label>
                        <select class="select-input" id="struct-size" onchange="updateConfigurator()">
                            <!-- Managed via state script -->
                        </select>
                    </div>

                    <div class="config-group" id="height-container">
                        <label>Interior Eave Ceiling Clearance</label>
                        <select class="select-input" id="struct-height" onchange="updateConfigurator()">
                            <option value="10">10 Foot Eave Clearance</option>
                            <option value="12" selected>12 Foot Eave Clearance</option>
                            <option value="14">14 Foot Eave Clearance</option>
                            <option value="16">16 Foot Eave Clearance</option>
                        </select>
                    </div>

                    <div class="config-group">
                        <label>Wall Panel Siding Tones</label>
                        <div class="swatch-grid" id="wall-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Roof Panel Sheeting Tones</label>
                        <div class="swatch-grid" id="roof-swatches"></div>
                    </div>

                    <div class="config-group">
                        <label>Contract Add-ons & Turn-key Additions</label>
                        <div class="checkbox-tile-group" id="upgrades-container"></div>
                    </div>
                </div>

                <div class="config-preview-panel">
                    <div class="preview-canvas-wrapper">
                        <svg id="preview-svg" width="320" height="280" viewBox="0 0 320 280">
                            <!-- Dynamic Structural Elements -->
                        </svg>
                    </div>

                    <div class="price-display">
                        <div>
                            <p style="font-size: 0.85rem; color: #94a3b8; font-weight: 600; text-transform: uppercase;">Turn-Key Range (Est)</p>
                            <span class="price-value" id="live-price">$0.00</span>
                        </div>
                        <button class="btn btn-primary" onclick="proceedToQuote()"><i class="fa-solid fa-calendar-check"></i> Consult On Layout</button>
                    </div>
                </div>
            </div>
        </section>

        <!-- TAB 3: PORTFOLIO -->
        <section id="gallery" class="tab-content">
            <h2 style="margin-bottom: 0.5rem;">Completed Service Portfolio</h2>
            <p style="color: var(--text-light); margin-bottom: 2rem;">Real job sites built across our delivery boundaries over the last 12 months.</p>
            
            <div class="filter-bar">
                <button class="filter-btn active" onclick="filterGallery('all')">All Sites</button>
                <button class="filter-btn" onclick="filterGallery('barn')">Post-Frame Barns</button>
                <button class="filter-btn" onclick="filterGallery('gazebo')">Gazebos & Pavilions</button>
            </div>

            <div class="gallery-grid" id="gallery-wrapper"></div>
        </section>

        <!-- TAB 4: CONSULTATION REQUEST -->
        <section id="quote" class="tab-content">
            <div class="quote-layout">
                <h2 style="margin-bottom: 0.5rem;">Schedule Site Consultation</h2>
                <p style="color: var(--text-light); margin-bottom: 2rem;">Confirm your parameters below to route data directly to our operational estimation foreman.</p>
                
                <form id="quote-form" onsubmit="handleFormSubmit(event)">
                    <div class="config-group">
                        <label>Selected Estimator Layout Scope</label>
                        <textarea class="text-input" id="quote-summary" rows="3" readonly style="resize: none; background-color: #f1f5f9; font-size: 0.9rem;"></textarea>
                    </div>

                    <div class="form-row">
                        <div class="config-group">
                            <label>Full Contact Name</label    <div class="bg-slate-900 border-b border-slate-800 text-center py-2.5 px-4 text-[11px] font-medium tracking-wider text-slate-400 flex items-center justify-center gap-2">
        <span class="w-1.5 h-1.5 rounded-full bg-amber-500"></span>
        <span>Current Pricing reflect Q3 engineered lumber indexes for regional delivery grids.</span>
    </div>

    <!-- Main Navigation Header -->
    <header class="border-b border-slate-900 bg-slate-950/80 backdrop-blur-md sticky top-0 z-50 py-4 px-6 flex justify-between items-center">
        <div class="flex items-center gap-3">
            <div class="bg-slate-900 border border-slate-800 p-2 rounded-xl text-slate-200">
                <i class="fa-solid fa-compass-drafting text-sm"></i>
            </div>
            <span class="text-md font-bold tracking-tight text-white">IRONCLAD <span class="text-slate-400 font-normal">STUDIO</span></span>
        </div>
        <div class="flex items-center gap-4">
            <span class="text-xs text-slate-500 font-mono">v2.4.1 // Stable Build</span>
        </div>
    </header>

    <main class="max-w-7xl mx-auto p-4 lg:p-8 grid grid-cols-1 lg:grid-cols-12 gap-8">
        
        <!-- Left Column: Image Canvas & Live Material List -->
        <div class="lg:col-span-7 space-y-6">
            <div class="bg-slate-900/40 border border-slate-900 rounded-2xl p-5 shadow-2xl relative overflow-hidden backdrop-blur-sm">
                
                <div class="flex justify-between items-center mb-4">
                    <div>
                        <span class="text-[10px] font-mono tracking-widest text-slate-500 uppercase">3D Perspective Space</span>
                        <h2 class="text-lg font-bold tracking-tight text-white" id="visualizerTitle">Suburban Workshop Setup</h2>
                    </div>
                    <div class="text-[11px] font-mono text-slate-400 flex items-center gap-2">
                        <span class="w-2 h-2 rounded-full bg-emerald-500"></span> Verified Engineering
                    </div>
                </div>
                
                <!-- Main Display Render -->
                <div class="bg-slate-950 rounded-xl border border-slate-900 overflow-hidden relative min-h-[350px] lg:min-h-[400px] flex items-center justify-center">
                    <img id="mainBarnVisual" src="https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=800&q=80" class="w-full h-full min-h-[350px] lg:min-h-[400px] object-cover transition-all duration-500 filter brightness-95" alt="Post Frame Structure Profile">
                    <div class="absolute inset-0 bg-gradient-to-t from-slate-950/90 via-transparent to-transparent"></div>
                    
                    <!-- Clean Architectural Spec Badge -->
                    <div class="absolute bottom-4 left-4 bg-slate-900/90 backdrop-blur-md border border-slate-800 px-3.5 py-2.5 rounded-lg text-xs space-y-1 shadow-xl">
                        <p class="text-[10px] text-slate-500 font-mono uppercase tracking-wider">Active Workspace footprint</p>
                        <p class="font-bold text-white flex items-center gap-2">
                            <i class="fa-solid fa-expand text-slate-400 text-xs"></i> <span id="overlayDimensions">30' x 40' Timber Frame</span>
                        </p>
                    </div>
                </div>

                <!-- Clean Pricing Summary Area -->
                <div class="mt-4 p-4 bg-slate-950/60 rounded-xl border border-slate-900 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                    <div>
                        <p class="text-[10px] text-slate-500 font-mono uppercase tracking-wider">Estimated Pre-Engineered Kit Total</p>
                        <div class="flex items-baseline gap-2 mt-0.5">
                            <span class="text-2xl font-bold tracking-tight text-white" id="priceDisplay">$12,400</span>
                            <span class="text-xs font-mono text-slate-500">MSRP Base USD</span>
                        </div>
                    </div>
                    <div class="text-right text-xs text-slate-400 max-w-xs sm:text-right text-left">
                        <p class="font-medium text-slate-300">FOB Manufacturing Hub Pricing</p>
                        <p class="text-[11px] text-slate-500 mt-0.5">Includes full framing blueprints, trim matrices, and fastener schedules.</p>
                    </div>
                </div>
            </div>

            <!-- NEW FEATURE: Live Dynamic Material Manifest Sheet -->
            <div class="bg-slate-900/20 border border-slate-900 rounded-2xl p-5 space-y-3">
                <h4 class="text-xs font-mono uppercase tracking-wider text-slate-400 flex items-center gap-2">
                    <i class="fa-solid fa-list-check text-slate-500"></i> Calculated Structural Manifest
                </h4>
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-2 text-xs">
                    <div class="bg-slate-900/40 border border-slate-900 p-3 rounded-xl flex justify-between items-center">
                        <span class="text-slate-500">Main Framing Post Profiles</span>
                        <span class="font-mono font-bold text-slate-300" id="manifestPosts">3-Ply 2x6 Laminated</span>
                    </div>
                    <div class="bg-slate-900/40 border border-slate-900 p-3 rounded-xl flex justify-between items-center">
                        <span class="text-slate-500">Truss Configuration</span>
                        <span class="font-mono font-bold text-slate-300" id="manifestTrusses">4/12 Pitch Steel</span>
                    </div>
                    <div class="bg-slate-900/40 border border-slate-900 p-3 rounded-xl flex justify-between items-center">
                        <span class="text-slate-500">Eave Overhang System</span>
                        <span class="font-mono font-bold text-slate-300" id="manifestOverhang">Flush Edge</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Right Column: Professional Architectural Control Board -->
        <div class="lg:col-span-5 space-y-4">
            
            <div class="grid grid-cols-2 gap-2 bg-slate-900/80 p-1 rounded-xl border border-slate-800">
                <button onclick="switchTab('design')" id="tab-design" class="w-full py-2.5 text-xs font-semibold tracking-wider uppercase rounded-lg bg-slate-800 border border-slate-700 text-white shadow-md transition-all">
                    Structural Options
                </button>
                <button onclick="switchTab('quote')" id="tab-quote" class="w-full py-2.5 text-xs font-semibold tracking-wider uppercase rounded-lg text-slate-500 hover:text-slate-300 transition-all">
                    Request Engineering Quote
                </button>
            </div>

            <!-- Option Interface Panel -->
            <div id="panel-design" class="bg-slate-900/30 border border-slate-900 p-6 rounded-2xl space-y-6 overflow-y-auto max-h-[75vh] custom-scrollbar">
                
                <!-- Structural Footprint Style -->
                <div>
                    <label class="block text-[11px] font-mono uppercase tracking-widest text-slate-500 mb-2.5">1. Architectural Profile</label>
                    <div class="grid grid-cols-2 gap-2">
                        <div onclick="selectModel('workshop', 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=800&q=80', this)" class="model-card border border-slate-700 bg-slate-900/60 p-3 rounded-xl cursor-pointer hover:border-slate-500 transition-all">
                            <p class="text-xs font-bold text-white flex items-center gap-2">
                                <i class="fa-solid fa-warehouse text-amber-500 text-xs"></i> Suburban Shop
                            </p>
                        </div>
                        <div onclick="selectModel('ag', 'https://images.unsplash.com/photo-1500937386664-56d1dfef3854?auto=format&fit=crop&w=800&q=80', this)" class="model-card border border-slate-900 bg-slate-900/60 p-3 rounded-xl cursor-pointer hover:border-slate-500 transition-all">
                            <p class="text-xs font-bold text-slate-400 flex items-center gap-2">
                                <i class="fa-solid fa-tractor text-xs"></i> Agricultural Storage
                            </p>
                        </div>
                    </div>
                </div>

                <!-- Envelope Footprint Dimensions -->
                <div>
                    <label class="block text-[11px] font-mono uppercase tracking-widest text-slate-500 mb-2.5 flex justify-between">
                        <span>2. Outer Dimensions</span>
                        <span class="text-slate-400 font-mono" id="sqftDisplay">1200 Sq Ft</span>
                    </label>
                    <div class="grid grid-cols-2 gap-2">
                        <div class="relative">
                            <select id="barnWidth" onchange="updateConfig()" class="w-full appearance-none bg-slate-950 border border-slate-800 rounded-xl px-3 py-2.5 text-xs font-medium text-slate-200 focus:outline-none focus:border-slate-600 cursor-pointer">
                                <option value="24">24' Width Grid</option>
                                <option value="30" selected>30' Width Grid</option>
                                <option value="40">40' Width Grid</option>
                            </select>
                            <i class="fa-solid fa-chevron-down absolute right-3 top-1/2 -translate-y-1/2 text-slate-600 text-[10px] pointer-events-none"></i>
                        </div>
                        <div class="relative">
                            <select id="barnLength" onchange="updateConfig()" class="w-full appearance-none bg-slate-950 border border-slate-800 rounded-xl px-3 py-2.5 text-xs font-medium text-slate-200 focus:outline-none focus:border-slate-600 cursor-pointer">
                                <option value="30">30' Length Footprint</option>
                                <option value="40" selected>40' Length Footprint</option>
                                <option value="60">60' Length Footprint</option>
                            </select>
                            <i class="fa-solid fa-chevron-down absolute right-3 top-1/2 -translate-y-1/2 text-slate-600 text-[10px] pointer-events-none"></i>
                        </div>
                    </div>
                </div>

                <!-- NEW FEATURE: Engineered Roof Pitch Options -->
                <div>
                    <label class="block text-[11px] font-mono uppercase tracking-widest text-slate-500 mb-2.5">3. Engineered Truss Roof Pitch</label>
                    <div class="grid grid-cols-3 gap-2">
                        <label class="flex items-center justify-center py-2.5 bg-slate-950 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer text-xs font-mono text-slate-300">
                            <input type="radio" name="roofPitch" value="3/12" onchange="updateConfig()" class="sr-only peer">
                            <span class="peer-checked:text-amber-500 peer-checked:font-bold">3/12 Low</span>
                        </label>
                        <label class="flex items-center justify-center py-2.5 bg-slate-950 rounded-xl border border-slate-700 hover:border-slate-700 cursor-pointer text-xs font-mono text-slate-300">
                            <input type="radio" name="roofPitch" value="4/12" checked onchange="updateConfig()" class="sr-only peer">
                            <span class="peer-checked:text-amber-500 peer-checked:font-bold">4/12 Std</span>
                        </label>
                        <label class="flex items-center justify-center py-2.5 bg-slate-950 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer text-xs font-mono text-slate-300">
                            <input type="radio" name="roofPitch" value="6/12" onchange="updateConfig()" class="sr-only peer">
                            <span class="peer-checked:text-amber-500 peer-checked:font-bold">6/12 High</span>
                        </label>
                    </div>
                </div>

                <!-- NEW FEATURE: Side Wall Eave Overhang Packages -->
                <div>
                    <label class="block text-[11px] font-mono uppercase tracking-widest text-slate-500 mb-2.5">4. Box Eave Overhang Profiles</label>
                    <div class="grid grid-cols-2 gap-2">
                        <label class="flex items-center gap-3 bg-slate-950 p-3 rounded-xl border border-slate-700 hover:border-slate-600 cursor-pointer">
                            <input type="radio" name="eaveOverhang" value="Flush" checked onchange="updateConfig()" class="w-3.5 h-3.5 text-slate-700 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-slate-300">Standard Flush</p>
                                <p class="text-[10px] text-slate-500">Clean geometric roof edge</p>
                            </div>
                        </label>
                        <label class="flex items-center gap-3 bg-slate-950 p-3 rounded-xl border border-slate-800 hover:border-slate-600 cursor-pointer">
                            <input type="radio" name="eaveOverhang" value="12-Inch" onchange="updateConfig()" class="w-3.5 h-3.5 text-slate-700 accent-amber-500">
                            <div>
                                <p class="text-xs font-bold text-slate-300">12" Box Eaves</p>
                                <p class="text-[10px] text-slate-500">Enhanced weather run-off</p>
                            </div>
                        </label>
                    </div>
                </div>

                <!-- Premium Texture Siding Panels -->
                <div>
                    <label class="block text-[11px] font-mono uppercase tracking-widest text-slate-500 mb-2.5">5. Outer Wall Envelope Steel Texture</label>
                    <div class="grid grid-cols-2 gap-2">
                        <div onclick="selectSiding('#991b1b', this)" class="siding-card border border-slate-700 bg-slate-950 rounded-xl overflow-hidden cursor-pointer shadow-md group relative h-16">
                            <img src="https://images.unsplash.com/photo-1589939705384-5185137a7f0f?auto=format&fit=crop&w=250&q=80" class="absolute inset-0 w-full h-full object-cover opacity-20 transition-all" alt="Crimson Texture">
                            <div class="absolute inset-0 bg-slate-950/40 p-3 flex items-center justify-between">
                                <span class="text-xs font-bold text-white">Crimson Red Steel</span>
                                <span class="text-[9px] font-mono bg-slate-900 px-1.5 py-0.5 rounded text-slate-400">29-Ga.</span>
                            </div>
                        </div>
                        <div onclick="selectSiding('#1e293b', this)" class="siding-card border border-slate-900 bg-slate-950 rounded-xl overflow-hidden cursor-pointer shadow-md group relative h-16">
                            <img src="https://images.unsplash.com/photo-1531834685988-c388d2c019ec?auto=format&fit=crop&w=250&q=80" class="absolute inset-0 w-full h-full object-cover opacity-20 transition-all" alt="Slate Texture">
                            <div class="absolute inset-0 bg-slate-950/40 p-3 flex items-center justify-between">
                                <span class="text-xs font-bold text-slate-400">Charcoal Slate</span>
                                <span class="text-[9px] font-mono bg-slate-900 px-1.5 py-0.5 rounded text-slate-500">29-Ga.</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Window Assemblies and Roll-Up Access Panels -->
                <div>
                    <label class="block text-[11px] font-mono uppercase tracking-widest text-slate-500 mb-2.5">6. Structural Openings</label>
                    <div class="grid grid-cols-2 gap-2">
                        <label class="flex items-center justify-between bg-slate-950 p-3 rounded-xl border border-slate-800 hover:border-slate-700 cursor-pointer transition-all">
                            <div class="flex items-center gap-2">
                                <input type="checkbox" id="addWindows" onchange="updateConfig()" class="w-3.5 h-3.5 accent-slate-400">
                                <span class="text-xs text-slate-300">Window Grid (x3)</span>
                            </div>
                            <span class="text-[10px] font-mono text-slate-500">+$1,150</span>
                        </label>
                        <label class="flex items-center justify-between bg-slate-950 p-3 rounded-xl border border-slate-700 hover:border-slate-700 cursor-pointer transition-all">
                            <div class="flex items-center gap-2">
                                <input type="checkbox" id="addDoor" checked onchange="updateConfig()" class="w-3.5 h-3.5 accent-slate-400">
                                <span class="text-xs text-slate-300">12x12 Sectional</span>
                            </div>
                            <span class="text-[10px] font-mono text-slate-500">+$1,200</span>
                        </label>
                    </div>
                </div>

                <button onclick="switchTab('quote')" class="w-full bg-slate-100 hover:bg-white text-slate-950 font-bold text-xs uppercase tracking-wider py-3.5 rounded-xl transition-all shadow-xl">
                    Proceed to Engineering Analysis
                </button>
            </div>

            <!-- Panel 2: Polished Professional Request Board (No High-Pressure Phrasing) -->
            <div id="panel-quote" class="bg-slate-900/30 border border-slate-900 p-6 rounded-2xl space-y-4 hidden">
                <div class="pb-2 border-b border-slate-900">
                    <h3 class="text-md font-bold tracking-tight text-white">Request Official Bill of Materials</h3>
                    <p class="text-xs text-slate-400 mt-1">Submit your specification data layer below. An engineering representative will review structural snow/wind loading metrics for your municipality.</p>
                </div>

                <form id="leadForm" onsubmit="handleLeadSubmit(event)" class="space-y-3">
                    <div>
                        <label class="block text-[10px] font-mono uppercase tracking-wider text-slate-500 mb-1">Point of Contact Name</label>
                        <input type="text" required placeholder="Full Structural Owner Name" class="w-full bg-slate-950 border border-slate-900 rounded-xl px-3 py-2.5 text-xs text-white placeholder-slate-700 focus:outline-none focus:borde
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
