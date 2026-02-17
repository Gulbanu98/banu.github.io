<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Энергосбыт — Панель оператора</title>

<style>
body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: #f2f6fb;
}

/* Верхняя панель */
.topbar {
    background: #005baa;
    color: white;
    padding: 15px 30px;
    font-size: 18px;
    display: flex;
    justify-content: space-between;
}

/* Основной контейнер */
.dashboard {
    display: flex;
    height: calc(100vh - 55px);
}

/* Левая панель */
.sidebar {
    width: 250px;
    background: white;
    border-right: 1px solid #ddd;
    padding: 20px;
}

.sidebar h3 {
    color: #005baa;
}

.sidebar button {
    width: 100%;
    padding: 12px;
    margin-bottom: 10px;
    border: none;
    border-radius: 8px;
    background: #005baa;
    color: white;
    cursor: pointer;
}

.sidebar button:hover {
    background: #0077e6;
}

/* Чат */
.chat-area {
    flex: 1;
    display: flex;
    flex-direction: column;
}

.chat-header {
    padding: 15px;
    background: #e9f2fb;
    border-bottom: 1px solid #ddd;
    font-weight: bold;
}

.chat-box {
    flex: 1;
    padding: 20px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.message {
    padding: 10px 15px;
    border-radius: 15px;
    max-width: 70%;
}

.client {
    background: #dfefff;
    align-self: flex-start;
}

.operator {
    background: #005baa;
    color: white;
    align-self: flex-end;
}

.chat-input {
    display: flex;
    padding: 15px;
    border-top: 1px solid #ddd;
    background: white;
}

.chat-input input {
    flex: 1;
    padding: 10px;
    border-radius: 8px;
    border: 1px solid #ccc;
}

.chat-input button {
    margin-left: 10px;
    padding: 10px 20px;
    border: none;
    background: #005baa;
    color: white;
    border-radius: 8px;
    cursor: pointer;
}
</style>
</head>

<body>

<div class="topbar">
⚡ Энергосбыт — Симулятор оператора
</div>

<div class="dashboard">

<div class="sidebar">
<h3>Обращения</h3>
<button onclick="acceptChat()">📥 Принять чат</button>
<button onclick="resetChat()">🔄 Новый диалог</button>
</div>

<div class="chat-area">
<div class="chat-header">Клиент онлайн</div>
<div class="chat-box" id="chatBox"></div>

<div class="chat-input">
<input type="text" id="userInput" placeholder="Введите сообщение...">
<button onclick="sendMessage()">Отправить</button>
</div>
</div>

</div>

<script>
const chatBox = document.getElementById("chatBox");
const userInput = document.getElementById("userInput");

let dialogStage = "start";
let problemType = null;

function addMessage(text, role="client") {
    const msg = document.createElement("div");
    msg.classList.add("message");
    msg.classList.add(role === "client" ? "client" : "operator");
    msg.innerText = text;
    chatBox.appendChild(msg);
    chatBox.scrollTop = chatBox.scrollHeight;
}

function acceptChat() {
    addMessage("Здравствуйте. У меня вопрос по начислениям за электроэнергию.", "client");
    dialogStage = "identify";
}

function resetChat() {
    chatBox.innerHTML = "";
    dialogStage = "start";
    problemType = null;
}

function analyzeIntent(text) {
    const lower = text.toLowerCase();

    if (dialogStage === "identify") {
        if (lower.includes("начис")  lower.includes("счет")  lower.includes("платеж")) {
            problemType = "billing";
            return "billing";
        }
        if (lower.includes("счетчик") || lower.includes("показан")) {
            problemType = "meter";
            return "meter";
        }
        return "clarify";
    }

    if (dialogStage === "clarify") {
        return "details";
    }

    return "continue";
}

function generateResponse(intent) {

    if (dialogStage === "identify") {
        dialogStage = "clarify";
        if (intent === "billing") {
            return "Понимаю. Подскажите, пожалуйста, за какой период возникли вопросы по начислениям?";
        }
        if (intent === "meter") {
return "Уточните, пожалуйста, передавали ли вы последние показания счетчика?";
        }
        return "Уточните, пожалуйста, в чем именно возникла сложность?";
    }

    if (dialogStage === "clarify") {
        dialogStage = "solution";
        if (problemType === "billing") {
            return "Спасибо за информацию. Я проверю корректность начислений. Возможно потребуется сверка показаний. Желаете оформить заявку на проверку?";
        }
        if (problemType === "meter") {
            return "Рекомендую проверить корректность передачи показаний. Если есть сомнения — можем оформить заявку на диагностику.";
        }
        return "Благодарю за уточнение. Сейчас разберемся.";
    }

    if (dialogStage === "solution") {
        dialogStage = "end";
        return "Ваша заявка принята в работу. Есть ли еще вопросы, с которыми я могу помочь?";
    }

    return "Спасибо за обращение!";
}

function sendMessage() {
    const text = userInput.value.trim();
    if (!text) return;

    addMessage(text, "operator");
    userInput.value = "";

    setTimeout(() => {
        const intent = analyzeIntent(text);
        const response = generateResponse(intent);
        addMessage(response, "client");
    }, 700);
}

userInput.addEventListener("keypress", e => {
    if (e.key === "Enter") sendMessage();
});
</script>

</body>
</html>
