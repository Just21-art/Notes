<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Мои заметки v2.0</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: white;
            padding: 20px;
            min-height: 100vh;
        }
        h1 {
            color: #ffd700;
            text-align: center;
            margin-bottom: 20px;
            font-size: 28px;
            text-shadow: 0 0 20px rgba(255, 215, 0, 0.5);
        }
        .input-box {
            display: flex;
            gap: 8px;
            margin-bottom: 20px;
        }
        input {
            flex: 1;
            padding: 12px;
            font-size: 16px;
            border-radius: 10px;
            border: 2px solid transparent;
            background: #0f3460;
            color: white;
            outline: none;
            transition: 0.3s;
        }
        input:focus { border-color: #ffd700; }
        button {
            padding: 12px 18px;
            font-size: 16px;
            background: #ffd700;
            color: #1a1a2e;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
        }
        button:hover { transform: scale(1.05); background: #ffed4e; }
        ul { list-style: none; }
        li {
            background: #16213e;
            margin: 12px 0;
            padding: 15px;
            border-radius: 12px;
            border-left: 4px solid #ffd700;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            animation: slideIn 0.3s ease;
        }
        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }
        .note-text { font-size: 16px; margin-bottom: 5px; }
        .note-date { font-size: 12px; color: #888; }
        .note-buttons { margin-top: 10px; }
        .del {
            background: #e94560;
            color: white;
            font-size: 14px;
            padding: 6px 12px;
            margin-right: 8px;
        }
        .edit {
            background: #0f3460;
            color: white;
            font-size: 14px;
            padding: 6px 12px;
        }
        .empty {
            text-align: center;
            color: #666;
            padding: 40px;
            font-style: italic;
        }
    </style>
</head>
<body>
    <h1>📝 Мои заметки v2.0</h1>
    <div class="input-box">
        <input id="text" placeholder="Напиши заметку...">
        <button onclick="add()">Добавить</button>
    </div>
    <ul id="list"></ul>

    <script>
        let notes = JSON.parse(localStorage.getItem("notes") || "[]");

        function save() {
            localStorage.setItem("notes", JSON.stringify(notes));
        }

        function add() {
            let text = document.getElementById("text").value;
            if (text === "") return;
            notes.push({
                text: text,
                date: new Date().toLocaleString("ru-RU")
            });
            document.getElementById("text").value = "";
            save();
            show();
        }

        function show() {
            let list = document.getElementById("list");
            if (notes.length === 0) {
                list.innerHTML = "<div class='empty'>📭 Заметок пока нет</div>";
                return;
            }
            list.innerHTML = "";
            for (let i = 0; i < notes.length; i++) {
                list.innerHTML += `
                    <li>
                        <div class="note-text">${notes[i].text}</div>
                        <div class="note-date">🕐 ${notes[i].date}</div>
                        <div class="note-buttons">
                            <button class="edit" onclick="edit(${i})">✏️ Изменить</button>
                            <button class="del" onclick="del(${i})">❌ Удалить</button>
                        </div>
                    </li>
                `;
            }
        }

        function edit(i) {
            let newText = prompt("Измени заметку:", notes[i].text);
            if (newText !== null && newText !== "") {
                notes[i].text = newText;
                notes[i].date = notes[i].date + " (изменено)";
                save();
                show();
            }
        }

        function del(i) {
            notes.splice(i, 1);
            save();
            show();
        }

        show();
    </script>
</body>
</html>
