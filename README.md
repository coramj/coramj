<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pablo Mascaró | Portfolio</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Iconos Lucide -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <style>
        /* --- ESTILOS BASE --- */
        html { scrollbar-gutter: stable; } /* Evita el salto lateral */
        body {
            background-color: #050505;
            color: #e2e8f0;
            font-family: system-ui, -apple-system, sans-serif;
            margin: 0;
            overflow-x: hidden;
        }

        /* --- FONDO ANIMADO --- */
        .blob {
            position: absolute; filter: blur(90px); opacity: 0.4; border-radius: 50%; z-index: -1;
            animation: float-blob 20s infinite alternate cubic-bezier(0.4, 0, 0.2, 1);
        }
        .blob-1 { top: 0; left: 0; width: 60vw; height: 60vw; background: #4f46e5; animation-delay: 0s; }
        .blob-2 { bottom: 0; right: 0; width: 60vw; height: 60vw; background: #059669; animation-delay: -5s; }
        .blob-3 { bottom: 20%; left: 20%; width: 40vw; height: 40vw; background: #7c3aed; animation-delay: -10s; opacity: 0.2; }

        @keyframes float-blob {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(50px, 50px) scale(1.1); }
        }

        .bg-grid {
            position: absolute; inset: 0; z-index: -1;
            background-image: linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
            linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
            background-size: 40px 40px;
        }

        /* --- TARJETAS --- */
        .glass-panel {
            background: rgba(20, 20, 20, 0.6);
            backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        }

        .card {
            background: rgba(20, 20, 20, 0.6);
            backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 1.5rem; padding: 1.5rem;
            cursor: pointer; position: relative; overflow: hidden;
            display: flex; flex-direction: column;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .card:hover {
            background: rgba(255, 255, 255, 0.08);
            transform: translateY(-6px) scale(1.01);
            border-color: rgba(255, 255, 255, 0.2);
            box-shadow: 0 20px 40px -5px rgba(0,0,0,0.4);
        }

        .card-icon-float {
            position: absolute; top: 1.5rem; right: 1.5rem;
            opacity: 0; transform: scale(0.8); transition: all 0.3s ease;
            color: rgba(255,255,255,0.5);
        }
        .card:hover .card-icon-float { opacity: 1; transform: scale(1); }

        /* --- UI ELEMENTS --- */
        .progress-bg { background: rgba(255,255,255,0.1); height: 6px; border-radius: 99px; overflow: hidden; width: 100%; margin-top:4px;}
        .progress-fill { height: 100%; border-radius: 99px; width: 0%; transition: width 1.5s ease-out; }
        .loaded .progress-fill { width: var(--w); }

        /* Slider Idioma */
        .lang-switch {
            position: fixed; top: 1.5rem; right: 1.5rem; z-index: 50;
            background: rgba(0,0,0,0.6); backdrop-filter: blur(12px);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 99px; padding: 4px; display: flex;
        }
        .lang-btn {
            background: none; border: none; padding: 8px 12px;
            cursor: pointer; position: relative; z-index: 10;
            display: flex; align-items: center; justify-content: center; width: 44px;
        }
        .lang-pill {
            position: absolute; top: 4px; bottom: 4px; left: 4px; width: 44px;
            background: #4f46e5; border-radius: 99px;
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1); z-index: 0;
        }

        /* Menu */
        .menu-btn {
            position: fixed; top: 1.5rem; left: 1.5rem; z-index: 50;
            width: 3rem; height: 3rem; border-radius: 0.75rem;
            background: rgba(0,0,0,0.6); border: 1px solid rgba(255,255,255,0.1);
            color: white; display: flex; align-items: center; justify-content: center;
            cursor: pointer; backdrop-filter: blur(10px); transition: transform 0.2s;
        }
        .menu-btn:active { transform: scale(0.95); }
        .menu-list {
            position: fixed; top: 5rem; left: 1.5rem; z-index: 49;
            background: rgba(20,20,20,0.95); border: 1px solid rgba(255,255,255,0.1);
            border-radius: 1rem; padding: 0.5rem; width: 200px;
            display: none; flex-direction: column; backdrop-filter: blur(12px);
            animation: fadeIn 0.2s ease-out;
        }
        .menu-list.show { display: flex; }
        .menu-item {
            padding: 10px 15px; color: #cbd5e1; text-decoration: none;
            display: flex; align-items: center; gap: 10px; font-size: 14px;
            border-radius: 8px; transition: background 0.2s; cursor: pointer;
        }
        .menu-item:hover { background: rgba(255,255,255,0.1); color: white; }

        /* Modales */
        .modal-overlay {
            position: fixed; inset: 0; background: rgba(0,0,0,0.85);
            backdrop-filter: blur(8px); z-index: 100;
            display: none; align-items: center; justify-content: center; padding: 20px;
            opacity: 0; transition: opacity 0.3s ease;
        }
        .modal-overlay.open { display: flex; opacity: 1; }
        .modal-content {
            background: #111; border: 1px solid rgba(255,255,255,0.1);
            border-radius: 24px; max-width: 700px; width: 100%;
            max-height: 90vh; overflow-y: auto; position: relative;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
            transform: translateY(20px); transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .modal-overlay.open .modal-content { transform: translateY(0); }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #0a0a0a; }
        ::-webkit-scrollbar-thumb { background: #333; border-radius: 4px; }
        body.modal-open { overflow: hidden; }
        
        .animate-enter { animation: slideUpFade 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards; opacity: 0; }
        @keyframes slideUpFade { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
        
        .delay-100 { animation-delay: 100ms; }
        .delay-200 { animation-delay: 200ms; }
        .delay-300 { animation-delay: 300ms; }
    </style>
</head>

<body class="antialiased selection:bg-indigo-500/30 selection:text-white">

    <!-- FONDO -->
    <div class="fixed inset-0 z-0 overflow-hidden">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
        <div class="blob blob-3"></div>
        <div class="bg-grid"></div>
        <div style="position: absolute; inset:0; background: radial-gradient(circle at center, transparent 0%, #050505 100%);"></div>
    </div>

    <!-- SLIDER IDIOMA -->
    <div class="lang-switch">
        <div id="lang-pill" class="lang-pill"></div>
        <button class="lang-btn active" onclick="setLang('es', 0)" aria-label="Español">
            <svg width="20" height="14" viewBox="0 0 20 14" fill="none"><rect width="20" height="14" fill="#AA151B"/><rect y="3.5" width="20" height="7" fill="#F1BF00"/></svg>
        </button>
        <button class="lang-btn" onclick="setLang('ca', 1)" aria-label="Català">
            <svg width="20" height="14" viewBox="0 0 20 14" fill="none"><rect width="20" height="14" fill="#F1BF00"/><path d="M0 2H20M0 5H20M0 8H20M0 11H20" stroke="#AA151B" stroke-width="2"/></svg>
        </button>
        <button class="lang-btn" onclick="setLang('en', 2)" aria-label="English">
            <svg width="20" height="14" viewBox="0 0 20 14" fill="none"><rect width="20" height="14" fill="#012169"/><path d="M0 0L20 14M20 0L0 14" stroke="white" stroke-width="2"/><path d="M10 0V14M0 7H20" stroke="white" stroke-width="3"/><path d="M10 0V14M0 7H20" stroke="#C8102E" stroke-width="2"/></svg>
        </button>
    </div>

    <!-- MENÚ -->
    <button class="menu-btn" onclick="toggleMenu()">
        <i data-lucide="terminal"></i>
    </button>
    <div id="menu-list" class="menu-list">
        <div class="menu-item" onclick="window.scrollTo({top:0, behavior:'smooth'}); toggleMenu()"><i data-lucide="arrow-up" size="16"></i> <span data-i18n="menu_home">Inicio</span></div>
        <div class="menu-item" onclick="openBlogOverlay(); toggleMenu()"><i data-lucide="layout" size="16"></i> <span data-i18n="menu_blog">Bitácora</span></div>
        <div class="menu-item" onclick="document.getElementById('contact').scrollIntoView({behavior:'smooth'}); toggleMenu()"><i data-lucide="mail" size="16"></i> <span data-i18n="menu_contact">Contacto</span></div>
        <div style="height:1px; background:rgba(255,255,255,0.1); margin: 5px 0;"></div>
        <div class="menu-item" onclick="window.print()" style="color: #818cf8;"><i data-lucide="printer" size="16"></i> <span data-i18n="menu_print">Imprimir CV</span></div>
    </div>

    <!-- CONTENIDO -->
    <div class="relative z-10 max-w-6xl mx-auto p-6 pt-24 pb-24" id="main-content">
        
        <header class="mb-16 animate-enter">
            <div class="inline-flex items-center gap-2 px-3 py-1 bg-white/10 border border-white/10 text-slate-300 text-[10px] font-bold tracking-widest mb-6 rounded-full">
                <span class="w-2 h-2 bg-emerald-500 rounded-full animate-pulse"></span> 
                <span data-i18n="status">DISPONIBLE JUNIO 2026</span>
            </div>
            <h1 class="text-5xl md:text-7xl font-bold text-white mb-6 tracking-tight">Pablo Mascaró</h1>
            <p class="text-xl text-slate-400 font-light max-w-2xl mb-8">
                <span data-i18n="role">Estudiante de Bachillerato Tecnológico.</span><br>
                <span class="text-white font-medium border-b border-indigo-500">Hardware</span>, 
                <span class="text-white font-medium border-b border-emerald-500" data-i18n="role_sys">Sistemas</span>
                <span data-i18n="role_end">y Reparaciones.</span>
            </p>
            <div class="flex flex-wrap gap-4">
                <a href="tel:683443335" class="bg-white text-black px-6 py-3 rounded-xl font-medium hover:bg-slate-200 transition-colors flex items-center gap-2 shadow-lg hover:scale-105 transform duration-200">
                    <i data-lucide="phone" size="18"></i> 683 443 335
                </a>
                <a href="mailto:pau14mascaro@gmail.com" class="bg-white/5 border border-white/10 text-white px-6 py-3 rounded-xl font-medium hover:bg-white/10 transition-colors flex items-center gap-2 hover:scale-105 transform duration-200">
                    <i data-lucide="mail" size="18"></i> Email
                </a>
            </div>
        </header>

        <!-- GRID -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

            <!-- 1. HARDWARE -->
            <div class="card lg:col-span-2 animate-enter delay-100 group" onclick="openModal('hw')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-4 mb-6">
                    <div class="p-3 bg-indigo-500/20 rounded-xl text-indigo-400 group-hover:bg-indigo-500/30 transition-colors"><i data-lucide="cpu" size="24"></i></div>
                    <h2 class="text-2xl font-bold text-white" data-i18n="hw_title">Ingeniería de Hardware</h2>
                </div>
                <div class="grid sm:grid-cols-2 gap-6">
                    <div>
                        <h4 class="text-xs font-bold text-slate-500 uppercase mb-3 flex items-center gap-2"><i data-lucide="wrench" size="14"></i> <span data-i18n="hw_sub1">Montaje</span></h4>
                        <ul class="text-sm text-slate-300 space-y-2">
                            <li class="flex gap-2"><span class="text-indigo-500">•</span> Gaming & Workstation Custom</li>
                            <li class="flex gap-2"><span class="text-indigo-500">•</span> Gestión de Cableado</li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="text-xs font-bold text-slate-500 uppercase mb-3 flex items-center gap-2"><i data-lucide="activity" size="14"></i> <span data-i18n="hw_sub2">Diagnóstico</span></h4>
                        <ul class="text-sm text-slate-300 space-y-2">
                            <li class="flex gap-2"><span class="text-yellow-500">•</span> Detección de fallos críticos</li>
                            <li class="flex gap-2"><span class="text-yellow-500">•</span> Mantenimiento Térmico</li>
                        </ul>
                    </div>
                </div>
            </div>

            <!-- 2. STACK -->
            <div class="card animate-enter delay-200" onclick="openModal('stack')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-6">
                    <div class="p-2 bg-blue-500/20 rounded-lg text-blue-400"><i data-lucide="code-2" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="stack_title">Stack</h2>
                </div>
                <div class="space-y-4">
                    <div><div class="flex justify-between text-xs text-slate-300 mb-1"><span>HTML/CSS</span><span>75%</span></div><div class="progress-bg"><div class="progress-fill bg-blue-500" style="--w: 75%"></div></div></div>
                    <div><div class="flex justify-between text-xs text-slate-300 mb-1"><span>JavaScript</span><span>60%</span></div><div class="progress-bg"><div class="progress-fill bg-blue-400" style="--w: 60%"></div></div></div>
                    <div><div class="flex justify-between text-xs text-slate-300 mb-1"><span>React (Básico)</span><span>40%</span></div><div class="progress-bg"><div class="progress-fill bg-indigo-400" style="--w: 40%"></div></div></div>
                </div>
            </div>

            <!-- 3. EXPERIENCIA -->
            <div class="card animate-enter delay-300" onclick="openModal('exp')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-emerald-500/20 rounded-lg text-emerald-400"><i data-lucide="briefcase" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="exp_title">Experiencia</h2>
                </div>
                <div class="mt-auto p-3 bg-white/5 rounded-xl border border-white/5">
                    <div class="font-bold text-white text-sm">Hotel Gemini</div>
                    <div class="text-xs text-emerald-400" data-i18n="exp_role">Camarero Desayunos</div>
                    <div class="text-[10px] text-slate-500 mt-1">2025 • Menorca</div>
                </div>
            </div>

            <!-- 4. IDIOMAS -->
            <div class="card animate-enter delay-100" onclick="openModal('lang')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-pink-500/20 rounded-lg text-pink-400"><i data-lucide="languages" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="lang_title">Idiomas</h2>
                </div>
                <div class="space-y-2 mt-auto text-sm">
                    <div class="flex justify-between p-2 rounded bg-white/5 border border-white/5"><span class="text-slate-300" data-i18n="lang_es">Español</span> <span class="text-pink-300 text-[10px] font-bold bg-pink-500/20 px-2 py-0.5 rounded" data-i18n="lang_native">NATIVO</span></div>
                    <div class="flex justify-between p-2 rounded bg-white/5 border border-white/5"><span class="text-slate-300" data-i18n="lang_ca">Catalán</span> <span class="text-pink-300 text-[10px] font-bold bg-pink-500/20 px-2 py-0.5 rounded" data-i18n="lang_native">NATIVO</span></div>
                    <div class="flex justify-between p-2 rounded bg-white/5 border border-white/5"><span class="text-slate-300" data-i18n="lang_en">Inglés</span> <span class="text-white text-[10px] font-bold">B2 / C1</span></div>
                </div>
            </div>

            <!-- 5. SETUP -->
            <div class="card animate-enter delay-200" onclick="openModal('setup')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-cyan-500/20 rounded-lg text-cyan-400"><i data-lucide="monitor" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="setup.title">Setup</h2>
                </div>
                <div class="mt-auto grid grid-cols-2 gap-2 text-center">
                    <div class="bg-black/40 p-2 rounded border border-white/5"><i data-lucide="cpu" class="w-4 h-4 text-cyan-400 mx-auto mb-1"></i><div class="text-[10px] text-slate-300">i5 14600KF</div></div>
                    <div class="bg-black/40 p-2 rounded border border-white/5"><i data-lucide="zap" class="w-4 h-4 text-green-400 mx-auto mb-1"></i><div class="text-[10px] text-slate-300">RTX 5060</div></div>
                </div>
            </div>

            <!-- 6. FORMACIÓN -->
            <div class="card lg:col-span-2 animate-enter delay-300" onclick="openModal('edu')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-4 mb-4">
                    <div class="p-2 bg-purple-500/20 rounded-lg text-purple-400"><i data-lucide="graduation-cap" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="edu_title">Formación</h2>
                </div>
                <div class="grid sm:grid-cols-2 gap-8 mt-2">
                    <div class="pl-4 border-l-2 border-purple-500">
                        <div class="text-[10px] font-bold text-purple-400 mb-1" data-i18n="edu_curr">ACTUALIDAD</div>
                        <div class="font-bold text-white text-sm" data-i18n="edu_deg1">Bachillerato Tecnológico</div>
                        <div class="text-xs text-slate-400 mt-0.5">IES Maria Àngels Cardona</div>
                    </div>
                    <div class="pl-4 border-l-2 border-slate-700">
                        <div class="text-[10px] font-bold text-slate-500 mb-1">2020 - 2024</div>
                        <div class="font-bold text-slate-300 text-sm">ESO</div>
                        <div class="text-xs text-slate-500">IES Maria Àngels Cardona</div>
                    </div>
                </div>
            </div>

            <!-- 7. CONTACTO -->
            <div id="contact" class="card animate-enter delay-100 hover:border-orange-500/50" onclick="openModal('contact')">
                <div class="flex items-center gap-3 mb-2">
                    <div class="p-2 bg-teal-500/20 rounded-lg text-teal-400"><i data-lucide="at-sign" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="contact_title">Contacto</h2>
                </div>
                <div class="mt-auto text-sm text-slate-400 break-all space-y-1">
                    <div class="flex items-center gap-2"><i data-lucide="mail" class="w-3 h-3"></i> pau14mascaro@...</div>
                    <div class="flex items-center gap-2 text-pink-400"><i data-lucide="instagram" class="w-3 h-3"></i> @pb.mojz_</div>
                </div>
            </div>

            <!-- 8. UBICACIÓN -->
            <div class="card animate-enter delay-200" onclick="openModal('location')">
                <div class="flex items-center gap-3 mb-2">
                    <div class="p-2 bg-orange-500/20 rounded-lg text-orange-400"><i data-lucide="map-pin" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="loc_title">Ubicación</h2>
                </div>
                <div class="mt-auto text-center">
                    <div class="font-bold text-white text-sm">Son Carrió</div>
                    <div class="text-[10px] text-orange-300/80 mt-1" data-i18n="loc_sub">Movilidad Propia</div>
                </div>
            </div>

            <!-- 9. BITÁCORA -->
            <div onclick="openBlogOverlay()" class="card lg:col-span-2 animate-enter delay-300 hover:border-gray-500/50">
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-gray-500/20 rounded-lg text-gray-400"><i data-lucide="terminal" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="blog_title">Bitácora</h2>
                </div>
                <div class="mt-auto p-4 rounded-xl border border-white/5 bg-black/20 group-hover:bg-black/40 transition-colors">
                    <div class="flex justify-between text-xs mb-1">
                        <span class="text-emerald-400 font-mono">LATEST LOG</span>
                        <span class="text-slate-500">27 JAN 2026</span>
                    </div>
                    <h3 class="text-white font-bold text-sm">Portfolio Final Release</h3>
                </div>
            </div>
        </div>
        
        <footer id="contact-footer" class="text-center text-[10px] font-mono text-slate-600 pb-12">SYSTEM ONLINE · CIUTADELLA</footer>
    </div>

    <!-- MODAL -->
    <div id="modal-overlay" class="modal-overlay" onclick="closeModal()">
        <div class="modal-content" onclick="event.stopPropagation()">
            <div class="p-8">
                <div class="flex justify-between items-center mb-6">
                    <div class="flex items-center gap-4">
                        <div id="modal-icon-wrapper" class="w-10 h-10 rounded-xl flex items-center justify-center bg-white/10"><i id="modal-icon" data-lucide="info"></i></div>
                        <h2 id="modal-title" class="text-2xl font-bold text-white">Título</h2>
                    </div>
                    <button onclick="closeModal()" class="text-slate-400 hover:text-white"><i data-lucide="x"></i></button>
                </div>
                <div id="modal-body" class="text-slate-300 leading-relaxed text-sm space-y-4"></div>
            </div>
        </div>
    </div>

    <!-- BLOG OVERLAY -->
    <div id="blog-overlay" class="fixed inset-0 z-[100] bg-[#050505] hidden flex-col">
        <div class="fixed top-0 left-0 right-0 p-6 flex justify-between items-center z-10 bg-[#050505]/90 backdrop-blur-md border-b border-white/10">
            <h2 class="text-2xl font-bold text-white flex items-center gap-3"><i data-lucide="terminal" class="text-emerald-500"></i> <span data-i18n="blog_title">Bitácora</span></h2>
            <button onclick="closeBlogOverlay()" class="w-10 h-10 rounded-full bg-white/10 flex items-center justify-center text-white hover:bg-white/20"><i data-lucide="x" class="w-5 h-5"></i></button>
        </div>
        <div class="overflow-y-auto pt-24 pb-12 px-6">
            <div class="max-w-3xl mx-auto space-y-8" id="blog-container"></div>
        </div>
    </div>

    <script>
        lucide.createIcons();
        window.onload = () => { document.body.classList.add('loaded'); lucide.createIcons(); };

        const texts = {
            es: { status: "DISPONIBLE JUNIO 2026", role: "Estudiante de Bachillerato Tecnológico.", role_sys: "Sistemas", role_end: "y Reparaciones.", hw_title: "Ingeniería de Hardware", hw_sub1: "Montaje", hw_sub2: "Diagnóstico", exp_title: "Experiencia", exp_role: "Camarero Desayunos", stack_title: "Conocimientos", lang_title: "Idiomas", lang_es: "Español", lang_ca: "Catalán", lang_en: "Inglés", lang_native: "NATIVO", setup_title: "Setup", edu_title: "Formación", edu_curr: "ACTUALIDAD", edu_deg1: "Bachillerato Tecnológico", contact_title: "Contacto", loc_title: "Ubicación", loc_sub: "Movilidad Propia", blog_title: "Bitácora", menu_home: "Inicio", menu_blog: "Bitácora", menu_contact: "Contacto", menu_print: "Imprimir CV" },
            ca: { status: "DISPONIBLE JUNY 2026", role: "Estudiant de Batxillerat Tecnològic.", role_sys: "Sistemes", role_end: "i Reparacions.", hw_title: "Enginyeria de Hardware", hw_sub1: "Muntatge", hw_sub2: "Diagnòstic", exp_title: "Experiència", exp_role: "Cambrer Esmorzars", stack_title: "Coneixements", lang_title: "Idiomes", lang_es: "Espanyol", lang_ca: "Català", lang_en: "Anglès", lang_native: "NATIU", setup_title: "Setup", edu_title: "Formació", edu_curr: "ACTUALITAT", edu_deg1: "Batxillerat Tecnològic", contact_title: "Contacte", loc_title: "Ubicació", loc_sub: "Mobilitat Pròpia", blog_title: "Bitàcola", menu_home: "Inici", menu_blog: "Bitàcola", menu_contact: "Contacte", menu_print: "Imprimir CV" },
            en: { status: "AVAILABLE JUNE 2026", role: "Tech High School Student.", role_sys: "Systems", role_end: "and Repairs.", hw_title: "Hardware Engineering", hw_sub1: "Assembly", hw_sub2: "Diagnostics", exp_title: "Experience", exp_role: "Breakfast Waiter", stack_title: "Knowledge", lang_title: "Languages", lang_es: "Spanish", lang_ca: "Catalan", lang_en: "English", lang_native: "NATIVE", setup_title: "Setup", edu_title: "Education", edu_curr: "PRESENT", edu_deg1: "Tech Baccalaureate", contact_title: "Contact", loc_title: "Location", loc_sub: "Own Vehicle", blog_title: "Dev Logs", menu_home: "Home", menu_blog: "Logs", menu_contact: "Contact", menu_print: "Print CV" }
        };

        const cardsData = {
            hw: { color: 'indigo', icon: 'cpu', es: `
                <div class="grid gap-4"><div class="p-4 bg-white/5 rounded-xl border border-white/10"><h4 class="text-indigo-400 font-bold mb-2 flex items-center gap-2"><i data-lucide="wrench" class="w-4 h-4"></i> Montaje a Medida</h4><p class="text-slate-400 text-sm">Planificación y ensamblaje de equipos desde cero evitando cuellos de botella.</p></div><div class="p-4 bg-white/5 rounded-xl border border-white/10"><h4 class="text-indigo-400 font-bold mb-2 flex items-center gap-2"><i data-lucide="zap" class="w-4 h-4"></i> Mantenimiento</h4><p class="text-slate-400 text-sm">Limpieza profunda, cambio de pasta térmica (Arctic/Noctua) y diagnóstico.</p></div></div>`, ca: `<p>Muntatge i reparació.</p>`, en: `<p>Assembly and repair.</p>` },
            exp: { color: 'emerald', icon: 'briefcase', es: `<ul class="space-y-3"><li><strong>Responsable de Desayunos</strong> (Hotel Gemini)<br>Gestión completa del servicio y atención a clientes internacionales.</li></ul>`, ca: `<p>Hotel Gemini.</p>`, en: `<p>Hotel Gemini.</p>` },
            stack: { color: 'blue', icon: 'code-2', es: `<p><strong>Frontend:</strong> HTML5, CSS3, JS, React.<br><strong>Backend/Sys:</strong> Java, C++, Linux (Mint/Ubuntu).</p>`, ca: `<p>Stack tecnològic.</p>`, en: `<p>Tech stack.</p>` },
            lang: { color: 'pink', icon: 'languages', es: `<p>• Español/Catalán: Nativo<br>• Inglés: B2/C1 (Fluido)</p>`, ca: `<p>Idiomes.</p>`, en: `<p>Languages.</p>` },
            setup: { color: 'cyan', icon: 'monitor', es: `<p><strong>PC:</strong> i5 14600KF + RTX 5060 + 32GB DDR5.<br><strong>Periféricos:</strong> Razer Huntsman Mini + Basilisk V3.</p>`, ca: `<p>Setup.</p>`, en: `<p>Setup.</p>` },
            edu: { color: 'purple', icon: 'graduation-cap', es: `<p><strong>Bachillerato Tecnológico</strong> (2024-Presente)<br>IES Maria Àngels Cardona.<br><br><strong>ESO</strong> (2020-2024)</p>`, ca: `<p>Educació.</p>`, en: `<p>Education.</p>` },
            contact: { color: 'teal', icon: 'at-sign', es: `<p>Tel: 683 443 335<br>Email: pau14mascaro@gmail.com</p>`, ca: `<p>Contacte.</p>`, en: `<p>Contact.</p>` },
            location: { color: 'orange', icon: 'map-pin', es: `<p>Son Carrió, Menorca.<br>Vehículo propio disponible.</p>`, ca: `<p>Ubicació.</p>`, en: `<p>Location.</p>` }
        };

        let currentLang = 'es';

        function setLang(lang, index) {
            currentLang = lang;
            document.querySelectorAll('[data-i18n]').forEach(el => { const key = el.getAttribute('data-i18n'); if (texts[lang][key]) el.innerText = texts[lang][key]; });
            document.getElementById('lang-pill').style.transform = `translateX(${index * 100}%)`;
            document.querySelectorAll('.lang-btn').forEach((btn, i) => { btn.style.color = i === index ? 'white' : '#64748b'; });
        }

        function openModal(id) {
            const data = cardsData[id] || { es: "Info...", color: 'gray', icon: 'info' };
            const titleKeys = { 'hw': 'hw_title', 'exp': 'exp_title', 'stack': 'stack_title', 'lang': 'lang_title', 'setup': 'setup_title', 'edu': 'edu_title', 'contact': 'contact_title', 'location': 'loc_title' };
            document.getElementById('modal-title').innerText = texts[currentLang][titleKeys[id]] || "Detalle";
            document.getElementById('modal-body').innerHTML = data[currentLang] || data['es'];
            
            const wrapper = document.getElementById('modal-icon-wrapper');
            wrapper.className = `w-10 h-10 rounded-xl flex items-center justify-center bg-${data.color}-500/20 text-${data.color}-400`;
            document.getElementById('modal-icon').setAttribute('data-lucide', data.icon);
            lucide.createIcons();

            const overlay = document.getElementById('modal-overlay');
            overlay.classList.remove('hidden');
            overlay.classList.add('flex');
            setTimeout(() => overlay.classList.add('open'), 10);
            document.body.classList.add('modal-open');
        }

        function closeModal() {
            const overlay = document.getElementById('modal-overlay');
            overlay.classList.remove('open');
            setTimeout(() => { overlay.classList.add('hidden'); overlay.classList.remove('flex'); document.body.classList.remove('modal-open'); }, 300);
        }

        function toggleMenu() { document.getElementById('menu-list').classList.toggle('show'); }

        // Blog
        function openBlogOverlay() {
            const posts = [{ title: "Portfolio Final", date: "27/01/2026", text: "Versión final optimizada." }, { title: "RTX 5060", date: "15/01/2026", text: "Benchmark térmico." }];
            document.getElementById('blog-container').innerHTML = posts.map(p => `<article class="p-6 rounded-2xl border border-white/10 bg-white/5"><div class="text-emerald-400 text-xs font-mono mb-2">${p.date}</div><h3 class="text-2xl font-bold text-white mb-2">${p.title}</h3><p class="text-slate-400">${p.text}</p></article>`).join('');
            document.getElementById('blog-overlay').classList.remove('hidden');
            document.getElementById('blog-overlay').classList.add('flex');
            document.body.classList.add('modal-open');
        }
        function closeBlogOverlay() { document.getElementById('blog-overlay').classList.add('hidden'); document.getElementById('blog-overlay').classList.remove('flex'); document.body.classList.remove('modal-open'); }
        function triggerEasterEgg() { alert("👾: Beep!"); }
        document.addEventListener('keydown', (e) => { if (e.key === 'Escape') { closeModal(); closeBlogOverlay(); } });
        setTimeout(() => { const active = document.querySelector('.lang-btn.active'); if(active) setLang('es', 0); }, 100);
    </script>
</body>
</html><!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pablo Mascaró | Portfolio</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Iconos Lucide -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <style>
        /* --- ESTILOS BASE --- */
        html { scrollbar-gutter: stable; } /* Evita el salto lateral */
        body {
            background-color: #050505;
            color: #e2e8f0;
            font-family: system-ui, -apple-system, sans-serif;
            margin: 0;
            overflow-x: hidden;
        }

        /* --- FONDO ANIMADO --- */
        .blob {
            position: absolute; filter: blur(90px); opacity: 0.4; border-radius: 50%; z-index: -1;
            animation: float-blob 20s infinite alternate cubic-bezier(0.4, 0, 0.2, 1);
        }
        .blob-1 { top: 0; left: 0; width: 60vw; height: 60vw; background: #4f46e5; animation-delay: 0s; }
        .blob-2 { bottom: 0; right: 0; width: 60vw; height: 60vw; background: #059669; animation-delay: -5s; }
        .blob-3 { bottom: 20%; left: 20%; width: 40vw; height: 40vw; background: #7c3aed; animation-delay: -10s; opacity: 0.2; }

        @keyframes float-blob {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(50px, 50px) scale(1.1); }
        }

        .bg-grid {
            position: absolute; inset: 0; z-index: -1;
            background-image: linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
            linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
            background-size: 40px 40px;
        }

        /* --- TARJETAS --- */
        .glass-panel {
            background: rgba(20, 20, 20, 0.6);
            backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
        }

        .card {
            background: rgba(20, 20, 20, 0.6);
            backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 1.5rem; padding: 1.5rem;
            cursor: pointer; position: relative; overflow: hidden;
            display: flex; flex-direction: column;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .card:hover {
            background: rgba(255, 255, 255, 0.08);
            transform: translateY(-6px) scale(1.01);
            border-color: rgba(255, 255, 255, 0.2);
            box-shadow: 0 20px 40px -5px rgba(0,0,0,0.4);
        }

        .card-icon-float {
            position: absolute; top: 1.5rem; right: 1.5rem;
            opacity: 0; transform: scale(0.8); transition: all 0.3s ease;
            color: rgba(255,255,255,0.5);
        }
        .card:hover .card-icon-float { opacity: 1; transform: scale(1); }

        /* --- UI ELEMENTS --- */
        .progress-bg { background: rgba(255,255,255,0.1); height: 6px; border-radius: 99px; overflow: hidden; width: 100%; margin-top:4px;}
        .progress-fill { height: 100%; border-radius: 99px; width: 0%; transition: width 1.5s ease-out; }
        .loaded .progress-fill { width: var(--w); }

        /* Slider Idioma */
        .lang-switch {
            position: fixed; top: 1.5rem; right: 1.5rem; z-index: 50;
            background: rgba(0,0,0,0.6); backdrop-filter: blur(12px);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 99px; padding: 4px; display: flex;
        }
        .lang-btn {
            background: none; border: none; padding: 8px 12px;
            cursor: pointer; position: relative; z-index: 10;
            display: flex; align-items: center; justify-content: center; width: 44px;
        }
        .lang-pill {
            position: absolute; top: 4px; bottom: 4px; left: 4px; width: 44px;
            background: #4f46e5; border-radius: 99px;
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1); z-index: 0;
        }

        /* Menu */
        .menu-btn {
            position: fixed; top: 1.5rem; left: 1.5rem; z-index: 50;
            width: 3rem; height: 3rem; border-radius: 0.75rem;
            background: rgba(0,0,0,0.6); border: 1px solid rgba(255,255,255,0.1);
            color: white; display: flex; align-items: center; justify-content: center;
            cursor: pointer; backdrop-filter: blur(10px); transition: transform 0.2s;
        }
        .menu-btn:active { transform: scale(0.95); }
        .menu-list {
            position: fixed; top: 5rem; left: 1.5rem; z-index: 49;
            background: rgba(20,20,20,0.95); border: 1px solid rgba(255,255,255,0.1);
            border-radius: 1rem; padding: 0.5rem; width: 200px;
            display: none; flex-direction: column; backdrop-filter: blur(12px);
            animation: fadeIn 0.2s ease-out;
        }
        .menu-list.show { display: flex; }
        .menu-item {
            padding: 10px 15px; color: #cbd5e1; text-decoration: none;
            display: flex; align-items: center; gap: 10px; font-size: 14px;
            border-radius: 8px; transition: background 0.2s; cursor: pointer;
        }
        .menu-item:hover { background: rgba(255,255,255,0.1); color: white; }

        /* Modales */
        .modal-overlay {
            position: fixed; inset: 0; background: rgba(0,0,0,0.85);
            backdrop-filter: blur(8px); z-index: 100;
            display: none; align-items: center; justify-content: center; padding: 20px;
            opacity: 0; transition: opacity 0.3s ease;
        }
        .modal-overlay.open { display: flex; opacity: 1; }
        .modal-content {
            background: #111; border: 1px solid rgba(255,255,255,0.1);
            border-radius: 24px; max-width: 700px; width: 100%;
            max-height: 90vh; overflow-y: auto; position: relative;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.7);
            transform: translateY(20px); transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .modal-overlay.open .modal-content { transform: translateY(0); }

        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #0a0a0a; }
        ::-webkit-scrollbar-thumb { background: #333; border-radius: 4px; }
        body.modal-open { overflow: hidden; }
        
        .animate-enter { animation: slideUpFade 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards; opacity: 0; }
        @keyframes slideUpFade { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
        
        .delay-100 { animation-delay: 100ms; }
        .delay-200 { animation-delay: 200ms; }
        .delay-300 { animation-delay: 300ms; }
    </style>
</head>

<body class="antialiased selection:bg-indigo-500/30 selection:text-white">

    <!-- FONDO -->
    <div class="fixed inset-0 z-0 overflow-hidden">
        <div class="blob blob-1"></div>
        <div class="blob blob-2"></div>
        <div class="blob blob-3"></div>
        <div class="bg-grid"></div>
        <div style="position: absolute; inset:0; background: radial-gradient(circle at center, transparent 0%, #050505 100%);"></div>
    </div>

    <!-- SLIDER IDIOMA -->
    <div class="lang-switch">
        <div id="lang-pill" class="lang-pill"></div>
        <button class="lang-btn active" onclick="setLang('es', 0)" aria-label="Español">
            <svg width="20" height="14" viewBox="0 0 20 14" fill="none"><rect width="20" height="14" fill="#AA151B"/><rect y="3.5" width="20" height="7" fill="#F1BF00"/></svg>
        </button>
        <button class="lang-btn" onclick="setLang('ca', 1)" aria-label="Català">
            <svg width="20" height="14" viewBox="0 0 20 14" fill="none"><rect width="20" height="14" fill="#F1BF00"/><path d="M0 2H20M0 5H20M0 8H20M0 11H20" stroke="#AA151B" stroke-width="2"/></svg>
        </button>
        <button class="lang-btn" onclick="setLang('en', 2)" aria-label="English">
            <svg width="20" height="14" viewBox="0 0 20 14" fill="none"><rect width="20" height="14" fill="#012169"/><path d="M0 0L20 14M20 0L0 14" stroke="white" stroke-width="2"/><path d="M10 0V14M0 7H20" stroke="white" stroke-width="3"/><path d="M10 0V14M0 7H20" stroke="#C8102E" stroke-width="2"/></svg>
        </button>
    </div>

    <!-- MENÚ -->
    <button class="menu-btn" onclick="toggleMenu()">
        <i data-lucide="terminal"></i>
    </button>
    <div id="menu-list" class="menu-list">
        <div class="menu-item" onclick="window.scrollTo({top:0, behavior:'smooth'}); toggleMenu()"><i data-lucide="arrow-up" size="16"></i> <span data-i18n="menu_home">Inicio</span></div>
        <div class="menu-item" onclick="openBlogOverlay(); toggleMenu()"><i data-lucide="layout" size="16"></i> <span data-i18n="menu_blog">Bitácora</span></div>
        <div class="menu-item" onclick="document.getElementById('contact').scrollIntoView({behavior:'smooth'}); toggleMenu()"><i data-lucide="mail" size="16"></i> <span data-i18n="menu_contact">Contacto</span></div>
        <div style="height:1px; background:rgba(255,255,255,0.1); margin: 5px 0;"></div>
        <div class="menu-item" onclick="window.print()" style="color: #818cf8;"><i data-lucide="printer" size="16"></i> <span data-i18n="menu_print">Imprimir CV</span></div>
    </div>

    <!-- CONTENIDO -->
    <div class="relative z-10 max-w-6xl mx-auto p-6 pt-24 pb-24" id="main-content">
        
        <header class="mb-16 animate-enter">
            <div class="inline-flex items-center gap-2 px-3 py-1 bg-white/10 border border-white/10 text-slate-300 text-[10px] font-bold tracking-widest mb-6 rounded-full">
                <span class="w-2 h-2 bg-emerald-500 rounded-full animate-pulse"></span> 
                <span data-i18n="status">DISPONIBLE JUNIO 2026</span>
            </div>
            <h1 class="text-5xl md:text-7xl font-bold text-white mb-6 tracking-tight">Pablo Mascaró</h1>
            <p class="text-xl text-slate-400 font-light max-w-2xl mb-8">
                <span data-i18n="role">Estudiante de Bachillerato Tecnológico.</span><br>
                <span class="text-white font-medium border-b border-indigo-500">Hardware</span>, 
                <span class="text-white font-medium border-b border-emerald-500" data-i18n="role_sys">Sistemas</span>
                <span data-i18n="role_end">y Reparaciones.</span>
            </p>
            <div class="flex flex-wrap gap-4">
                <a href="tel:683443335" class="bg-white text-black px-6 py-3 rounded-xl font-medium hover:bg-slate-200 transition-colors flex items-center gap-2 shadow-lg hover:scale-105 transform duration-200">
                    <i data-lucide="phone" size="18"></i> 683 443 335
                </a>
                <a href="mailto:pau14mascaro@gmail.com" class="bg-white/5 border border-white/10 text-white px-6 py-3 rounded-xl font-medium hover:bg-white/10 transition-colors flex items-center gap-2 hover:scale-105 transform duration-200">
                    <i data-lucide="mail" size="18"></i> Email
                </a>
            </div>
        </header>

        <!-- GRID -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

            <!-- 1. HARDWARE -->
            <div class="card lg:col-span-2 animate-enter delay-100 group" onclick="openModal('hw')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-4 mb-6">
                    <div class="p-3 bg-indigo-500/20 rounded-xl text-indigo-400 group-hover:bg-indigo-500/30 transition-colors"><i data-lucide="cpu" size="24"></i></div>
                    <h2 class="text-2xl font-bold text-white" data-i18n="hw_title">Ingeniería de Hardware</h2>
                </div>
                <div class="grid sm:grid-cols-2 gap-6">
                    <div>
                        <h4 class="text-xs font-bold text-slate-500 uppercase mb-3 flex items-center gap-2"><i data-lucide="wrench" size="14"></i> <span data-i18n="hw_sub1">Montaje</span></h4>
                        <ul class="text-sm text-slate-300 space-y-2">
                            <li class="flex gap-2"><span class="text-indigo-500">•</span> Gaming & Workstation Custom</li>
                            <li class="flex gap-2"><span class="text-indigo-500">•</span> Gestión de Cableado</li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="text-xs font-bold text-slate-500 uppercase mb-3 flex items-center gap-2"><i data-lucide="activity" size="14"></i> <span data-i18n="hw_sub2">Diagnóstico</span></h4>
                        <ul class="text-sm text-slate-300 space-y-2">
                            <li class="flex gap-2"><span class="text-yellow-500">•</span> Detección de fallos críticos</li>
                            <li class="flex gap-2"><span class="text-yellow-500">•</span> Mantenimiento Térmico</li>
                        </ul>
                    </div>
                </div>
            </div>

            <!-- 2. STACK -->
            <div class="card animate-enter delay-200" onclick="openModal('stack')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-6">
                    <div class="p-2 bg-blue-500/20 rounded-lg text-blue-400"><i data-lucide="code-2" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="stack_title">Stack</h2>
                </div>
                <div class="space-y-4">
                    <div><div class="flex justify-between text-xs text-slate-300 mb-1"><span>HTML/CSS</span><span>75%</span></div><div class="progress-bg"><div class="progress-fill bg-blue-500" style="--w: 75%"></div></div></div>
                    <div><div class="flex justify-between text-xs text-slate-300 mb-1"><span>JavaScript</span><span>60%</span></div><div class="progress-bg"><div class="progress-fill bg-blue-400" style="--w: 60%"></div></div></div>
                    <div><div class="flex justify-between text-xs text-slate-300 mb-1"><span>React (Básico)</span><span>40%</span></div><div class="progress-bg"><div class="progress-fill bg-indigo-400" style="--w: 40%"></div></div></div>
                </div>
            </div>

            <!-- 3. EXPERIENCIA -->
            <div class="card animate-enter delay-300" onclick="openModal('exp')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-emerald-500/20 rounded-lg text-emerald-400"><i data-lucide="briefcase" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="exp_title">Experiencia</h2>
                </div>
                <div class="mt-auto p-3 bg-white/5 rounded-xl border border-white/5">
                    <div class="font-bold text-white text-sm">Hotel Gemini</div>
                    <div class="text-xs text-emerald-400" data-i18n="exp_role">Camarero Desayunos</div>
                    <div class="text-[10px] text-slate-500 mt-1">2025 • Menorca</div>
                </div>
            </div>

            <!-- 4. IDIOMAS -->
            <div class="card animate-enter delay-100" onclick="openModal('lang')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-pink-500/20 rounded-lg text-pink-400"><i data-lucide="languages" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="lang_title">Idiomas</h2>
                </div>
                <div class="space-y-2 mt-auto text-sm">
                    <div class="flex justify-between p-2 rounded bg-white/5 border border-white/5"><span class="text-slate-300" data-i18n="lang_es">Español</span> <span class="text-pink-300 text-[10px] font-bold bg-pink-500/20 px-2 py-0.5 rounded" data-i18n="lang_native">NATIVO</span></div>
                    <div class="flex justify-between p-2 rounded bg-white/5 border border-white/5"><span class="text-slate-300" data-i18n="lang_ca">Catalán</span> <span class="text-pink-300 text-[10px] font-bold bg-pink-500/20 px-2 py-0.5 rounded" data-i18n="lang_native">NATIVO</span></div>
                    <div class="flex justify-between p-2 rounded bg-white/5 border border-white/5"><span class="text-slate-300" data-i18n="lang_en">Inglés</span> <span class="text-white text-[10px] font-bold">B2 / C1</span></div>
                </div>
            </div>

            <!-- 5. SETUP -->
            <div class="card animate-enter delay-200" onclick="openModal('setup')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-cyan-500/20 rounded-lg text-cyan-400"><i data-lucide="monitor" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="setup.title">Setup</h2>
                </div>
                <div class="mt-auto grid grid-cols-2 gap-2 text-center">
                    <div class="bg-black/40 p-2 rounded border border-white/5"><i data-lucide="cpu" class="w-4 h-4 text-cyan-400 mx-auto mb-1"></i><div class="text-[10px] text-slate-300">i5 14600KF</div></div>
                    <div class="bg-black/40 p-2 rounded border border-white/5"><i data-lucide="zap" class="w-4 h-4 text-green-400 mx-auto mb-1"></i><div class="text-[10px] text-slate-300">RTX 5060</div></div>
                </div>
            </div>

            <!-- 6. FORMACIÓN -->
            <div class="card lg:col-span-2 animate-enter delay-300" onclick="openModal('edu')">
                <i data-lucide="maximize-2" class="card-icon-float"></i>
                <div class="flex items-center gap-4 mb-4">
                    <div class="p-2 bg-purple-500/20 rounded-lg text-purple-400"><i data-lucide="graduation-cap" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="edu_title">Formación</h2>
                </div>
                <div class="grid sm:grid-cols-2 gap-8 mt-2">
                    <div class="pl-4 border-l-2 border-purple-500">
                        <div class="text-[10px] font-bold text-purple-400 mb-1" data-i18n="edu_curr">ACTUALIDAD</div>
                        <div class="font-bold text-white text-sm" data-i18n="edu_deg1">Bachillerato Tecnológico</div>
                        <div class="text-xs text-slate-400 mt-0.5">IES Maria Àngels Cardona</div>
                    </div>
                    <div class="pl-4 border-l-2 border-slate-700">
                        <div class="text-[10px] font-bold text-slate-500 mb-1">2020 - 2024</div>
                        <div class="font-bold text-slate-300 text-sm">ESO</div>
                        <div class="text-xs text-slate-500">IES Maria Àngels Cardona</div>
                    </div>
                </div>
            </div>

            <!-- 7. CONTACTO -->
            <div id="contact" class="card animate-enter delay-100 hover:border-orange-500/50" onclick="openModal('contact')">
                <div class="flex items-center gap-3 mb-2">
                    <div class="p-2 bg-teal-500/20 rounded-lg text-teal-400"><i data-lucide="at-sign" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="contact_title">Contacto</h2>
                </div>
                <div class="mt-auto text-sm text-slate-400 break-all space-y-1">
                    <div class="flex items-center gap-2"><i data-lucide="mail" class="w-3 h-3"></i> pau14mascaro@...</div>
                    <div class="flex items-center gap-2 text-pink-400"><i data-lucide="instagram" class="w-3 h-3"></i> @pb.mojz_</div>
                </div>
            </div>

            <!-- 8. UBICACIÓN -->
            <div class="card animate-enter delay-200" onclick="openModal('location')">
                <div class="flex items-center gap-3 mb-2">
                    <div class="p-2 bg-orange-500/20 rounded-lg text-orange-400"><i data-lucide="map-pin" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="loc_title">Ubicación</h2>
                </div>
                <div class="mt-auto text-center">
                    <div class="font-bold text-white text-sm">Son Carrió</div>
                    <div class="text-[10px] text-orange-300/80 mt-1" data-i18n="loc_sub">Movilidad Propia</div>
                </div>
            </div>

            <!-- 9. BITÁCORA -->
            <div onclick="openBlogOverlay()" class="card lg:col-span-2 animate-enter delay-300 hover:border-gray-500/50">
                <div class="flex items-center gap-3 mb-4">
                    <div class="p-2 bg-gray-500/20 rounded-lg text-gray-400"><i data-lucide="terminal" size="20"></i></div>
                    <h2 class="text-xl font-bold text-white" data-i18n="blog_title">Bitácora</h2>
                </div>
                <div class="mt-auto p-4 rounded-xl border border-white/5 bg-black/20 group-hover:bg-black/40 transition-colors">
                    <div class="flex justify-between text-xs mb-1">
                        <span class="text-emerald-400 font-mono">LATEST LOG</span>
                        <span class="text-slate-500">27 JAN 2026</span>
                    </div>
                    <h3 class="text-white font-bold text-sm">Portfolio Final Release</h3>
                </div>
            </div>
        </div>
        
        <footer id="contact-footer" class="text-center text-[10px] font-mono text-slate-600 pb-12">SYSTEM ONLINE · CIUTADELLA</footer>
    </div>

    <!-- MODAL -->
    <div id="modal-overlay" class="modal-overlay" onclick="closeModal()">
        <div class="modal-content" onclick="event.stopPropagation()">
            <div class="p-8">
                <div class="flex justify-between items-center mb-6">
                    <div class="flex items-center gap-4">
                        <div id="modal-icon-wrapper" class="w-10 h-10 rounded-xl flex items-center justify-center bg-white/10"><i id="modal-icon" data-lucide="info"></i></div>
                        <h2 id="modal-title" class="text-2xl font-bold text-white">Título</h2>
                    </div>
                    <button onclick="closeModal()" class="text-slate-400 hover:text-white"><i data-lucide="x"></i></button>
                </div>
                <div id="modal-body" class="text-slate-300 leading-relaxed text-sm space-y-4"></div>
            </div>
        </div>
    </div>

    <!-- BLOG OVERLAY -->
    <div id="blog-overlay" class="fixed inset-0 z-[100] bg-[#050505] hidden flex-col">
        <div class="fixed top-0 left-0 right-0 p-6 flex justify-between items-center z-10 bg-[#050505]/90 backdrop-blur-md border-b border-white/10">
            <h2 class="text-2xl font-bold text-white flex items-center gap-3"><i data-lucide="terminal" class="text-emerald-500"></i> <span data-i18n="blog_title">Bitácora</span></h2>
            <button onclick="closeBlogOverlay()" class="w-10 h-10 rounded-full bg-white/10 flex items-center justify-center text-white hover:bg-white/20"><i data-lucide="x" class="w-5 h-5"></i></button>
        </div>
        <div class="overflow-y-auto pt-24 pb-12 px-6">
            <div class="max-w-3xl mx-auto space-y-8" id="blog-container"></div>
        </div>
    </div>

    <script>
        lucide.createIcons();
        window.onload = () => { document.body.classList.add('loaded'); lucide.createIcons(); };

        const texts = {
            es: { status: "DISPONIBLE JUNIO 2026", role: "Estudiante de Bachillerato Tecnológico.", role_sys: "Sistemas", role_end: "y Reparaciones.", hw_title: "Ingeniería de Hardware", hw_sub1: "Montaje", hw_sub2: "Diagnóstico", exp_title: "Experiencia", exp_role: "Camarero Desayunos", stack_title: "Conocimientos", lang_title: "Idiomas", lang_es: "Español", lang_ca: "Catalán", lang_en: "Inglés", lang_native: "NATIVO", setup_title: "Setup", edu_title: "Formación", edu_curr: "ACTUALIDAD", edu_deg1: "Bachillerato Tecnológico", contact_title: "Contacto", loc_title: "Ubicación", loc_sub: "Movilidad Propia", blog_title: "Bitácora", menu_home: "Inicio", menu_blog: "Bitácora", menu_contact: "Contacto", menu_print: "Imprimir CV" },
            ca: { status: "DISPONIBLE JUNY 2026", role: "Estudiant de Batxillerat Tecnològic.", role_sys: "Sistemes", role_end: "i Reparacions.", hw_title: "Enginyeria de Hardware", hw_sub1: "Muntatge", hw_sub2: "Diagnòstic", exp_title: "Experiència", exp_role: "Cambrer Esmorzars", stack_title: "Coneixements", lang_title: "Idiomes", lang_es: "Espanyol", lang_ca: "Català", lang_en: "Anglès", lang_native: "NATIU", setup_title: "Setup", edu_title: "Formació", edu_curr: "ACTUALITAT", edu_deg1: "Batxillerat Tecnològic", contact_title: "Contacte", loc_title: "Ubicació", loc_sub: "Mobilitat Pròpia", blog_title: "Bitàcola", menu_home: "Inici", menu_blog: "Bitàcola", menu_contact: "Contacte", menu_print: "Imprimir CV" },
            en: { status: "AVAILABLE JUNE 2026", role: "Tech High School Student.", role_sys: "Systems", role_end: "and Repairs.", hw_title: "Hardware Engineering", hw_sub1: "Assembly", hw_sub2: "Diagnostics", exp_title: "Experience", exp_role: "Breakfast Waiter", stack_title: "Knowledge", lang_title: "Languages", lang_es: "Spanish", lang_ca: "Catalan", lang_en: "English", lang_native: "NATIVE", setup_title: "Setup", edu_title: "Education", edu_curr: "PRESENT", edu_deg1: "Tech Baccalaureate", contact_title: "Contact", loc_title: "Location", loc_sub: "Own Vehicle", blog_title: "Dev Logs", menu_home: "Home", menu_blog: "Logs", menu_contact: "Contact", menu_print: "Print CV" }
        };

        const cardsData = {
            hw: { color: 'indigo', icon: 'cpu', es: `
                <div class="grid gap-4"><div class="p-4 bg-white/5 rounded-xl border border-white/10"><h4 class="text-indigo-400 font-bold mb-2 flex items-center gap-2"><i data-lucide="wrench" class="w-4 h-4"></i> Montaje a Medida</h4><p class="text-slate-400 text-sm">Planificación y ensamblaje de equipos desde cero evitando cuellos de botella.</p></div><div class="p-4 bg-white/5 rounded-xl border border-white/10"><h4 class="text-indigo-400 font-bold mb-2 flex items-center gap-2"><i data-lucide="zap" class="w-4 h-4"></i> Mantenimiento</h4><p class="text-slate-400 text-sm">Limpieza profunda, cambio de pasta térmica (Arctic/Noctua) y diagnóstico.</p></div></div>`, ca: `<p>Muntatge i reparació.</p>`, en: `<p>Assembly and repair.</p>` },
            exp: { color: 'emerald', icon: 'briefcase', es: `<ul class="space-y-3"><li><strong>Responsable de Desayunos</strong> (Hotel Gemini)<br>Gestión completa del servicio y atención a clientes internacionales.</li></ul>`, ca: `<p>Hotel Gemini.</p>`, en: `<p>Hotel Gemini.</p>` },
            stack: { color: 'blue', icon: 'code-2', es: `<p><strong>Frontend:</strong> HTML5, CSS3, JS, React.<br><strong>Backend/Sys:</strong> Java, C++, Linux (Mint/Ubuntu).</p>`, ca: `<p>Stack tecnològic.</p>`, en: `<p>Tech stack.</p>` },
            lang: { color: 'pink', icon: 'languages', es: `<p>• Español/Catalán: Nativo<br>• Inglés: B2/C1 (Fluido)</p>`, ca: `<p>Idiomes.</p>`, en: `<p>Languages.</p>` },
            setup: { color: 'cyan', icon: 'monitor', es: `<p><strong>PC:</strong> i5 14600KF + RTX 5060 + 32GB DDR5.<br><strong>Periféricos:</strong> Razer Huntsman Mini + Basilisk V3.</p>`, ca: `<p>Setup.</p>`, en: `<p>Setup.</p>` },
            edu: { color: 'purple', icon: 'graduation-cap', es: `<p><strong>Bachillerato Tecnológico</strong> (2024-Presente)<br>IES Maria Àngels Cardona.<br><br><strong>ESO</strong> (2020-2024)</p>`, ca: `<p>Educació.</p>`, en: `<p>Education.</p>` },
            contact: { color: 'teal', icon: 'at-sign', es: `<p>Tel: 683 443 335<br>Email: pau14mascaro@gmail.com</p>`, ca: `<p>Contacte.</p>`, en: `<p>Contact.</p>` },
            location: { color: 'orange', icon: 'map-pin', es: `<p>Son Carrió, Menorca.<br>Vehículo propio disponible.</p>`, ca: `<p>Ubicació.</p>`, en: `<p>Location.</p>` }
        };

        let currentLang = 'es';

        function setLang(lang, index) {
            currentLang = lang;
            document.querySelectorAll('[data-i18n]').forEach(el => { const key = el.getAttribute('data-i18n'); if (texts[lang][key]) el.innerText = texts[lang][key]; });
            document.getElementById('lang-pill').style.transform = `translateX(${index * 100}%)`;
            document.querySelectorAll('.lang-btn').forEach((btn, i) => { btn.style.color = i === index ? 'white' : '#64748b'; });
        }

        function openModal(id) {
            const data = cardsData[id] || { es: "Info...", color: 'gray', icon: 'info' };
            const titleKeys = { 'hw': 'hw_title', 'exp': 'exp_title', 'stack': 'stack_title', 'lang': 'lang_title', 'setup': 'setup_title', 'edu': 'edu_title', 'contact': 'contact_title', 'location': 'loc_title' };
            document.getElementById('modal-title').innerText = texts[currentLang][titleKeys[id]] || "Detalle";
            document.getElementById('modal-body').innerHTML = data[currentLang] || data['es'];
            
            const wrapper = document.getElementById('modal-icon-wrapper');
            wrapper.className = `w-10 h-10 rounded-xl flex items-center justify-center bg-${data.color}-500/20 text-${data.color}-400`;
            document.getElementById('modal-icon').setAttribute('data-lucide', data.icon);
            lucide.createIcons();

            const overlay = document.getElementById('modal-overlay');
            overlay.classList.remove('hidden');
            overlay.classList.add('flex');
            setTimeout(() => overlay.classList.add('open'), 10);
            document.body.classList.add('modal-open');
        }

        function closeModal() {
            const overlay = document.getElementById('modal-overlay');
            overlay.classList.remove('open');
            setTimeout(() => { overlay.classList.add('hidden'); overlay.classList.remove('flex'); document.body.classList.remove('modal-open'); }, 300);
        }

        function toggleMenu() { document.getElementById('menu-list').classList.toggle('show'); }

        // Blog
        function openBlogOverlay() {
            const posts = [{ title: "Portfolio Final", date: "27/01/2026", text: "Versión final optimizada." }, { title: "RTX 5060", date: "15/01/2026", text: "Benchmark térmico." }];
            document.getElementById('blog-container').innerHTML = posts.map(p => `<article class="p-6 rounded-2xl border border-white/10 bg-white/5"><div class="text-emerald-400 text-xs font-mono mb-2">${p.date}</div><h3 class="text-2xl font-bold text-white mb-2">${p.title}</h3><p class="text-slate-400">${p.text}</p></article>`).join('');
            document.getElementById('blog-overlay').classList.remove('hidden');
            document.getElementById('blog-overlay').classList.add('flex');
            document.body.classList.add('modal-open');
        }
        function closeBlogOverlay() { document.getElementById('blog-overlay').classList.add('hidden'); document.getElementById('blog-overlay').classList.remove('flex'); document.body.classList.remove('modal-open'); }
        function triggerEasterEgg() { alert("👾: Beep!"); }
        document.addEventListener('keydown', (e) => { if (e.key === 'Escape') { closeModal(); closeBlogOverlay(); } });
        setTimeout(() => { const active = document.querySelector('.lang-btn.active'); if(active) setLang('es', 0); }, 100);
    </script>
</body>
</html>
