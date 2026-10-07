```html
<!DOCTYPE html>
<html lang="es" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Plataforma de Sorteos Digitales Pro (0000 - 9999)</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#fffbe1',
                            100: '#fff3b8',
                            400: '#facc15',
                            500: '#eab308',
                            600: '#ca8a04',
                            900: '#713f12',
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace'],
                    }
                }
            }
        }
    </script>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=JetBrains+Mono:wght@500;700;800&display=swap" rel="stylesheet">
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

    <style>
        body { font-family: 'Inter', sans-serif; }
        .slot-digit-container {
            perspective: 400px;
        }
        .glow-gold {
            box-shadow: 0 0 35px -5px rgba(234, 179, 8, 0.35);
        }
        .glow-emerald {
            box-shadow: 0 0 35px -5px rgba(16, 185, 129, 0.35);
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #475569; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col justify-between selection:bg-amber-500 selection:text-slate-950">

    <header class="bg-slate-900/90 backdrop-blur-md border-b border-slate-800/80 sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3.5 flex justify-between items-center">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="switchMainTab('play')">
                <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-amber-500 via-yellow-400 to-amber-300 flex items-center justify-center text-slate-950 shadow-lg shadow-amber-500/20">
                    <i data-lucide="dices" class="w-6 h-6 stroke-[2.5]"></i>
                </div>
                <div>
                    <h1 class="text-lg sm:text-xl font-extrabold bg-gradient-to-r from-amber-300 via-yellow-200 to-amber-400 bg-clip-text text-transparent leading-tight" id="header-app-title">
                        Plataforma de Sorteos
                    </h1>
                    <p class="text-[11px] text-slate-400 flex items-center gap-1 font-mono">
                        <span class="inline-block w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
                        Boletas 0000 - 9999
                    </p>
                </div>
            </div>

            <nav class="flex items-center space-x-2 sm:space-x-3">
                <button onclick="switchMainTab('play')" id="nav-btn-play" class="px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold transition bg-amber-500 text-slate-950 hover:bg-amber-400 flex items-center gap-2">
                    <i data-lucide="sparkles" class="w-4 h-4"></i>
                    <span>Probar Suerte</span>
                </button>
                <button onclick="shareRaffleLink()" class="px-3.5 py-2 rounded-xl text-xs sm:text-sm font-semibold text-slate-300 hover:text-white hover:bg-slate-800 transition flex items-center gap-2 border border-slate-800" title="Compartir Sorteo">
                    <i data-lucide="share-2" class="w-4 h-4 text-amber-400"></i>
                    <span class="hidden sm:inline">Compartir</span>
                </button>
                <button onclick="openAdminAuthModal()" class="p-2.5 rounded-xl text-slate-400 hover:text-amber-400 hover:bg-slate-800 transition border border-transparent hover:border-slate-700" title="Panel de Administración">
                    <i data-lucide="shield-check" class="w-5 h-5"></i>
                </button>
            </nav>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8 flex-grow w-full">

        <!-- RAFFLE INFO BANNER -->
        <section class="mb-8 sm:mb-10 bg-gradient-to-r from-slate-900 via-slate-900/95 to-slate-800 border border-slate-800/80 rounded-3xl p-6 sm:p-8 shadow-2xl relative overflow-hidden">
            <div class="absolute -right-12 -bottom-12 w-80 h-80 bg-amber-500/10 rounded-full blur-3xl pointer-events-none"></div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 items-center relative z-10">
                <div class="md:col-span-2 space-y-3">
                    <div class="flex items-center gap-2">
                        <span class="inline-flex items-center px-3 py-1 rounded-full text-xs font-semibold bg-amber-500/10 text-amber-400 border border-amber-500/20">
                            <i data-lucide="trophy" class="w-3.5 h-3.5 mr-1.5 text-amber-400"></i> Gran Sorteo Oficial
                        </span>
                        <span id="banner-pricing-tag" class="inline-flex items-center px-3 py-1 rounded-full text-xs font-mono font-bold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                            Precio = Número de Boleta
                        </span>
                    </div>
                    <h2 class="text-2xl sm:text-4xl font-extrabold text-white tracking-tight" id="banner-title">
                        Cargando Sorteo...
                    </h2>
                    <p class="text-slate-300 text-sm sm:text-base leading-relaxed" id="banner-description">
                        Obtén tu número aleatorio presionando el botón. El valor a pagar será idéntico al número obtenido.
                    </p>
                    <div class="flex flex-wrap gap-3 text-xs font-medium text-slate-400 pt-2">
                        <div class="flex items-center bg-slate-950/60 px-3 py-2 rounded-xl border border-slate-800">
                            <i data-lucide="calendar" class="w-4 h-4 text-amber-400 mr-2"></i>
                            <span id="banner-date">Fecha: Por definir</span>
                        </div>
                        <div class="flex items-center bg-slate-950/60 px-3 py-2 rounded-xl border border-slate-800">
                            <i data-lucide="building-2" class="w-4 h-4 text-amber-400 mr-2"></i>
                            <span id="banner-lottery">Lotería: Sorteo Mayor</span>
                        </div>
                    </div>
                </div>

                <!-- PRIZES CAROUSEL / HIGHLIGHT -->
                <div class="bg-slate-950/80 border border-slate-800 rounded-2xl p-5 backdrop-blur">
                    <h3 class="text-xs uppercase tracking-wider text-slate-400 font-bold mb-3 flex items-center justify-between">
                        <span class="flex items-center gap-1.5">
                            <i data-lucide="gift" class="w-4 h-4 text-amber-400"></i> Plan de Premios
                        </span>
                        <span class="text-[10px] text-amber-400/80 font-normal">100% Garantizado</span>
                    </h3>
                    <div id="prizes-container" class="space-y-2.5">
                        <!-- Dynamic Prize Cards -->
                    </div>
                </div>
            </div>
        </section>

        <div id="tab-play" class="space-y-8">
            <div class="max-w-2xl mx-auto text-center space-y-6 bg-slate-900/60 border border-slate-800/80 p-6 sm:p-8 rounded-3xl backdrop-blur relative shadow-xl">
                
                <div class="space-y-2">
                    <span class="px-3 py-1 bg-amber-500/10 text-amber-400 text-xs font-semibold rounded-full border border-amber-500/20">
                        Generador Aleatorio Criptográfico
                    </span>
                    <h3 class="text-2xl sm:text-3xl font-black text-white">Prueba tu Suerte</h3>
                    <p class="text-slate-400 text-xs sm:text-sm">Presiona el botón para tirar los dados. Tu costo exacto será determinado por el número aleatorio obtenido (0000 al 9999).</p>
                </div>

                <!-- SLOT MACHINE DISPLAY -->
                <div class="bg-slate-950 border-2 border-amber-500/40 rounded-2xl p-6 sm:p-8 my-6 glow-gold relative overflow-hidden">
                    <div class="flex justify-center items-center space-x-2 sm:space-x-4">
                        <div id="digit-0" class="w-14 h-20 sm:w-20 sm:h-28 bg-slate-900 border border-slate-700/80 rounded-2xl flex items-center justify-center text-3xl sm:text-5xl font-black text-amber-400 font-mono shadow-inner select-none">0</div>
                        <div id="digit-1" class="w-14 h-20 sm:w-20 sm:h-28 bg-slate-900 border border-slate-700/80 rounded-2xl flex items-center justify-center text-3xl sm:text-5xl font-black text-amber-400 font-mono shadow-inner select-none">0</div>
                        <div id="digit-2" class="w-14 h-20 sm:w-20 sm:h-28 bg-slate-900 border border-slate-700/80 rounded-2xl flex items-center justify-center text-3xl sm:text-5xl font-black text-amber-400 font-mono shadow-inner select-none">0</div>
                        <div id="digit-3" class="w-14 h-20 sm:w-20 sm:h-28 bg-slate-900 border border-slate-700/80 rounded-2xl flex items-center justify-center text-3xl sm:text-5xl font-black text-amber-400 font-mono shadow-inner select-none">0</div>
                    </div>

                    <!-- DYNAMIC PRICE DISPLAY -->
                    <div id="generated-price-box" class="mt-6 pt-5 border-t border-slate-800/80 transition-all duration-300">
                        <span class="text-xs uppercase tracking-widest text-slate-400 block font-bold mb-1">Precio de la Boleta Obtenida</span>
                        <div class="text-3xl sm:text-4xl font-extrabold text-emerald-400 font-mono" id="generated-price-display">
                            $0 COP
                        </div>
                        <p class="text-xs text-slate-400 mt-1" id="price-rule-explanation">
                            Valor equivalente al número aleatorio generado
                        </p>
                    </div>
                </div>

                <!-- MAIN INTERACTIVE BUTTONS -->
                <div class="flex flex-col sm:flex-row gap-4 justify-center items-center">
                    <button id="btn-roll" onclick="rollLuckyNumber()" class="w-full sm:w-auto px-8 py-4 bg-gradient-to-r from-amber-500 via-yellow-400 to-amber-500 hover:from-amber-400 hover:to-yellow-300 text-slate-950 font-black text-base sm:text-lg rounded-2xl shadow-xl hover:shadow-amber-500/20 transform hover:-translate-y-0.5 transition active:scale-95 flex items-center justify-center space-x-3">
                        <i data-lucide="dices" class="w-6 h-6 stroke-[2.5]"></i>
                        <span id="roll-btn-text">¡PROBAR MI SUERTE!</span>
                    </button>

                    <button id="btn-buy-generated" onclick="openCheckoutModal()" disabled class="w-full sm:w-auto px-8 py-4 bg-emerald-600 hover:bg-emerald-500 disabled:bg-slate-800 disabled:text-slate-600 disabled:border-slate-800 disabled:cursor-not-allowed text-white font-extrabold text-base sm:text-lg rounded-2xl shadow-lg transition active:scale-95 flex items-center justify-center space-x-2 border border-emerald-500/30">
                        <i data-lucide="shopping-cart" class="w-5 h-5"></i>
                        <span>Comprar Este Número</span>
                    </button>
                </div>
            </div>
        </div>

    </main>

    <div id="modal-checkout" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-3xl max-w-lg w-full p-6 sm:p-8 shadow-2xl relative max-h-[90vh] overflow-y-auto">
            <button onclick="closeCheckoutModal()" class="absolute top-5 right-5 text-slate-400 hover:text-white p-2">
                <i data-lucide="x" class="w-6 h-6"></i>
            </button>

            <div class="flex items-center space-x-3 mb-2">
                <div class="p-2.5 rounded-xl bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">
                    <i data-lucide="ticket" class="w-6 h-6"></i>
                </div>
                <div>
                    <h3 class="text-xl font-black text-white">Completar Reserva</h3>
                    <p class="text-xs text-slate-400">Ingresa tus datos para apartar tu número de la suerte.</p>
                </div>
            </div>

            <!-- TICKET SUMMARY -->
            <div class="bg-slate-950 border border-amber-500/30 rounded-2xl p-4 my-5 flex justify-between items-center">
                <div>
                    <span class="text-[10px] uppercase font-bold text-slate-400 block tracking-wider">Número de Boleta</span>
                    <span id="checkout-ticket-num" class="text-3xl font-black text-amber-400 font-mono">0000</span>
                </div>
                <div class="text-right">
                    <span class="text-[10px] uppercase font-bold text-slate-400 block tracking-wider">Monto Exacto a Pagar</span>
                    <span id="checkout-ticket-price" class="text-2xl font-extrabold text-emerald-400 font-mono">$0 COP</span>
                </div>
            </div>

            <!-- BUYER FORM -->
            <form id="checkout-form" onsubmit="handleCheckoutSubmit(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-300 uppercase mb-1">Nombre Completo *</label>
                    <input type="text" id="buyer-name" required placeholder="Ej. Carlos Mendoza" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-100 focus:outline-none focus:border-amber-500">
                </div>

                <div>
                    <label class="block text-xs font-bold text-slate-300 uppercase mb-1">Teléfono / WhatsApp *</label>
                    <input type="tel" id="buyer-phone" required placeholder="Ej. +57 300 123 4567" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-100 focus:outline-none focus:border-amber-500">
                </div>

                <div>
                    <label class="block text-xs font-bold text-slate-300 uppercase mb-1">Cédula / Documento ID *</label>
                    <input type="text" id="buyer-id" required placeholder="Ej. 1098765432" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-100 focus:outline-none focus:border-amber-500">
                </div>

                <div>
                    <label class="block text-xs font-bold text-slate-300 uppercase mb-1">Referencia o Nro de Comprobante</label>
                    <input type="text" id="buyer-ref" placeholder="Ej. Transacción Nequi #98421" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-sm text-slate-100 focus:outline-none focus:border-amber-500">
                </div>

                <!-- PAYMENT METHODS INSTRUCTIONS -->
                <div class="border-t border-slate-800/80 pt-4 mt-4">
                    <h4 class="text-xs font-bold text-amber-400 uppercase mb-2 flex items-center gap-1.5">
                        <i data-lucide="landmark" class="w-4 h-4"></i> Cuentas Autorizadas para Pago
                    </h4>
                    <div id="payment-instructions-box" class="bg-slate-950 p-4 rounded-xl text-xs text-slate-300 whitespace-pre-line border border-slate-800 font-mono leading-relaxed">
                        Cargando información bancaria...
                    </div>
                </div>

                <div class="pt-2">
                    <button type="submit" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-bold py-3.5 rounded-xl shadow-lg transition flex items-center justify-center space-x-2">
                        <i data-lucide="check-circle-2" class="w-5 h-5"></i>
                        <span>Confirmar y Enviar Reserva</span>
                    </button>
                </div>
            </form>
        </div>
    </div>

    <div id="modal-admin-auth" class="fixed inset-0 z-50 bg-slate-950/80 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-3xl max-w-sm w-full p-6 shadow-2xl relative text-center">
            <button onclick="closeAdminAuthModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <div class="w-12 h-12 bg-amber-500/10 text-amber-400 rounded-2xl flex items-center justify-center mx-auto mb-4 border border-amber-500/20">
                <i data-lucide="lock" class="w-6 h-6"></i>
            </div>
            <h3 class="text-xl font-bold text-white mb-1">Acceso Administrador</h3>
            <p class="text-xs text-slate-400 mb-4">Ingresa tu PIN de seguridad para editar el sorteo.</p>

            <form onsubmit="handleAdminLogin(event)" class="space-y-4">
                <input type="password" id="admin-pin-input" maxlength="8" required placeholder="PIN (Defecto: 1234)" class="w-full bg-slate-950 border border-slate-800 rounded-xl px-4 py-3 text-center text-xl font-mono text-amber-400 focus:outline-none focus:border-amber-500">
                <button type="submit" class="w-full bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold py-3 rounded-xl transition">
                    Ingresar
                </button>
            </form>
        </div>
    </div>

    <div id="modal-admin-panel" class="fixed inset-0 z-50 bg-slate-950/90 backdrop-blur-md hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-3xl max-w-4xl w-full p-6 sm:p-8 shadow-2xl relative max-h-[92vh] flex flex-col">
            
            <div class="flex justify-between items-center pb-4 border-b border-slate-800">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-xl bg-amber-500 text-slate-950 flex items-center justify-center font-bold">
                        <i data-lucide="sliders" class="w-5 h-5"></i>
                    </div>
                    <div>
                        <h3 class="text-xl font-bold text-white">Panel de Control de Administración</h3>
                        <p class="text-xs text-slate-400">Edita configuración, precios, boletas y ejecuta el sorteo</p>
                    </div>
                </div>
                <button onclick="closeAdminPanelModal()" class="text-slate-400 hover:text-white p-2">
                    <i data-lucide="x" class="w-6 h-6"></i>
                </button>
            </div>

            <!-- ADMIN TABS NAVIGATION -->
            <div class="flex border-b border-slate-800 my-4 space-x-2 sm:space-x-4 overflow-x-auto">
                <button onclick="switchAdminSubTab('settings')" id="admin-tab-btn-settings" class="pb-2 px-1 text-xs sm:text-sm font-bold border-b-2 border-amber-500 text-amber-40
