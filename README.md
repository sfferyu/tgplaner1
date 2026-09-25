[index (1).html](https://github.com/user-attachments/files/32669436/index.1.html)
# tgplaner1<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tout est devant soi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:italic&display=swap');

        body { 
            background-color: #EDE6D8; 
            color: #311B14; 
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }

        .title-font { 
            font-family: 'Times New Roman', Times, serif; 
            font-style: italic; 
            letter-spacing: 0.05em;
        }

        .tab-active { 
            border-bottom: 2px solid #311B14; 
            color: #311B14; 
            font-weight: 600; 
        }

        .tab-inactive {
            color: #8C7A70;
        }

        .hidden { display: none; }

        .card {
            background-color: #F5F0E6;
            border: 1px solid rgba(49, 27, 20, 0.1);
            border-radius: 16px;
            transition: all 0.2s ease;
        }

        /* Убираем стандартные стили чекбоксов */
        input[type="checkbox"] {
            accent-color: #311B14;
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
    </style>
</head>
<body class="min-h-screen">
    <div class="max-w-md mx-auto p-5 flex flex-col min-h-screen">
        
        <!-- Заголовок -->
        <header class="py-8">
            <h1 class="text-4xl title-font text-center">Tout est devant soi</h1>
        </header>

        <!-- Навигация -->
        <nav class="flex justify-around border-b border-[#311B14]/10 mb-8">
            <button onclick="switchTab('tasks')" id="tab-tasks" class="pb-3 px-2 text-sm uppercase tracking-widest tab-active">Задачи</button>
            <button onclick="switchTab('habits')" id="tab-habits" class="pb-3 px-2 text-sm uppercase tracking-widest tab-inactive">Привычки</button>
            <button onclick="switchTab('mood')" id="tab-mood" class="pb-3 px-2 text-sm uppercase tracking-widest tab-inactive">Мотивация</button>
        </nav>

        <!-- Основной контент -->
        <main class="flex-grow">
            <!-- Секция Задач -->
            <div id="section-tasks" class="space-y-3">
                <div id="tasks-list" class="space-y-3"></div>
            </div>

            <!-- Секция Привычек -->
            <div id="section-habits" class="space-y-3 hidden">
                <div id="habits-list" class="space-y-3"></div>
            </div>

            <!-- Секция Мотивации -->
            <div id="section-mood" class="space-y-4 hidden">
                <div id="images-grid" class="grid grid-cols-2 gap-3"></div>
            </div>
        </main>

        <!-- Кнопка добавления -->
        <footer class="mt-8">
            <button onclick="handleAddItem()" class="w-full bg-[#311B14] text-[#EDE6D8] py-4 rounded-2xl shadow-xl active:scale-95 transition-transform font-medium">
                + Добавить запись
            </button>
        </footer>
    </div>

    <script>
        // Инициализация данных из памяти (LocalStorage)
        let state = {
            activeTab: 'tasks',
            tasks: JSON.parse(localStorage.getItem('tasks')) || [],
            habits: JSON.parse(localStorage.getItem('habits')) || [],
            images: JSON.parse(localStorage.getItem('images')) || [
                "https://images.unsplash.com/photo-1518495973542-4542c06a5843?q=80&w=600&auto=format&fit=crop",
                "https://images.unsplash.com/photo-1506126613408-eca07ce68773?q=80&w=600&auto=format&fit=crop"
            ]
        };

        function saveData() {
            localStorage.setItem('tasks', JSON.stringify(state.tasks));
            localStorage.setItem('habits', JSON.stringify(state.habits));
            localStorage.setItem('images', JSON.stringify(state.images));
        }

        // Переключение вкладок
        function switchTab(tabName) {
            state.activeTab = tabName;
            
            // Стили кнопок
            ['tasks', 'habits', 'mood'].forEach(t => {
                const btn = document.getElementById(`tab-${t}`);
                const sec = document.getElementById(`section-${t}`);
                if (t === tabName) {
                    btn.className = "pb-3 px-2 text-sm uppercase tracking-widest tab-active";
                    sec.classList.remove('hidden');
                } else {
                    btn.className = "pb-3 px-2 text-sm uppercase tracking-widest tab-inactive";
                    sec.classList.add('hidden');
                }
            });
            render();
        }

        // Логика добавления
        function handleAddItem() {
            if (state.activeTab === 'tasks') {
                const val = prompt("Какую задачу добавим?");
                if (val) state.tasks.push({ text: val, done: false });
            } else if (state.activeTab === 'habits') {
                const val = prompt("Название новой привычки?");
                if (val) state.habits.push({ text: val, streak: 0 });
            } else if (state.activeTab === 'mood') {
                const val = prompt("Вставь ссылку на картинку из интернета:");
                if (val) state.images.push(val);
            }
            saveData();
            render();
        }

        // Удаление
        function deleteItem(type, index) {
            if (confirm("Удалить эту запись?")) {
                state[type].splice(index, 1);
                saveData();
                render();
            }
        }

        // Переключение чекбокса
        function toggleTask(index) {
            state.tasks[index].done = !state.tasks[index].done;
            saveData();
            render();
        }

        // Отрисовка
        function render() {
            // Рендер задач
            const tasksList = document.getElementById('tasks-list');
            tasksList.innerHTML = state.tasks.map((t, i) => `
                <div class="card flex items-center p-4">
                    <input type="checkbox" ${t.done ? 'checked' : ''} onchange="toggleTask(${i})">
                    <span class="ml-4 flex-grow ${t.done ? 'line-through opacity-50' : ''}">${t.text}</span>
                    <button onclick="deleteItem('tasks', ${i})" class="text-xs opacity-30">✕</button>
                </div>
            `).join('');

            // Рендер привычек
            const habitsList = document.getElementById('habits-list');
            habitsList.innerHTML = state.habits.map((h, i) => `
                <div class="card flex justify-between items-center p-4">
                    <div>
                        <p class="font-medium">${h.text}</p>
                        <p class="text-xs opacity-60">Прогресс: ${h.streak} дн.</p>
                    </div>
                    <div class="flex items-center gap-3">
                        <button onclick="state.habits[${i}].streak++; saveData(); render();" class="bg-[#311B14]/5 px-3 py-1 rounded-lg text-sm">🔥 +1</button>
                        <button onclick="deleteItem('habits', ${i})" class="text-xs opacity-30 ml-2">✕</button>
                    </div>
                </div>
            `).join('');

            // Рендер мотивации
            const grid = document.getElementById('images-grid');
            grid.innerHTML = state.images.map((img, i) => `
                <div class="aspect-[3/4] card overflow-hidden relative group">
                    <img src="${img}" class="w-full h-full object-cover">
                    <button onclick="deleteItem('images', ${i})" class="absolute top-2 right-2 bg-white/50 rounded-full w-6 h-6 text-xs shadow-sm">✕</button>
                </div>
            `).join('');
        }

        // Запуск при старте
        render();

        // Подключение к Telegram
        if (window.Telegram && window.Telegram.WebApp) {
            const tg = window.Telegram.WebApp;
            tg.expand();
            tg.ready();
        }
    </script>
</body>
</html>
