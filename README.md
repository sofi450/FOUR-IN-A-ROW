# FOUR-IN-A-ROW
trabajo de pensamiento algoritmico

#include <iostream>
using namespace std;

void imprimirTablero(char tablero[6][7]) {
    cout << endl << " 1 2 3 4 5 6 7 " << endl;
    for (int fila = 0; fila < 6; fila++) {
        for (int col = 0; col < 7; col++) {
            if (tablero[fila][col] == ' ') cout << "|.";
            else cout << "|" << tablero[fila][col];
        }
        cout << "|" << endl;
    }
}

int colocarFicha(char tablero[6][7], int col, char ficha) {
    for (int fila = 5; fila >= 0; fila--) {
        if (tablero[fila][col] == ' ') {
            tablero[fila][col] = ficha;
            return fila; // fila donde se colocó la ficha
        }
    }
    return -1; // columna llena
}

bool hayVictoria(char tablero[6][7], int f, int c, char ficha) {
    int contador, i, j;

    // Horizontal
    contador = 0;
    for (j = 0; j < 7; j++) {
        if (tablero[f][j] == ficha) contador++;
        else contador = 0;
        if (contador == 4) return true;
    }

    // Vertical
    contador = 0;
    for (i = 0; i < 6; i++) {
        if (tablero[i][c] == ficha) contador++;
        else contador = 0;
        if (contador == 4) return true;
    }

    // Diagonal descendente (\)
    i = f; j = c;
    while (i > 0 && j > 0) { i--; j--; }
    contador = 0;
    while (i < 6 && j < 7) {
        if (tablero[i][j] == ficha) contador++;
        else contador = 0;
        if (contador == 4) return true;
        i++; j++;
    }

    // Diagonal ascendente (/)
    i = f; j = c;
    while (i < 5 && j > 0) { i++; j--; }
    contador = 0;
    while (i >= 0 && j < 7) {
        if (tablero[i][j] == ficha) contador++;
        else contador = 0;
        if (contador == 4) return true;
        i--; j++;
    }

    return false;
}

bool hayEmpate(char tablero[6][7]) {
    for (int col = 0; col < 7; col++) {
        if (tablero[0][col] == ' ') return false;
    }
    return true;
}

int main() {
    char tablero[6][7];
    int fila, col;
    int jugar = 1; // Control del bucle general

    while (jugar == 1) {
        int turno = 1;

        // inicia con el tablero vacío
        for (fila = 0; fila < 6; fila++) {
            for (col = 0; col < 7; col++) {
                tablero[fila][col] = ' ';
            }
        }

        int terminado = 0; // para terminar o interrumpir el juego

        while (terminado == 0) {
            // imprime el tablero usando la función
            imprimirTablero(tablero);

            // le pide al usuario su jugada
            cout << " Turno del jugador ";
            if (turno == 1) cout << "X"; else cout << "O";
            cout << ". Ingrese columna (1-7)(0 = terminar juego): ";
            int c;
            if (!(cin >> c)) return 0;
            if (c == 0) {
                cout << "Juego interrumpido." << endl;
                return 0;
            }
            if (c < 1 || c > 7) {
                cout << "Columna invalida." << endl;
                continue;
            }

            c = c - 1; // posiciones de 0 a 6

            // intenta colocar la ficha usando la función
            char ficha = (turno == 1 ? 'X' : 'O');
            int f = colocarFicha(tablero, c, ficha);
            if (f == -1) {
                cout << " Columna llena" << endl;
                continue;
            }

            // valida victoria usando la función
            if (hayVictoria(tablero, f, c, ficha)) {
                imprimirTablero(tablero);
                cout << " Gana jugador " << ficha << endl;
                terminado = 1;
                continue;
            }

            // verifica empate
            if (hayEmpate(tablero)) {
                imprimirTablero(tablero);
                cout << " Empate " << endl;
                terminado = 1;
                continue;
            }

            // cambiar turno si no terminó
            turno = (turno == 1 ? 2 : 1);
        }

        // preguntar si jugar otra vez
        cout << "\n¿Quieres jugar otra vez? (1 = sí, 0 = salir): ";
        if (!(cin >> jugar)) break;
    }

    cout << "Gracias por jugar." << endl;
    return 0;
}
