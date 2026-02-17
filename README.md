html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Мой Сайт на GitHub</title>
    <style>
        /* Основные стили */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            line-height: 1.6;
            color: #333;
            scroll-behavior: smooth;
        }

        /* Навигация */
        nav {
            background: #2d3436;
            color: white;
            padding: 1rem;
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 1000;
            display: flex;
            justify-content: center;
        }

        nav a {
            color: white;
            text-decoration: none;
            margin: 0 15px;
            font-weight: bold;
            transition: color 0.3s;
        }

        nav a:hover {
            color: #0984e3;
        }

        /* Главный блок (Hero) */
        header {
            height: 100vh;
            background: linear-gradient(rgba(0,0,0,0.6), rgba(0,0,0,0.6)), 
                        url('https://images.unsplash.com');
            background-size: cover;
            background-position: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            text-align: center;
            padding: 0 20px;
        }

        .btn {
            background: #0984e3;
            color: white;
            padding: 12px 30px;
            text-decoration: none;
            border-radius: 25px;
            font-size: 1.2rem;
            transition: transform 0.3s, background 0.3s;
            margin-top: 20px;
            display: inline-block;
        }

        .btn:hover {
            background: #74b9ff;
            transform: scale(1.05);
        }

        /* Секции контента */
        section {
            padding: 100px 20px;
            max-width: 800px;
            margin: 0 auto;
            text-align: center;
        }

        #about { background: #f9f9f9; }

        footer {
            background: #2d3436;
            color: white;
            text-align: center;
            padding: 20px;
        }
    </style>
</head>
<body>

    <nav>
        <a href="#home">Главная</a>
        <a href="#about">Обо мне</a>
        <a href="#contact">Контакты</a>
    </nav>

    <header id="home">
        <h1>Добро пожаловать на мой сайт!</h1>
        <p>Создан с помощью нейросети и GitHub Pages</p>
        <a href="#about" class="btn">Узнать больше</a>
    </header>

    <section id="about">
        <h2>Обо мне</h2>
        <p>Привет! Это мой первый проект, размещенный на GitHub. Здесь я буду делиться своими идеями и наработками.</p>
    </section>

    <section id="contact">
        <h2>Контакты</h2>
        <p>Свяжитесь со мной через GitHub или социальные сети.</p>
        <a href="https://github.com" target="_blank" class="btn" style="background: #2d3436;">Мой GitHub</a>
    </section>

    <footer>
        <p>&copy; 2024 Мой Сайт. Сделано с любовью.</p>
    </footer> 
    <!-- Виджет чата -->
    <div id="chat-container" style="position: fixed; bottom: 20px; right: 20px; width: 300px; background: white; border: 1px solid #ccc; border-radius: 10px; box-shadow: 0 5px 15px rgba(0,0,0,0.2); display: flex; flex-direction: column; overflow: hidden; font-family: sans-serif; z-index: 2000;">
        <div style="background: #0984e3; color: white; padding: 10px; font-weight: bold;">Чат-помощник</div>
        <div id="chat-box" style="height: 250px; overflow-y: auto; padding: 10px; font-size: 0.9rem; display: flex; flex-direction: column; gap: 8px; background: #fff;"></div>
        <div style="display: flex; border-top: 1px solid #eee;">
            <input type="text" id="user-input" placeholder="Напишите привет..." style="flex: 1; border: none; padding: 10px; outline: none;">
            <button onclick="sendMessage()" style="background: #0984e3; color: white; border: none; padding: 10px; cursor: pointer;">➤</button>
        </div>
    </div>
    html
    <script>
        const chatBox = document.getElementById('chat-box');
        const userInput = document.getElementById('user-input');

        function addMessage(text, isUser = false) {
            const msg = document.createElement('div');
            msg.innerText = text;
            msg.style.padding = '8px 12px';
            msg.style.borderRadius = '15px';
            msg.style.maxWidth = '85%';
            msg.style.marginBottom = '5px';
            msg.style.alignSelf = isUser ? 'flex-end' : 'flex-start';
            msg.style.background = isUser ? '#0984e3' : '#e9ecef';
            msg.style.color = isUser ? 'white' : 'black';
            chatBox.appendChild(msg);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function getBotResponse(text) {
            const input = text.toLowerCase();
            // Логика ответов:
            if (input.includes('привет')) return 'Привет! Рад тебя видеть на моем сайте. Как дела?';
            if (input.includes('хорошо')) return 'Это отлично! Чем я могу тебе помочь сегодня?';
            if (input.includes('сайт')) return 'Я помогу тебе сориентироваться. Что именно интересно?';
            if (input.includes('автор')) return 'Этот сайт создал будущий крутой разработчик!';
            return 'Я пока только учусь, но звучит интересно! Можешь спросить про "сайт" или просто сказать "привет".';
        }

        function sendMessage() {
            const text = userInput.value.trim();
            if (!text) return;
            addMessage(text, true);
            userInput.value = '';
            setTimeout(() => {
                addMessage(getBotResponse(text));
            }, 600);
        }

        userInput.addEventListener('keypress', (e) => { if (e.key === 'Enter') sendMessage(); });

        // Первая фраза при загрузке
        window.onload = () => {
            setTimeout(() => addMessage('Привет! Я твой помощник. Напиши мне что-нибудь!'), 1000);
        };
    </script>
</body>
</html>
