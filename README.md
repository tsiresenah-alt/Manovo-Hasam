<!DOCTYPE html>
<html lang="fr" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>M-Work | Micro-tâches & Gains en ligne</title>
    <!-- Tailwind CSS via CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: { 50: '#eef2ff', 600: '#2563eb', 700: '#1d4ed8', 800: '#1e40af', 900: '#0f172a' },
                        momo: { emerald: '#059669', mvola: '#008751', orange: '#ff6600', airtel: '#e60000' }
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; -webkit-tap-highlight-color: transparent; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="h-full text-slate-800 bg-slate-100 flex flex-col justify-between select-none">

    <div id="app-container" class="max-w-md w-full mx-auto min-h-screen bg-slate-50 flex flex-col relative shadow-xl">

        <!-- EN-TÊTE SUPÉRIEUR -->
        <header id="main-header" class="hidden bg-slate-900 text-white px-4 pt-4 pb-4 sticky top-0 z-30 shadow-md">
            <div class="flex items-center justify-between">
                <div class="flex items-center space-x-2">
                    <div class="w-9 h-9 rounded-xl bg-gradient-to-tr from-blue-600 to-emerald-500 flex items-center justify-center font-black text-lg shadow-inner">
                        M
                    </div>
                    <div>
                        <h1 class="text-base font-bold tracking-tight leading-tight flex items-center gap-1.5">
                            M-Work <span class="text-[10px] font-semibold uppercase bg-emerald-500/20 text-emerald-400 border border-emerald-500/30 px-1.5 py-0.5 rounded">Mada</span>
                        </h1>
                        <p class="text-[11px] text-slate-400" id="header-user-phone">034 ••• ••</p>
                    </div>
                </div>
                <div class="flex items-center gap-2">
                    <div class="bg-slate-800 border border-slate-700/80 rounded-full px-3 py-1 flex items-center gap-1.5 cursor-pointer" onclick="navigateTo('wallet')">
                        <span class="w-2 h-2 rounded-full bg-emerald-400 animate-ping"></span>
                        <span class="text-xs font-semibold text-emerald-400" id="header-balance">0 Ar</span>
                    </div>
                </div>
            </div>
        </header>

        <!-- VUE AUTHENTIFICATION -->
        <section id="view-auth" class="flex-1 flex flex-col justify-center px-5 py-8">
            <div class="text-center mb-8">
                <div class="w-16 h-16 rounded-2xl bg-gradient-to-tr from-blue-700 via-blue-600 to-emerald-500 text-white font-extrabold text-3xl mx-auto flex items-center justify-center shadow-lg shadow-blue-500/20 mb-3">M</div>
                <h2 class="text-2xl font-black text-slate-900 tracking-tight">M-Work Madagascar</h2>
                <p class="text-sm text-slate-600 mt-1 max-w-xs mx-auto">Effectuez des micro-tâches rémunérées et retirez vos gains directement en Ariary via MVola, Orange Money ou Airtel Money.</p>
            </div>

            <!-- Onglets Inscription / Connexion -->
            <div class="bg-slate-200/80 p-1 rounded-xl flex mb-6">
                <button id="tab-btn-register" onclick="switchAuthTab('register')" class="flex-1 py-2 text-xs font-bold rounded-lg shadow-sm bg-white text-blue-700">Inscription</button>
                <button id="tab-btn-login" onclick="switchAuthTab('login')" class="flex-1 py-2 text-xs font-bold rounded-lg text-slate-600">Connexion</button>
            </div>

            <!-- FORMULAIRE INSCRIPTION (3 champs obligatoires) -->
            <form id="form-register" onsubmit="handleRegister(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">1. Nom complet *</label>
                    <input type="text" id="reg-name" required placeholder="Ex: Jean Rakoto" class="w-full px-3.5 py-2.5 bg-white border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">2. Numéro de téléphone (Identifiant unique) *</label>
                    <input type="tel" id="reg-phone" required placeholder="Ex: 034 12 345 67" class="w-full px-3.5 py-2.5 bg-white border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">3. Mot de passe (Hachage SHA-256) *</label>
                    <input type="password" id="reg-password" required minlength="6" placeholder="Minimum 6 caractères" class="w-full px-3.5 py-2.5 bg-white border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    <p class="text-[11px] text-emerald-600 mt-1">✓ Sécurisé par hachage SHA-256 côté client</p>
                </div>
                <button type="submit" class="w-full py-3 bg-gradient-to-r from-blue-700 to-emerald-600 text-white font-bold text-sm rounded-xl shadow-lg">Créer mon compte (+1 000 Ar offert)</button>
            </form>

            <!-- FORMULAIRE CONNEXION -->
            <form id="form-login" onsubmit="handleLogin(event)" class="space-y-4 hidden">
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Numéro de téléphone</label>
                    <input type="tel" id="login-phone" required placeholder="034 00 000 00" class="w-full px-3.5 py-2.5 bg-white border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Mot de passe</label>
                    <input type="password" id="login-password" required placeholder="••••••••" class="w-full px-3.5 py-2.5 bg-white border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-blue-500 focus:outline-none">
                </div>
                <button type="submit" class="w-full py-3 bg-blue-700 text-white font-bold text-sm rounded-xl shadow-lg">Se connecter</button>
            </form>

            <button type="button" onclick="loadDemoAccount()" class="mt-4 w-full py-2.5 bg-slate-200 text-slate-700 text-xs font-semibold rounded-xl">
                Connexion 1-Clic Démo (Jean Rakoto • 15 000 Ar)
            </button>
        </section>

        <!-- DASHBOARD (TABLEAU DE BORD) -->
        <main id="view-dashboard" class="hidden flex-1 px-4 py-4 space-y-4 pb-24">
            <div class="bg-gradient-to-br from-slate-900 via-blue-950 to-slate-900 text-white rounded-2xl p-5 shadow-xl border border-slate-800">
                <div class="flex items-center justify-between text-slate-400 text-xs">
                    <span>Solde disponible en Ariary</span>
                    <button onclick="toggleBalanceVisibility()" class="p-1">👁</button>
                </div>
                <div class="mt-2 flex items-baseline gap-2">
                    <span id="dash-balance-amount" class="text-3xl font-black text-white">15 000</span>
                    <span class="text-emerald-400 font-extrabold text-lg">Ar</span>
                </div>
                <div class="mt-3">
                    <div class="flex justify-between text-[11px] text-slate-300 mb-1">
                        <span>Seuil de retrait : <b class="text-emerald-400">5 000 Ar</b></span>
                        <span id="withdrawal-eligibility-badge" class="text-emerald-400 font-bold">✓ Éligible au retrait</span>
                    </div>
                    <div class="w-full h-1.5 bg-slate-800 rounded-full overflow-hidden">
                        <div id="balance-progress-bar" class="h-full bg-gradient-to-r from-emerald-500 to-blue-500" style="width: 100%;"></div>
                    </div>
                </div>
                <div class="mt-5 grid grid-cols-2 gap-2.5">
                    <button onclick="navigateTo('tasks')" class="bg-blue-600 text-white py-2.5 px-3 rounded-xl text-xs font-bold">Gagner plus</button>
                    <button onclick="navigateTo('wallet')" class="bg-emerald-600 text-white py-2.5 px-3 rounded-xl text-xs font-bold">Retirer Mobile Money</button>
                </div>
            </div>

            <!-- Statistiques -->
            <div class="grid grid-cols-3 gap-2">
                <div class="bg-white p-3 rounded-xl border border-slate-200 text-center"><p class="text-[10px] text-slate-500 font-semibold uppercase">Tâches Finies</p><p id="stat-completed" class="text-base font-extrabold text-blue-700">8</p></div>
                <div class="bg-white p-3 rounded-xl border border-slate-200 text-center"><p class="text-[10px] text-slate-500 font-semibold uppercase">En attente</p><p id="stat-pending" class="text-base font-extrabold text-amber-500">1</p></div>
                <div class="bg-white p-3 rounded-xl border border-slate-200 text-center"><p class="text-[10px] text-slate-500 font-semibold uppercase">Total Gains</p><p id="stat-total-earned" class="text-base font-extrabold text-emerald-600">23 500 Ar</p></div>
            </div>

            <div class="space-y-2">
                <h3 class="text-sm font-bold text-slate-800">Tâches en vedette</h3>
                <div id="dashboard-featured-tasks" class="space-y-2.5"></div>
            </div>
        </main>

        <!-- TÂCHES DISPONIBLES -->
        <section id="view-tasks" class="hidden flex-1 px-4 py-4 space-y-4 pb-24">
            <div>
                <h2 class="text-lg font-black text-slate-900">Tâches disponibles</h2>
                <p class="text-xs text-slate-500">Complétez les tâches et fournissez vos preuves de réalisation.</p>
            </div>
            <div class="flex gap-1.5 overflow-x-auto pb-1 no-scrollbar text-xs font-semibold">
                <button onclick="filterTasks('all')" class="task-filter-btn px-3 py-1.5 rounded-full bg-slate-900 text-white whitespace-nowrap" data-category="all">Toutes</button>
                <button onclick="filterTasks('video')" class="task-filter-btn px-3 py-1.5 rounded-full bg-slate-200 text-slate-700 whitespace-nowrap" data-category="video">🎬 Vidéos</button>
                <button onclick="filterTasks('social')" class="task-filter-btn px-3 py-1.5 rounded-full bg-slate-200 text-slate-700 whitespace-nowrap" data-category="social">📢 Réseaux Sociaux</button>
                <button onclick="filterTasks('survey')" class="task-filter-btn px-3 py-1.5 rounded-full bg-slate-200 text-slate-700 whitespace-nowrap" data-category="survey">📝 Sondages</button>
                <button onclick="filterTasks('app')" class="task-filter-btn px-3 py-1.5 rounded-full bg-slate-200 text-slate-700 whitespace-nowrap" data-category="app">📱 Test d'App</button>
            </div>
            <div id="tasks-list-container" class="space-y-3"></div>
        </section>

        <!-- SECTION RETRAITS MOBILE MONEY -->
        <section id="view-wallet" class="hidden flex-1 px-4 py-4 space-y-4 pb-24">
            <div>
                <h2 class="text-lg font-black text-slate-900">Retrait Mobile Money</h2>
                <p class="text-xs text-slate-500">Transférez vos gains en Ariary vers votre compte MVola, Orange Money ou Airtel Money.</p>
            </div>

            <div class="bg-white border border-slate-200 rounded-2xl p-4 shadow-sm">
                <span class="text-xs text-slate-500">Solde disponible :</span>
                <div class="flex items-baseline gap-1 mt-1">
                    <span id="withdraw-available-balance" class="text-2xl font-black text-slate-900">15 000</span>
                    <span class="text-sm font-bold text-emerald-600">Ar</span>
                </div>
            </div>

            <!-- Formulaire de retrait -->
            <form onsubmit="handleWithdrawalSubmit(event)" class="bg-white border border-slate-200 rounded-2xl p-4 space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-2">1. Opérateur Mobile Money</label>
                    <div class="grid grid-cols-3 gap-2">
                        <label class="cursor-pointer">
                            <input type="radio" name="momo_operator" value="mvola" checked class="peer sr-only" onchange="onOperatorSelect('mvola')">
                            <div class="p-2.5 rounded-xl border-2 peer-checked:border-emerald-600 peer-checked:bg-emerald-50 text-center font-bold text-xs text-slate-800">MVola (034/038)</div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="momo_operator" value="orange" class="peer sr-only" onchange="onOperatorSelect('orange')">
                            <div class="p-2.5 rounded-xl border-2 peer-checked:border-orange-500 peer-checked:bg-orange-50 text-center font-bold text-xs text-slate-800">Orange (032)</div>
                        </label>
                        <label class="cursor-pointer">
                            <input type="radio" name="momo_operator" value="airtel" class="peer sr-only" onchange="onOperatorSelect('airtel')">
                            <div class="p-2.5 rounded-xl border-2 peer-checked:border-rose-600 peer-checked:bg-rose-50 text-center font-bold text-xs text-slate-800">Airtel (033)</div>
                        </label>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">2. Numéro récepteur</label>
                    <input type="tel" id="withdraw-phone" required placeholder="034 12 345 67" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm">
                </div>

                <div>
                    <div class="flex justify-between items-center mb-1">
                        <label class="text-xs font-semibold text-slate-700">3. Montant à retirer (Ar)</label>
                        <button type="button" onclick="setWithdrawAmountMax()" class="text-[11px] font-bold text-blue-600">Tout retirer</button>
                    </div>
                    <input type="number" id="withdraw-amount" required min="5000" step="500" placeholder="Min. 5 000" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm font-bold" oninput="calculateWithdrawalSummary()">
                </div>

                <div class="bg-slate-50 p-3 rounded-xl border text-xs text-slate-600 space-y-1">
                    <div class="flex justify-between"><span>Montant :</span><span id="summary-requested-amount" class="font-semibold text-slate-800">0 Ar</span></div>
                    <div class="flex justify-between"><span>Frais :</span><span class="font-semibold text-emerald-600">0 Ar (Offert)</span></div>
                    <div class="flex justify-between border-t pt-1 font-bold"><span>Net reçu :</span><span id="summary-net-amount" class="text-emerald-600">0 Ar</span></div>
                </div>

                <button type="submit" class="w-full py-3 bg-gradient-to-r from-emerald-600 to-teal-700 text-white font-bold text-sm rounded-xl">Confirmer la demande de retrait</button>
            </form>

            <!-- Suivi des statuts (En attente -> Validé -> Payé) -->
            <div class="space-y-3 pt-2">
                <div class="flex items-center justify-between">
                    <h3 class="text-sm font-bold text-slate-900">Suivi des demandes</h3>
                    <button onclick="simulateStatusProgression()" class="text-[11px] bg-blue-50 text-blue-700 border px-2 py-1 rounded-lg">Simuler Statut ▶</button>
                </div>
                <div id="withdrawals-history-list" class="space-y-3"></div>
            </div>
        </section>

        <!-- PROFIL -->
        <section id="view-profile" class="hidden flex-1 px-4 py-4 space-y-4 pb-24">
            <h2 class="text-lg font-black text-slate-900">Mon Profil</h2>
            <div class="bg-white border rounded-2xl p-4 flex items-center space-x-3.5">
                <div class="w-12 h-12 rounded-full bg-blue-700 text-white font-extrabold flex items-center justify-center text-lg" id="profile-avatar-letter">J</div>
                <div>
                    <h3 id="profile-name" class="font-bold text-slate-900">Jean Rakoto</h3>
                    <p id="profile-phone" class="text-xs text-slate-500 font-mono">034 12 345 67</p>
                </div>
            </div>
            <button onclick="handleLogout()" class="w-full py-3 bg-rose-50 text-rose-600 font-bold text-xs rounded-xl border border-rose-200">Se déconnecter</button>
        </section>

        <!-- BARRE DE NAVIGATION INFÉRIEURE -->
        <nav id="bottom-nav" class="hidden fixed bottom-0 left-0 right-0 max-w-md mx-auto bg-white/95 backdrop-blur-md border-t border-slate-200 z-30 px-3 py-2 flex justify-around shadow-lg">
            <button onclick="navigateTo('dashboard')" id="nav-btn-dashboard" class="nav-item flex flex-col items-center text-blue-600 flex-1"><span class="text-xs">🏠</span><span class="text-[10px] font-bold">Accueil</span></button>
            <button onclick="navigateTo('tasks')" id="nav-btn-tasks" class="nav-item flex flex-col items-center text-slate-500 flex-1"><span class="text-xs">📋</span><span class="text-[10px] font-bold">Tâches</span></button>
            <button onclick="navigateTo('wallet')" id="nav-btn-wallet" class="nav-item flex flex-col items-center text-slate-500 flex-1"><span class="text-xs">💸</span><span class="text-[10px] font-bold">Retraits</span></button>
            <button onclick="navigateTo('profile')" id="nav-btn-profile" class="nav-item flex flex-col items-center text-slate-500 flex-1"><span class="text-xs">👤</span><span class="text-[10px] font-bold">Profil</span></button>
        </nav>

        <!-- MODAL SOUMISSION DE PREUVE -->
        <div id="modal-task-proof" class="hidden fixed inset-0 z-50 bg-slate-900/60 backdrop-blur-sm flex items-end sm:items-center justify-center p-0 sm:p-4">
            <div class="bg-white rounded-t-3xl sm:rounded-2xl max-w-md w-full p-5 space-y-4">
                <div class="flex items-center justify-between border-b pb-2">
                    <h3 id="modal-task-title" class="font-bold text-slate-900">Soumettre la preuve</h3>
                    <button onclick="closeTaskModal()" class="text-slate-400">✕</button>
                </div>
                <p id="modal-task-desc" class="text-xs text-slate-600"></p>
                <p id="modal-task-reward" class="text-sm font-black text-emerald-600"></p>
                <form onsubmit="handleProofSubmit(event)" class="space-y-3">
                    <input type="hidden" id="modal-task-id">
                    <div class="grid grid-cols-2 gap-2 text-xs">
                        <label class="p-2 border rounded-lg flex items-center gap-1 cursor-pointer"><input type="radio" name="proof_type" value="url" checked onchange="toggleProofInputType('url')"> Lien URL</label>
                        <label class="p-2 border rounded-lg flex items-center gap-1 cursor-pointer"><input type="radio" name="proof_type" value="file" onchange="toggleProofInputType('file')"> Capture d'écran</label>
                    </div>
                    <div id="proof-input-url-container">
                        <input type="<div align="center">

<img width="1200" height="475" alt="GHBanner" src="https://github.com/user-attachments/assets/0aa67016-6eaf-458a-adb2-6e31a0763ed6" />

  <h1>Built with AI Studio</h2>

  <p>The fastest path from prompt to production with Gemini.</p>

  <a href="https://aistudio.google.com/apps">Start building</a>

</div>
