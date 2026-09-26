<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tout est devant soi</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
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

        .hidden { display: none !important; }

        .card {
            background-color: #F5F0E6;
            border: 1px solid rgba(49, 27, 20, 0.1);
            border-radius: 16px;
            transition: all 0.2s ease;
        }

        /* Убираем стандартные чекбоксы и делаем эстетичные круглые */
        .custom-checkbox {
            appearance: none;
            width: 22px;
            height: 22px;
            border: 2px solid #311B14;
            border-radius: 6px;
            outline: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s ease;
        }

        .custom-checkbox:checked {
            background-color: #311B14;
        }

        .custom-checkbox:checked::after {
            content: "✓";
            color: #EDE6D8;
            font-size: 14px;
            font-weight: bold;
        }
    </style>
</head>
<body class="min-h-screen pb-24">
    <div class="max-w-md mx-auto p-5 flex flex-col min-h-screen justify-between">
        
        <div>
            <!-- Заголовок -->
            <header class="pt-8 pb-2 text-center">
                <h1 class="text-4xl title-font mb-2">Tout est devant soi</h1>
                <!-- Текущая дата -->
                <p id="current-date" class="text-[11px] uppercase tracking-widest opacity-60"></p>
            </header>

            <!-- Прогресс дня -->
            <div id="progress-container" class="mb-6 px-1">
                <div class="flex justify-between text-[11px] opacity-70 mb-1">
                    <span>Прогресс дня</span>
                    <span id="progress-percent">0%</span>
                </div>
                <div class="w-full bg-[#311B14]/10 h-[3px] rounded-full overflow-hidden">
                    <div id="progress-bar" class="bg-[#311B14] h-full transition-all duration-500" style="width: 0%"></div>
                </div>
            </div>

            <!-- Навигация -->
            <nav class="flex justify-around border-b border-[#311B14]/10 mb-6">
                <button onclick="switchTab('tasks')" id="tab-tasks" class="pb-3 px-2 text-xs uppercase tracking-widest tab-active">Задачи</button>
                <button onclick="switchTab('habits')" id="tab-habits" class="pb-3 px-2 text-xs uppercase tracking-widest tab-inactive">Привычки</button>
                <button onclick="switchTab('mood')" id="tab-mood" class="pb-3 px-2 text-lg tab-inactive transition-all">🤍</button>
            </nav>

            <!-- Основной контент -->
            <main>
                <!-- Секция Задач -->
                <div id="section-tasks" class="space-y-3">
                    <div id="tasks-list" class="space-y-3"></div>
                </div>

                <!-- Секция Привычек -->
                <div id="section-habits" class="space-y-3 hidden">
                    <div id="habits-list" class="space-y-3"></div>
                </div>

                <!-- Секция Мотивации (Доска Pinterest) -->
                <div id="section-mood" class="hidden">
                    <div id="images-grid" class="columns-2 gap-3 space-y-3">
                        <!-- Сюда вставится Pinterest сетка -->
                    </div>
                </div>
            </main>
        </div>

        <!-- Кнопка добавления -->
        <div class="fixed bottom-0 left-0 right-0 p-5 bg-[#EDE6D8]/90 backdrop-blur-md max-w-md mx-auto">
            <button onclick="openModal()" class="w-full bg-[#311B14] text-[#EDE6D8] py-4 rounded-2xl shadow-xl active:scale-95 transition-transform font-medium">
                + Добавить запись
            </button>
        </div>
    </div>

    <!-- КРАСИВОЕ МОДАЛЬНОЕ ОКНО ДОБАВЛЕНИЯ -->
    <div id="custom-modal" class="hidden fixed inset-0 bg-[#311B14]/40 backdrop-blur-sm flex items-center justify-center p-4 z-50">
        <div class="bg-[#F5F0E6] w-full max-w-xs rounded-3xl p-6 border border-[#311B14]/10 shadow-2xl">
            <h3 id="modal-title" class="text-base font-semibold mb-4 tracking-wide text-center">Добавить запись</h3>
            
            <input type="text" id="modal-input" 
                   class="w-full p-3 bg-[#EDE6D8] border border-[#311B14]/20 rounded-xl focus:outline-none focus:border-[#311B14] mb-5 text-sm" 
                   placeholder="Напишите здесь...">
            
            <div class="flex justify-end gap-3 text-xs uppercase tracking-wider">
                <button onclick="closeModal()" class="px-4 py-2 opacity-50 font-semibold">Отмена</button>
                <button onclick="saveModalItem()" class="px-5 py-2 bg-[#311B14] text-[#EDE6D8] rounded-xl font-semibold">ОК</button>
            </div>
        </div>
    </div>

    <script>
        // Инициализация данных
        let state = {
            activeTab: 'tasks',
            tasks: JSON.parse(localStorage.getItem('tasks')) || [],
            habits: JSON.parse(localStorage.getItem('habits')) || [],
            images: JSON.parse(localStorage.getItem('images')) || [
                "https://images.unsplash.com/photo-1515003197210-e0cd71810b5f?q=80&w=600&auto=format&fit=crop",
                "https://images.unsplash.com/photo-1490730141103-6cac27aaab94?q=80&w=600&auto=format&fit=crop",
                "https://images.unsplash.com/photo-1447752875215-b2761acb3c5d?q=80&w=600&auto=format&fit=crop"
            ]
        };

        // Получаем сегодняшнюю дату в формате YYYY-MM-DD
        function getTodayString() {
            const d = new Date();
            return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`;
        }

        // Вывод красивой даты на русском
        function renderDate() {
            const options = { weekday: 'long', day: 'numeric', month: 'long' };
            let dateStr = new Date().toLocaleDateString('ru-RU', options);
            document.getElementById('current-date').innerText = dateStr.charAt(0).toUpperCase() + dateStr.slice(1);
        }

        function saveData() {
            localStorage.setItem('tasks', JSON.stringify(state.tasks));
            localStorage.setItem('habits', JSON.stringify(state.habits));
            localStorage.setItem('images', JSON.stringify(state.images));
        }

        // Переключение вкладок
        function switchTab(tabName) {
            state.activeTab = tabName;
            
            ['tasks', 'habits', 'mood'].forEach(t => {
                const btn = document.getElementById(`tab-${t}`);
                const sec = document.getElementById(`section-${t}`);
                
                if (t === tabName) {
                    if (t === 'mood') {
                        btn.className = "pb-3 px-2 text-lg tab-active scale-110";
                    } else {
                        btn.className = "pb-3 px-2 text-xs uppercase tracking-widest tab-active";
                    }
                    sec.classList.remove('hidden');
                } else {
                    if (t === 'mood') {
                        btn.className = "pb-3 px-2 text-lg tab-inactive";
                    } else {
                        btn.className = "pb-3 px-2 text-xs uppercase tracking-widest tab-inactive";
                    }
                    sec.classList.add('hidden');
                }
            });
            render();
        }

        // ЛОГИКА КРАСИВОГО МОДАЛЬНОГО ОКНА
        function openModal() {
            const modal = document.getElementById('custom-modal');
            const title = document.getElementById('modal-title');
            const input = document.getElementById('modal-input');
            
            input.value = '';
            
            if (state.activeTab === 'tasks') {
                title.innerText = "Новая задача";
                input.placeholder = "Например: Выпить воды";
            } else if (state.activeTab === 'habits') {
                title.innerText = "Новая привычка";
                input.placeholder = "Например: Чтение книги";
            } else if (state.activeTab === 'mood') {
                title.innerText = "Добавить фото";
                input.placeholder = "Вставьте ссылку на картинку из интернета";
            }
            
            modal.classList.remove('hidden');
            input.focus();
        }

        function closeModal() {
            document.getElementById('custom-modal').classList.add('hidden');
        }

        function saveModalItem() {
            const val = document.getElementById('modal-input').value.trim();
            if (val) {
                if (state.activeTab === 'tasks') {
                    state.tasks.push({ text: val, done: false });
                } else if (state.activeTab === 'habits') {
                    state.habits.push({ text: val, streak: 0, lastCompleted: "" });
                } else if (state.activeTab === 'mood') {
                    state.images.push(val);
                }
                saveData();
                render();
            }
            closeModal();
        }

        // Удаление элементов
        function deleteItem(type, index) {
            state[type].splice(index, 1);
            saveData();
            render();
        }

        // Переключение задачи
        function toggleTask(index) {
            state.tasks[index].done = !state.tasks[index].done;
            saveData();
            render();
            triggerVibration();
        }

        // Переключение привычки (Галочка на один день)
        function toggleHabit(index) {
            const habit = state.habits[index];
            const today = getTodayString();
            
            if (habit.lastCompleted === today) {
                // Если уже была выполнена сегодня - отменяем выполнение
                habit.lastCompleted = "";
                habit.streak = Math.max(0, habit.streak - 1);
            } else {
                // Выполняем сегодня
                habit.lastCompleted = today;
                habit.streak += 1;
                triggerVibration();
            }
            saveData();
            render();
        }

        // Вибрация Telegram (Haptic Feedback)
        function triggerVibration() {
            if (window.Telegram && window.Telegram.WebApp && window.Telegram.WebApp.HapticFeedback) {
                window.Telegram.WebApp.HapticFeedback.impactOccurred('medium');
            }
        }

        // Отрисовка всего интерфейса
        function render() {
            const today = getTodayString();

            // 1. Рендер Задач
            const tasksList = document.getElementById('tasks-list');
            tasksList.innerHTML = state.tasks.map((t, i) => `
                <div class="card flex items-center p-4 shadow-sm">
                    <input type="checkbox" ${t.done ? 'checked' : ''} onchange="toggleTask(${i})" class="custom-checkbox">
                    <span class="ml-4 flex-grow text-sm ${t.done ? 'line-through opacity-40' : ''}">${t.text}</span>
                    <button onclick="deleteItem('tasks', ${i})" class="text-xs opacity-25 p-1">✕</button>
                </div>
            `).join('');

            // 2. Рендер Привычек (с галочкой и автосбросом в 00:00)
            const habitsList = document.getElementById('habits-list');
            habitsList.innerHTML = state.habits.map((h, i) => {
                const isCompletedToday = (h.lastCompleted === today);
                
                return `
                    <div class="card flex justify-between items-center p-4 shadow-sm">
                        <div class="flex items-center">
                            <input type="checkbox" ${isCompletedToday ? 'checked' : ''} onchange="toggleHabit(${i})" class="custom-checkbox">
                            <div class="ml-4">
                                <p class="font-medium text-sm ${isCompletedToday ? 'line-through opacity-40' : ''}">${h.text}</p>
                                <p class="text-[10px] uppercase tracking-wider opacity-60 mt-0.5">Серия: ${h.streak} 🔥</p>
                            </div>
                        </div>
                        <button onclick="deleteItem('habits', ${i})" class="text-xs opacity-25 p-1">✕</button>
                    </div>
                `;
            }).join('');

            // 3. Рендер Мотивации (Стиль Pinterest Доски)
            const grid = document.getElementById('images-grid');
            grid.innerHTML = state.images.map((img, i) => `
                <div class="break-inside-avoid mb-3 card overflow-hidden relative group">
                    <img src="${img}" class="w-full object-cover rounded-2xl block" onerror="this.src='https://images.unsplash.com/photo-1513542789411-b6a5d4f31634?q=80&w=600'">
                    <button onclick="deleteItem('images', ${i})" class="absolute top-2 right-2 bg-[#EDE6D8]/80 text-[#311B14] rounded-full w-6 h-6 text-[10px] flex items-center justify-center shadow-md">✕</button>
                </div>
            `).join('');

            // 4. Расчет Прогресс-бара дня
            const totalTasks = state.tasks.length;
            const completedTasks = state.tasks.filter(t => t.done).length;
            const percent = totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;
            
            document.getElementById('progress-percent').innerText = `${percent}%`;
            document.getElementById('progress-bar').style.width = `${percent}%`;
        }

        // Запуск
        renderDate();
        render();

        // Подключение Telegram WebApp
        if (window.Telegram && window.Telegram.WebApp) {
            const tg = window.Telegram.WebApp;
            tg.expand();
            tg.ready();
        }
    </script>
</body>
</html>
