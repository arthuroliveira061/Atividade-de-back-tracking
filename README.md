#include <iostream>
using namespace std;

// Define o tamanho do labirinto (N x N)
#define N 4

// ==========================================
// ALGORITMO DE BACKTRACKING: LABIRINTO
// ==========================================

// Objetivo da função auxiliar: imprimir a matriz de forma legível no console.
void imprimir_solucao(int solucao[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            cout << solucao[i][j] << " ";
        }
        cout << endl;
    }
}

// Objetivo da função auxiliar: verificar se o movimento atual é válido.
bool eh_valido(int labirinto[N][N], int x, int y) {
    // Critério usado para verificar se uma escolha parcial é válida:
    // A posição (x,y) deve estar dentro dos limites da matriz (0 a N-1) 
    // e o bloco não pode ser uma parede (deve ser 1).
    if (x >= 0 && x < N && y >= 0 && y < N && labirinto[x][y] == 1) {
        return true;
    }
    return false;
}

// Declaração prévia da função recursiva
bool resolver_labirinto_util(int labirinto[N][N], int x, int y, int solucao[N][N]);

// Objetivo da função principal: Inicializar a matriz de solução e iniciar a busca.
bool resolver_labirinto(int labirinto[N][N]) {
    // Inicializa a matriz de solução com zeros (caminho vazio)
    int solucao[N][N] = { {0, 0, 0, 0},
                          {0, 0, 0, 0},
                          {0, 0, 0, 0},
                          {0, 0, 0, 0} };

    if (resolver_labirinto_util(labirinto, 0, 0, solucao) == false) {
        cout << "Nao existe solucao para este labirinto." << endl;
        return false;
    }

    cout << "Caminho encontrado:" << endl;
    imprimir_solucao(solucao);
    return true;
}

// Função auxiliar que contém a lógica recursiva do Backtracking
bool resolver_labirinto_util(int labirinto[N][N], int x, int y, int solucao[N][N]) {
    
    // Condição de parada da recursão: 
    // O algoritmo atingiu o destino (canto inferior direito: N-1, N-1) em um bloco livre.
    if (x == N - 1 && y == N - 1 && labirinto[x][y] == 1) {
        solucao[x][y] = 1; // Confirma o último passo da solução
        return true;
    }

    // Verifica se a célula atual atende ao critério de escolha parcial
    if (eh_valido(labirinto, x, y)) {
        
        // Evita ciclos verificando se já visitou esta célula na solução atual
        if (solucao[x][y] == 1) {
            return false;
        }

        // Escolha: marca o caminho atual como parte da solução parcial (tentativa)
        solucao[x][y] = 1;

        // Momento em que o algoritmo avança para uma nova tentativa:
        // 1º Tenta avançar para BAIXO (x + 1)
        if (resolver_labirinto_util(labirinto, x + 1, y, solucao) == true) {
            return true;
        }

        // Momento em que o algoritmo avança para uma nova tentativa:
        // 2º Tenta avançar para a DIREITA (y + 1)
        if (resolver_labirinto_util(labirinto, x, y + 1, solucao) == true) {
            return true;
        }

        // Se avançar para baixo ou para a direita não levar à saída, a escolha atual foi ruim.
        // Momento em que ocorre o retrocesso (Backtracking) para desfazer uma escolha inadequada:
        solucao[x][y] = 0; // Desmarca a célula atual (apaga o rastro)
        return false;
    }

    // Retorna falso se a célula não for válida (bateu em parede ou limite)
    return false;
}

int main() {
    // Matriz 4x4 representando o labirinto (1 = livre, 0 = parede)
    int labirinto[N][N] = { {1, 0, 0, 0},
                            {1, 1, 0, 1},
                            {0, 1, 0, 0},
                            {1, 1, 1, 1} };

    resolver_labirinto(labirinto);
    return 0;
}
