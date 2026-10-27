# tris.html
<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gioco del Tris</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --text-color: #f8fafc;
            --grid-color: #334155;
            --x-color: #38bdf8;
            --o-color: #f43f5e;
            --win-bg: #22c55e;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 0.5rem;
            letter-spacing: 1px;
        }

        #status {
            font-size: 1.25rem;
            margin-bottom: 2rem;
            font-weight: 500;
            min-height: 30px;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 10px;
            background-color: var(--grid-color);
            padding: 10px;
            border-radius: 12px;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
        }

        .cell {
            background-color: #1e293b;
            border: none;
            border-radius: 8px;
            font-size: 2.5rem;
            font-weight: bold;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            transition: background-color 0.2s, transform 0.1s;
        }

        .cell:hover {
            background-color: #475569;
        }

        .cell:active {
            transform: scale(0.95);
        }

        .cell.X {
            color: var(--x-color);
        }

        .cell.O {
            color: var(--o-color);
        }

        .cell.winner {
            background-color: var(--win-bg);
            color: white;
            animation: pulse 1s infinite alternate;
        }

        #reset-btn {
            margin-top: 2rem;
            background-color: #3b82f6;
            color: white;
            border: none;
            padding: 12px 28px;
            font-size: 1rem;
            font-weight: 600;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.2s, transform 0.2s;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
        }

        #reset-btn:hover {
            background-color: #2563eb;
            transform: translateY(-2px);
        }

        #reset-btn:active {
            transform: translateY(0);
        }

        @keyframes pulse {
            from { transform: scale(1); }
            to { transform: scale(1.05); }
        }
    </style>
</head>
<body>

    <h1>Tris Interattivo</h1>
    <div id="status">Tocca una casella per iniziare. Turno di: X</div>

    <div class="board" id="board">
        <button class="cell" data-index="0"></button>
        <button class="cell" data-index="1"></button>
        <button class="cell" data-index="2"></button>
        <button class="cell" data-index="3"></button>
        <button class="cell" data-index="4"></button>
        <button class="cell" data-index="5"></button>
        <button class="cell" data-index="6"></button>
        <button class="cell" data-index="7"></button>
        <button class="cell" data-index="8"></button>
    </div>

    <button id="reset-btn" onclick="resetGioco()">Ricomincia Partita</button>

    <script>
        const boardElement = document.getElementById('board');
        const statusElement = document.getElementById('status');
        const cells = document.querySelectorAll('.cell');
        
        let tabellone = ["", "", "", "", "", "", "", "", ""];
        let giocatoreAttuale = "X";
        let giocoAttivo = true;

        // Array corretto contenente tutte le combinazioni per vincere
        const condizioniVittoria = [, [3, 4, 5], [6, 7, 8], // Orizzontali, [1, 4, 7], [2, 5, 8], // Verticali, [2, 4, 6]             // Diagonali
        ];

        cells.forEach(cell => {
            cell.addEventListener('click', () => mossaGiocatore(cell));
        });

        function mossaGiocatore(cell) {
            const index = cell.getAttribute('data-index');

            // Se la casella è già occupata o il gioco è finito, non fare nulla
            if (tabellone[index] !== "" || !giocoAttivo) return;

            // Aggiorna lo stato del tabellone e la grafica della cella
            tabellone[index] = giocatoreAttuale;
            cell.textContent = giocatoreAttuale;
            cell.classList.add(giocatoreAttuale);

            controllaEsito();
        }

        function controllaEsito() {
            let roundVinto = false;
            let combinazioneVincente = [];

            // Verifica se il giocatore attuale ha fatto tris
            for (let i = 0; i < condizioniVittoria.length; i++) {
                const [a, b, c] = condizioniVittoria[i];
                if (tabellone[a] !== "" && tabellone[a] === tabellone[b] && tabellone[a] === tabellone[c]) {
                    roundVinto = true;
                    combinazioneVincente = [a, b, c];
                    break;
                }
            }

            // Gestione Vittoria
            if (roundVinto) {
                statusElement.textContent = `🎉 Il giocatore ${giocatoreAttuale} ha vinto!`;
                combinazioneVincente.forEach(index => cells[index].classList.add('winner'));
                giocoAttivo = false;
                return;
            }

            // Gestione Pareggio
            if (!tabellone.includes("")) {
                statusElement.textContent = "🤝 Pareggio! Ottima partita.";
                giocoAttivo = false;
                return;
            }

            // Cambio turno corretto tra X e O
            giocatoreAttuale = giocatoreAttuale === "X" ? "O" : "X";
            statusElement.textContent = `Turno del giocatore: ${giocatoreAttuale}`;
        }

        function resetGioco() {
            tabellone = ["", "", "", "", "", "", "", "", ""];
            giocatoreAttuale = "X";
            giocoAttivo = true;
            statusElement.textContent = `Turno del giocatore: ${giocatoreAttuale}`;
            
            cells.forEach(cell => {
                cell.textContent = "";
                cell.className = "cell"; 
            });
        }
    </script>

</body>
</html>
