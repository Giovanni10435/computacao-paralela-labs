# Laboratório 8 de Computação Paralela


## Atividade de Sala

```
/*
 * Arquivo: omp_mm_1d.c
 * ---------------------
 * Objetivo: Implementar o mapeamento 1D (por linhas) para multiplicação de matrizes,
 * conectando com a aula teórica.
 *
 * Como compilar e executar:
 * $ gcc -o mm_1d -fopenmp omp_mm_1d.c -O2
 * $ export OMP_NUM_THREADS=4
 * $ ./mm_1d
 */

#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

#define N 1024 // Tamanho da matriz

// Matrizes globais para evitar estouro da pilha
double A[N][N], B[N][N], C[N][N];

int main() {
    // Inicializa as matrizes
    #pragma omp parallel for
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            A[i][j] = 1.0;
            B[i][j] = 2.0;
            C[i][j] = 0.0;
        }
    }

    double start_time = omp_get_wtime();

    // --- Região Paralela ---
    // A diretiva 'parallel for' é aplicada ao laço mais externo (i).
    // Isso implementa o Mapeamento 1D por Linhas: cada thread recebe um
    // conjunto contíguo de linhas da matriz C para calcular.
    #pragma omp parallel for

    // A tarefa de aula foi adicionar o schedule

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            // A variável 'sum' é declarada dentro do escopo do laço.
            // Isso a torna 'private' por padrão, evitando condições de corrida.
            double sum = 0.0;
            for (int k = 0; k < N; k++) {
                sum += A[i][k] * B[k][j];
            }
            C[i][j] = sum;
        }
    }

    double end_time = omp_get_wtime();
    printf("Tempo de execução: %f segundos\n", end_time - start_time);
    
    // Imprime o número de threads usadas para confirmação
    #pragma omp parallel
    {
        #pragma omp master
        printf("Executado com %d threads.\n", omp_get_num_threads());
    }
    
    return 0;
}
```

## Atividade 1

```
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

#define N 2048
#define M 2048

#define CHUNK_SIZE 4


double a[N][M], b[N][M], c[N][M];

int main() {
	printf("Inicializando as Matrizes...\n");
	// Inicializando as Matrizes
	#pragma omp parallel for
	for (int i = 0; i < N; i++){
		for (int j = 0; j < M; j++) {
			a[i][j] = 1.0;
			b[i][j] = 2.0;
		}
	}
	
	double start_time = omp_get_wtime();
	
	printf("Realizando a Multiplicação...\n");
	
	// Região Paralela
	#pragma omp parallel for schedule(guided)
	for (int i = 0; i < N; i++){
		for (int j = 0; j < M; j++) {
			// Inicializa a Matriz C
			c[i][j] = 0.0;
			for(int k = 0; k < N; k++) {
				// Calcula os valores da matriz C
                		c[i][j] += a[i][k] * b[k][j];
            		}
		}
	}
	
	printf("\n---- Multiplicacao concluida! ----\n");
	
	double end_time = omp_get_wtime();
	printf("Tempo de execução: %f segundos\n", end_time - start_time);
	
}
```
