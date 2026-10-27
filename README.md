# tris.py
def stampa_tabellone(tabellone):
    print("\n")
    print(f" {tabellone[0]} | {tabellone[1]} | {tabellone[2]} ")
    print("---|---|---")
    print(f" {tabellone[3]} | {tabellone[4]} | {tabellone[5]} ")
    print("---|---|---")
    print(f" {tabellone[6]} | {tabellone[7]} | {tabellone[8]} ")
    print("\n")

def controlla_vittoria(tabellone, giocatore):
    condizioni_vittoria = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], # Orizzontali
        [0, 3, 6], [1, 4, 7], [2, 5, 8], # Verticali
        [0, 4, 8], [2, 4, 6]             # Diagonali
    ]
    for condizione in condizioni_vittoria:
        if tabellone[condizione[0]] == tabellone[condizione[1]] == tabellone[condizione[2]] == giocatore:
            return True
    return False

def gioco():
    tabellone = [" " for _ in range(9)]
    giocatore_attuale = "X"
    mosse = 0

    print("Benvenuto al Gioco del Tris!")
    print("Le posizioni sono numerate da 1 a 9, partendo in alto a sinistra.")
    stampa_tabellone(["1", "2", "3", "4", "5", "6", "7", "8", "9"])

    while mosse < 9:
        stampa_tabellone(tabellone)
        try:
            mossa = int(input(f"Turno del giocatore {giocatore_attuale}. Scegli una posizione (1-9): ")) - 1
        except ValueError:
            print("Inserisci un numero valido!")
            continue

        if mossa < 0 or mossa > 8 or tabellone[mossa] != " ":
            print("Mossa non valida o posizione già occupata. Riprova.")
            continue

        tabellone[mossa] = giocatore_attuale
        mosse += 1

        if controlla_vittoria(tabellone, giocatore_attuale):
            stampa_tabellone(tabellone)
            print(f"Congratulazioni! Il giocatore {giocatore_attuale} ha vinto!")
            return

        giocatore_attuale = "O" if giocatore_attuale == "X" else "X"

    stampa_tabellone(tabellone)
    print("Pareggio! Ottima partita.")

if __name__ == "__main__":
    gioco()
