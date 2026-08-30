<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Nominate Zone</title>
    <style>
        :root {
            --bg-color: #f1f3f5;        /* สีเทาอ่อนมาก สำหรับพื้นหลังหน้าจอ */
            --card-bg: #ffffff;         /* สีขาว สำหรับกล่องข้อความตรงกลาง */
            --border-color: #dee2e6;     /* สีเทาอ่อน สำหรับเส้นขอบ */
            --text-color: #343a40;       /* สีเทาเข้มเกือบดำ สำหรับตัวหนังสือ */
            --subtitle-color: #6c757d;   /* สีเทากลาง สำหรับคำอธิบาย */
            --accent-color: #ffd43b;     /* สีเหลืองสว่างหลัก */
            --accent-hover: #fab005;     /* สีเหลืองตอนเอาเมาส์วาง */
            --btn-text: #212529;         /* สีตัวอักษรบนปุ่มเหลือง */
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .container {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 32px;
            width: 100%;
            max-width: 500px;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.05);
        }

        p.subtitle {
            color: var(--subtitle-color);
            text-align: center;
            margin-top: 0;
            margin-bottom: 24px;
            font-size: 15px;
            font-weight: 500;
        }

        .input-group {
            display: flex;
            gap: 8px;
            margin-bottom: 24px;
        }

        input[type="text"] {
            flex: 1;
            background-color: #f8f9fa;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 12px 14px;
            font-size: 14px;
            color: var(--text-color);
            outline: none;
            transition: all 0.2s;
        }

        input[type="text"]:focus {
            background-color: #ffffff;
            border-color: var(--accent-hover);
            box-shadow: 0 0 0 3px rgba(250, 176, 5, 0.2);
        }

        button {
            background-color: var(--accent-color);
            color: var(--btn-text);
            border: 1px solid rgba(0, 0, 0, 0.05);
            border-radius: 8px;
            padding: 12px 24px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        button:hover {
            background-color: var(--accent-hover);
            transform: translateY(-1px);
        }

        button:active {
            transform: translateY(0);
        }

        .zone-title {
            font-size: 14px;
            font-weight: 600;
            margin-bottom: 12px;
            border-bottom: 2px solid var(--border-color);
            padding-bottom: 8px;
            color: var(--subtitle-color);
        }

        ul {
            list-style: none;
            padding: 0;
            margin: 0;
            max-height: 300px;
            overflow-y: auto;
        }

        li {
            background-color: #f8f9fa;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 12px 16px;
            margin-bottom: 8px;
            font-size: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            animation: fadeIn 0.3s ease;
            border-left: 4px solid var(--accent-color);
        }

        .delete-btn {
            background: none;
            border: none;
            color: #e03131;
            cursor: pointer;
            font-size: 13px;
            padding: 0;
            font-weight: 500;
        }

        .delete-btn:hover {
            text-decoration: underline;
        }

        .empty-state {
            color: var(--subtitle-color);
            text-align: center;
            font-style: italic;
            font-size: 14px;
            padding: 24px 0;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-5px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body>

    <div class="container">
        <p class="subtitle">Enter a name below to add them to the zone.</p>

        <div class="input-group">
            <input type="text" id="nameInput" placeholder="Type a name..." autocomplete="off">
            <button onclick="addNomination()">Nominate</button>
        </div>

        <div class="zone-title">Nominated People</div>
        <ul id="nomineeList"></ul>
    </div>

    <script>
        let nominees = JSON.parse(localStorage.getItem('nominees')) || [];

        function renderNominees() {
            const list = document.getElementById('nomineeList');
            list.innerHTML = '';

            if (nominees.length === 0) {
                list.innerHTML = '<div class="empty-state">No one has been nominated yet.</div>';
                return;
            }

            nominees.forEach((name, index) => {
                const li = document.createElement('li');
                li.innerHTML = `
                    <span>${name}</span>
                    <button class="delete-btn" onclick="removeNomination(${index})">Remove</button>
                `;
                list.appendChild(li);
            });
        }

        function addNomination() {
            const input = document.getElementById('nameInput');
            const name = input.value.trim();

            if (name === '') {
                alert('Please enter a valid name.');
                return;
            }

            nominees.push(name);
            localStorage.setItem('nominees', JSON.stringify(nominees));
            input.value = '';
            renderNominees();
        }

        function removeNomination(index) {
            nominees.splice(index, 1);
            localStorage.setItem('nominees', JSON.stringify(nominees));
            renderNominees();
        }

        document.getElementById('nameInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addNomination();
            }
        });

        renderNominees();
    </script>
</body>
</html>
