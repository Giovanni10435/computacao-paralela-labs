# Segue o Laboratório 07 do curso de Computação Paralela do Mackenzie

## Atividade 1
```
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

#define N 20000000

double a[N], b[N], c[N];

int main() {
	
	printf("Inicializando os Vetores\n");
	#pragma omp parallel for
	for (int i = 0; i < N; i++){
		a[i] = 1.0;
		b[i] = 2.0;
	}
	
	printf("Inicializando a Soma Paralela...\n");
	#pragma omp parallel for
	for (int i = 0; i < N; i++){
		c[i] = a[i] + b[i];
	}
	
}
```
 ## Atividade 2
```
#include <stdio.h>
#include <stdlib.h>
#include <omp.h>

#define N 1024
#define M 1024


double a[N][M], b[N][M], c[N][M];

int main() {
	printf("Inicializando as Matrizes...\n");
	#pragma omp parallel for
	for (int i = 0; i < N; i++){
		for (int j = 0; j < M; j++) {
			a[i][j] = 1.0;
			b[i][j] = 2.0;
		}
	}
	
	printf("Realizando a Multiplicação...\n");
	#pragma omp parallel for
	for (int i = 0; i < N; i++){
		for (int j = 0; j < M; j++) {
			c[i][j] = 0.0;
			for(int k = 0; k < N; k++) {
                		c[i][j] += a[i][k] * b[k][j];
            		}
		}
	}
	
	printf("\n---- Multiplicacao concluida! ----\n");
	
}
```
