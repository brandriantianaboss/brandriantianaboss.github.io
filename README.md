<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Compte Rendu d'Activité Quotidien</title>
    <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&family=Playfair+Display:wght@600;700&display=swap" rel="stylesheet">
    <style>
        /* ============================================================
           CONFIGURATION : Remplacez par l'URL de votre Apps Script
           ============================================================ */

        :root {
            --primary: #1a2742;
            --primary-light: #2a3f6a;
            --accent: #e8913a;
            --accent-hover: #d47e2a;
            --surface: #ffffff;
            --surface-alt: #f6f8fb;
            --border: #dfe4ed;
            --border-focus: #2a3f6a;
            --text: #1a2742;
            --text-muted: #6b7a94;
            --text-light: #8d9bb5;
            --success: #2a9d6e;
            --success-bg: #e8f7f0;
            --danger: #d44040;
            --danger-bg: #fdf0f0;
            --warning: #e8913a;
            --warning-bg: #fef5eb;
            --info: #3b82c4;
            --info-bg: #eef4fb;
            --shadow-sm: 0 1px 3px rgba(26,39,66,0.06);
            --shadow-md: 0 4px 16px rgba(26,39,66,0.08);
            --shadow-lg: 0 8px 32px rgba(26,39,66,0.12);
            --radius: 10px;
            --radius-lg: 16px;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'DM Sans', sans-serif;
            background: var(--surface-alt);
            color: var(--text);
            min-height: 100vh;
            line-height: 1.6;
        }

        /* ---- Header ---- */
        .header {
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            padding: 2.5rem 2rem 3.5rem;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -20%;
            width: 400px;
            height: 400px;
            background: radial-gradient(circle, rgba(232,145,58,0.15) 0%, transparent 70%);
            border-radius: 50%;
        }

        .header::after {
            content: '';
            position: absolute;
            bottom: -60%;
            left: -10%;
            width: 300px;
            height: 300px;
            background: radial-gradient(circle, rgba(255,255,255,0.05) 0%, transparent 70%);
            border-radius: 50%;
        }

        .header h1 {
            font-family: 'Playfair Display', serif;
            font-size: 1.9rem;
            font-weight: 700;
            color: #ffffff;
            letter-spacing: -0.02em;
            position: relative;
            z-index: 1;
        }

        .header p {
            color: rgba(255,255,255,0.65);
            font-size: 0.92rem;
            margin-top: 0.5rem;
            font-weight: 400;
            position: relative;
            z-index: 1;
        }

        .header .accent-bar {
            width: 48px;
            height: 3px;
            background: var(--accent);
            border-radius: 3px;
            margin: 1rem auto 0;
            position: relative;
            z-index: 1;
        }

        /* ---- Main container ---- */
        .container {
            max-width: 740px;
            margin: -2rem auto 3rem;
            padding: 0 1.25rem;
            position: relative;
            z-index: 2;
        }

        /* ---- Card ---- */
        .card {
            background: var(--surface);
            border-radius: var(--radius-lg);
            box-shadow: var(--shadow-md);
            padding: 2rem;
            margin-bottom: 1.25rem;
        }

        .card-title {
            font-family: 'Playfair Display', serif;
            font-size: 1.15rem;
            font-weight: 600;
            color: var(--primary);
            margin-bottom: 1.5rem;
            display: flex;
            align-items: center;
            gap: 0.6rem;
        }

        .card-title .icon {
            width: 32px;
            height: 32px;
            background: var(--surface-alt);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem;
            flex-shrink: 0;
        }

        /* ---- Form fields ---- */
        .field-row {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1rem;
        }

        @media (max-width: 560px) {
            .field-row { grid-template-columns: 1fr; }
        }

        .field {
            margin-bottom: 1.15rem;
        }

        .field label {
            display: block;
            font-size: 0.82rem;
            font-weight: 600;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.06em;
            margin-bottom: 0.4rem;
        }

        .field label .optional {
            font-weight: 400;
            text-transform: none;
            color: var(--text-light);
            font-size: 0.78rem;
            letter-spacing: 0;
        }

        .field input,
        .field textarea,
        .field select {
            width: 100%;
            padding: 0.7rem 0.9rem;
            font-family: 'DM Sans', sans-serif;
            font-size: 0.92rem;
            color: var(--text);
            background: var(--surface-alt);
            border: 1.5px solid var(--border);
            border-radius: var(--radius);
            transition: border-color 0.2s, box-shadow 0.2s;
            outline: none;
        }

        .field input:focus,
        .field textarea:focus,
        .field select:focus {
            border-color: var(--border-focus);
            box-shadow: 0 0 0 3px rgba(42,63,106,0.08);
        }

        .field input[readonly] {
            background: var(--surface-alt);
            color: var(--text-muted);
            cursor: default;
        }

        .field textarea {
            resize: vertical;
            min-height: 80px;
        }

        .field select {
            cursor: pointer;
            appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg width='12' height='8' viewBox='0 0 12 8' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1 1.5L6 6.5L11 1.5' stroke='%236b7a94' stroke-width='1.5' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 0.9rem center;
            padding-right: 2.5rem;
        }

        /* ---- Task block ---- */
        .tasks-list {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .task-item {
            background: var(--surface-alt);
            border: 1.5px solid var(--border);
            border-radius: var(--radius);
            padding: 1.25rem;
            position: relative;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(-8px); }
            to   { opacity: 1; transform: translateY(0); }
        }

        .task-item .task-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 1rem;
        }

        .task-number {
            font-size: 0.78rem;
            font-weight: 700;
            color: var(--accent);
            background: var(--warning-bg);
            padding: 0.2rem 0.65rem;
            border-radius: 20px;
            letter-spacing: 0.04em;
        }

        .btn-remove {
            background: none;
            border: none;
            color: var(--text-light);
            cursor: pointer;
            font-size: 1.15rem;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 6px;
            transition: all 0.2s;
        }

        .btn-remove:hover {
            background: var(--danger-bg);
            color: var(--danger);
        }

        .task-item .field {
            margin-bottom: 0.85rem;
        }

        .task-item .field:last-child {
            margin-bottom: 0;
        }

        .task-row {
            display: grid;
            grid-template-columns: 1fr 120px;
            gap: 0.85rem;
        }

        @media (max-width: 560px) {
            .task-row { grid-template-columns: 1fr; }
        }

        /* ---- Add task button ---- */
        .btn-add {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            width: 100%;
            padding: 0.85rem;
            margin-top: 1rem;
            background: transparent;
            border: 2px dashed var(--border);
            border-radius: var(--radius);
            color: var(--primary-light);
            font-family: 'DM Sans', sans-serif;
            font-size: 0.9rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .btn-add:hover {
            border-color: var(--accent);
            color: var(--accent);
            background: var(--warning-bg);
        }

        .btn-add .plus {
            font-size: 1.2rem;
            line-height: 1;
        }

        /* ---- Submit button ---- */
        .btn-submit {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 0.6rem;
            width: 100%;
            padding: 1rem;
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            color: #ffffff;
            border: none;
            border-radius: var(--radius);
            font-family: 'DM Sans', sans-serif;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.25s;
            box-shadow: var(--shadow-sm);
            position: relative;
            overflow: hidden;
        }

        .btn-submit:hover {
            box-shadow: var(--shadow-lg);
            transform: translateY(-1px);
        }

        .btn-submit:active {
            transform: translateY(0);
        }

        .btn-submit:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .btn-submit .spinner {
            display: none;
            width: 18px;
            height: 18px;
            border: 2.5px solid rgba(255,255,255,0.3);
            border-top-color: #ffffff;
            border-radius: 50%;
            animation: spin 0.7s linear infinite;
        }

        .btn-submit.loading .spinner { display: block; }
        .btn-submit.loading .btn-label { display: none; }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* ---- Toast / Messages ---- */
        .toast {
            position: fixed;
            top: 1.5rem;
            right: 1.5rem;
            padding: 1rem 1.5rem;
            border-radius: var(--radius);
            font-size: 0.9rem;
            font-weight: 500;
            box-shadow: var(--shadow-lg);
            z-index: 1000;
            transform: translateX(120%);
            transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
            max-width: 360px;
        }

        .toast.show {
            transform: translateX(0);
        }

        .toast.success {
            background: var(--success-bg);
            color: var(--success);
            border-left: 4px solid var(--success);
        }

        .toast.error {
            background: var(--danger-bg);
            color: var(--danger);
            border-left: 4px solid var(--danger);
        }

        /* ---- Footer ---- */
        .footer {
            text-align: center;
            padding: 1rem;
            color: var(--text-light);
            font-size: 0.78rem;
        }
    </style>
</head>
<body>

    <!-- Header -->
    <div class="header">
        <h1>Compte Rendu d'Activité</h1>
        <p>Rapport quotidien — Suivi des tâches et avancement</p>
        <div class="accent-bar"></div>
    </div>

    <!-- Main form -->
    <div class="container">

        <!-- Identification -->
        <div class="card">
            <div class="card-title">
                <span class="icon">👤</span>
                Identification
            </div>
            <div class="field-row">
                <div class="field">
                    <label for="matricule">Matricule</label>
                    <input type="number" id="matricule" placeholder="Ex : 10042" required>
                </div>
                <div class="field">
                    <label for="email">Email</label>
                    <input type="email" id="email" placeholder="prenom.nom@entreprise.com" required>
                </div>
            </div>
            <div class="field">
                <label for="date">Date du rapport</label>
                <input type="text" id="date" readonly>
            </div>
        </div>

        <!-- Tâches -->
        <div class="card">
            <div class="card-title">
                <span class="icon">📋</span>
                Tâches réalisées
            </div>
            <div class="tasks-list" id="tasksList">
                <!-- Les tâches sont ajoutées dynamiquement -->
            </div>
            <button type="button" class="btn-add" onclick="addTask()">
                <span class="plus">+</span> Ajouter une tâche
            </button>
        </div>

        <!-- Compléments -->
        <div class="card">
            <div class="card-title">
                <span class="icon">💬</span>
                Informations complémentaires
            </div>
            <div class="field">
                <label for="difficultes">Difficultés rencontrées <span class="optional">(optionnel)</span></label>
                <textarea id="difficultes" rows="3" placeholder="Décrivez les blocages ou difficultés..."></textarea>
            </div>
            <div class="field">
                <label for="remarques">Remarques / Besoins <span class="optional">(optionnel)</span></label>
                <textarea id="remarques" rows="3" placeholder="Besoins en ressources, suggestions..."></textarea>
            </div>
        </div>

        <!-- Soumission -->
        <button type="button" class="btn-submit" id="btnSubmit" onclick="submitForm()">
            <span class="spinner"></span>
            <span class="btn-label">Soumettre le compte rendu</span>
        </button>

        <div class="footer">
            Système de suivi d'activité — Les données sont enregistrées dans Google Sheets.
        </div>
    </div>

    <!-- Toast notification -->
    <div class="toast" id="toast"></div>

    <script>
        // ================================================================
        //  ⚙️ CONFIGURATION — Remplacez par l'URL de votre Apps Script
        // ================================================================
        const SCRIPT_URL = "https://script.google.com/macros/s/VOTRE_SCRIPT_ID/exec";
        // ================================================================

        let taskCounter = 0;

        // --- Remplir la date du jour au format français ---
        function setTodayDate() {
            const now = new Date();
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('date').value = now.toLocaleDateString('fr-FR', options);
        }

        // --- Ajouter une tâche ---
        function addTask() {
            taskCounter++;
            const taskHTML = `
                <div class="task-item" id="task-${taskCounter}" data-task-id="${taskCounter}">
                    <div class="task-header">
                        <span class="task-number">TÂCHE ${taskCounter}</span>
                        <button type="button" class="btn-remove" onclick="removeTask(${taskCounter})" title="Supprimer cette tâche">✕</button>
                    </div>
                    <div class="field">
                        <label>Intitulé de la tâche</label>
                        <input type="text" class="task-title" placeholder="Ex : Rédaction du rapport mensuel" required>
                    </div>
                    <div class="field">
                        <label>Description détaillée</label>
                        <textarea class="task-desc" rows="2" placeholder="Détaillez le travail effectué..."></textarea>
                    </div>
                    <div class="task-row">
                        <div class="field">
                            <label>Temps consacré</label>
                            <input type="text" class="task-time" placeholder="Ex : 2h30">
                        </div>
                        <div class="field">
                            <label>Statut</label>
                            <select class="task-status">
                                <option value="Terminé">Terminé</option>
                                <option value="En cours" selected>En cours</option>
                                <option value="Bloqué">Bloqué</option>
                                <option value="Reporté">Reporté</option>
                            </select>
                        </div>
                    </div>
                </div>
            `;
            document.getElementById('tasksList').insertAdjacentHTML('beforeend', taskHTML);
        }

        // --- Supprimer une tâche ---
        function removeTask(id) {
            const el = document.getElementById(`task-${id}`);
            if (el) {
                el.style.opacity = '0';
                el.style.transform = 'translateY(-8px)';
                el.style.transition = 'all 0.25s ease';
                setTimeout(() => {
                    el.remove();
                    renumberTasks();
                }, 250);
            }
        }

        // --- Renuméroter les tâches après suppression ---
        function renumberTasks() {
            const tasks = document.querySelectorAll('.task-item');
            tasks.forEach((task, index) => {
                task.querySelector('.task-number').textContent = `TÂCHE ${index + 1}`;
            });
        }

        // --- Collecter les tâches ---
        function collectTasks() {
            const items = document.querySelectorAll('.task-item');
            const tasks = [];
            items.forEach((item, index) => {
                const title = item.querySelector('.task-title').value.trim();
                const desc  = item.querySelector('.task-desc').value.trim();
                const time  = item.querySelector('.task-time').value.trim();
                const status = item.querySelector('.task-status').value;
                if (title === '') return; // ignorer les tâches sans intitulé
                tasks.push({
                    numero: index + 1,
                    intitule: title,
                    description: desc,
                    temps: time,
                    statut: status
                });
            });
            return tasks;
        }

        // --- Validation ---
        function validate() {
            const matricule = document.getElementById('matricule').value.trim();
            const email = document.getElementById('email').value.trim();
            const tasks = collectTasks();

            if (!matricule) { showToast('Veuillez renseigner votre matricule.', 'error'); return null; }
            if (!email || !email.includes('@')) { showToast('Veuillez renseigner un email valide.', 'error'); return null; }
            if (tasks.length === 0) { showToast('Ajoutez au moins une tâche avec un intitulé.', 'error'); return null; }

            return {
                matricule: matricule,
                email: email,
                date: document.getElementById('date').value,
                taches: tasks,
                difficultes: document.getElementById('difficultes').value.trim(),
                remarques: document.getElementById('remarques').value.trim()
            };
        }

        // --- Soumission ---
        async function submitForm() {
            const data = validate();
            if (!data) return;

            const btn = document.getElementById('btnSubmit');
            btn.classList.add('loading');
            btn.disabled = true;

            try {
                const response = await fetch(SCRIPT_URL, {
                    method: 'POST',
                    mode: 'no-cors', // nécessaire pour Apps Script
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(data)
                });

                // Avec mode 'no-cors', on ne peut pas lire la réponse.
                // On considère l'envoi comme réussi si pas d'erreur réseau.
                showToast('✓ Compte rendu envoyé avec succès !', 'success');
                resetForm();

            } catch (error) {
                console.error('Erreur:', error);
                showToast('Erreur lors de l\'envoi. Vérifiez votre connexion.', 'error');
            } finally {
                btn.classList.remove('loading');
                btn.disabled = false;
            }
        }

        // --- Réinitialiser le formulaire ---
        function resetForm() {
            document.getElementById('matricule').value = '';
            document.getElementById('email').value = '';
            document.getElementById('difficultes').value = '';
            document.getElementById('remarques').value = '';
            document.getElementById('tasksList').innerHTML = '';
            taskCounter = 0;
            // Ré-ajouter une tâche vide
            setTimeout(() => addTask(), 400);
        }

        // --- Toast notification ---
        function showToast(message, type) {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.className = `toast ${type} show`;
            setTimeout(() => { toast.classList.remove('show'); }, 4000);
        }

        // --- Initialisation ---
        document.addEventListener('DOMContentLoaded', () => {
            setTodayDate();
            addTask(); // une tâche par défaut
        });
    </script>

</body>
</html>
