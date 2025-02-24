<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Placar Exato - Brasileirão 2025</title>
    <style>
        body {
            font-family: 'Roboto', Arial, sans-serif;
            background: linear-gradient(135deg, #1e90ff, #32cd32);
            color: #333;
            margin: 0;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .container {
            background: rgba(255, 255, 255, 0.95);
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
            max-width: 900px;
            width: 100%;
            transition: all 0.3s ease;
        }
        h1 {
            color: #007bff;
            font-size: 2em;
            margin-bottom: 20px;
        }
        h2 {
            color: #444;
            font-size: 1.5em;
            margin-top: 20px;
        }
        .palpites-container {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            gap: 20px;
        }
        .input-group {
            flex: 1 1 45%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 10px 0;
            font-size: 1.1em;
            background: #f8f9fa;
            padding: 10px;
            border-radius: 8px;
        }
        input[type="number"] {
            width: 50px;
            padding: 6px;
            margin: 0 5px;
            border: 2px solid #ced4da;
            border-radius: 8px;
            text-align: center;
            font-size: 1em;
            transition: border-color 0.3s ease;
        }
        input[type="number"]:focus {
            border-color: #007bff;
            outline: none;
        }
        input[type="text"], input[type="password"] {
            width: 200px;
            padding: 10px;
            margin: 10px 0;
            border: 2px solid #ced4da;
            border-radius: 8px;
            font-size: 1em;
        }
        button {
            background: #007bff;
            color: white;
            border: none;
            padding: 12px 25px;
            margin: 10px;
            border-radius: 8px;
            font-size: 1em;
            cursor: pointer;
            transition: background 0.3s ease, transform 0.2s ease;
        }
        button:hover {
            background: #0056b3;
            transform: scale(1.05);
        }
        .ranking {
            margin-top: 25px;
            background: #f1f3f5;
            padding: 15px;
            border-radius: 10px;
            text-align: left;
        }
        .ranking h3 {
            margin: 0 0 10px;
            color: #007bff;
        }
        .ranking p {
            margin: 5px 0;
            font-size: 1.1em;
        }
        .toggle-link {
            color: #007bff;
            cursor: pointer;
            text-decoration: underline;
        }
        @media (max-width: 600px) {
            .container {
                padding: 20px;
            }
            h1 {
                font-size: 1.5em;
            }
            .input-group {
                flex: 1 1 100%;
            }
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <script>
        let usuarios = {};
        let jogos = [
            { id: 1, timeA: "Flamengo", timeB: "Internacional" },
            { id: 2, timeA: "Vasco", timeB: "Santos" },
            { id: 3, timeA: "Palmeiras", timeB: "Botafogo" },
            { id: 4, timeA: "São Paulo", timeB: "Sport" },
            { id: 5, timeA: "Red Bull Bragantino", timeB: "Ceará" },
            { id: 6, timeA: "Cruzeiro", timeB: "Mirassol" },
            { id: 7, timeA: "Grêmio", timeB: "Atlético-MG" },
            { id: 8, timeA: "Bahia", timeB: "Corinthians" },
            { id: 9, timeA: "Fortaleza", timeB: "Fluminense" },
            { id: 10, timeA: "Juventude", timeB: "Vitória" }
        ];
        let palpites = [];
        let ranking = {};

        function showCadastro() {
            document.getElementById("login-form").style.display = "none";
            document.getElementById("cadastro-form").style.display = "block";
        }

        function showLogin() {
            document.getElementById("cadastro-form").style.display = "none";
            document.getElementById("login-form").style.display = "block";
        }

        function cadastrar() {
            let usuario = document.getElementById("cadastro-usuario").value;
            let senha = document.getElementById("cadastro-senha").value;
            if (!usuario || !senha) {
                alert("Por favor, insira nome e senha!");
                return;
            }
            if (usuarios[usuario]) {
                alert("Usuário já existe! Escolha outro nome.");
                return;
            }
            usuarios[usuario] = senha;
            alert("Cadastro realizado com sucesso! Faça login.");
            showLogin();
        }

        function login() {
            let usuario = document.getElementById("login-usuario").value;
            let senha = document.getElementById("login-senha").value;
            if (!usuario || !senha) {
                alert("Por favor, insira nome e senha!");
                return;
            }
            if (usuarios[usuario] && usuarios[usuario] === senha) {
                document.getElementById("login-form").style.display = "none";
                document.getElementById("placar-exato").style.display = "block";
                document.getElementById("usuarioAtual").value = usuario;
            } else {
                alert("Usuário ou senha incorretos!");
            }
        }

        function registrarPalpite() {
            let usuario = document.getElementById("usuarioAtual").value;
            jogos.forEach(jogo => {
                let palpiteA = document.getElementById(`palpiteA-${jogo.id}`).value;
                let palpiteB = document.getElementById(`palpiteB-${jogo.id}`).value;
                if (palpiteA && palpiteB) {
                    palpites.push({ usuario, jogoId: jogo.id, palpiteA, palpiteB });
                }
            });
            alert("Palpite registrado com sucesso!");
        }

        function calcularPontos() {
            ranking = {};
            palpites.forEach(({ usuario, jogoId, palpiteA, palpiteB }) => {
                let pontos = Math.floor(Math.random() * 6); // Simulação de pontos
                ranking[usuario] = (ranking[usuario] || 0) + pontos;
            });
            exibirRanking();
        }

        function exibirRanking() {
            let rankingDiv = document.getElementById("ranking");
            rankingDiv.innerHTML = "<h3>Ranking da Rodada</h3>";
            Object.entries(ranking).sort((a, b) => b[1] - a[1]).forEach(([usuario, pontos]) => {
                rankingDiv.innerHTML += `<p>${usuario}: ${pontos} pontos</p>`;
            });
        }
    </script>
</head>
<body>
    <div class="container" id="login-form">
        <h1>Placar Exato - Login</h1>
        <label for="login-usuario">Nome:</label><br>
        <input type="text" id="login-usuario" required><br>
        <label for="login-senha">Senha:</label><br>
        <input type="password" id="login-senha" required><br>
        <button onclick="login()">Entrar</button>
        <p>Ainda não tem conta? <span class="toggle-link" onclick="showCadastro()">Cadastre-se</span></p>
    </div>

    <div class="container" id="cadastro-form" style="display: none;">
        <h1>Placar Exato - Cadastro</h1>
        <label for="cadastro-usuario">Nome:</label><br>
        <input type="text" id="cadastro-usuario" required><br>
        <label for="cadastro-senha">Senha:</label><br>
        <input type="password" id="cadastro-senha" required><br>
        <button onclick="cadastrar()">Cadastrar</button>
        <p>Já tem conta? <span class="toggle-link" onclick="showLogin()">Faça login</span></p>
    </div>

    <div class="container" id="placar-exato" style="display: none;">
        <h1>Placar Exato - Brasileirão 2025</h1>
        <input type="hidden" id="usuarioAtual">
        <h2>Dê seus palpites</h2>
        <div class="palpites-container" id="palpites">
            <script>
                jogos.forEach(jogo => {
                    document.write(`
                        <div class="input-group">
                            ${jogo.timeA} 
                            <input type="number" id="palpiteA-${jogo.id}" min="0" placeholder="0"> x 
                            <input type="number" id="palpiteB-${jogo.id}" min="0" placeholder="0"> 
                            ${jogo.timeB}
                        </div>
                    `);
                });
            </script>
        </div>
        <button onclick="registrarPalpite()">Enviar Palpites</button>
        <button onclick="calcularPontos()">Ver Ranking</button>
        <div id="ranking" class="ranking"></div>
    </div>
</body>
</html>
