#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#define MAX 10
 

int A[MAX][MAX], B[MAX][MAX], C[MAX][MAX];
int N;     // Matrix size

typedef struct {
    int row;
    int col;
} ThreadData;

void* multiply(void* arg) {
    ThreadData* data = (ThreadData*)arg;
    int sum = 0;
    for (int k = 0; k < N; k++) {
        sum += A[data->row][k] * B[k][data->col];
    }
    C[data->row][data->col] = sum;
    pthread_exit(NULL);
}

int main() {
    printf("Enter the size of matrices: ");
    scanf("%d", &N);

    
    printf("Matrix A:\n");
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            A[i][j] = rand() % 10;  
            printf("%d ", A[i][j]);
        }
        printf("\n");
    }

    printf("Matrix B:\n");
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            B[i][j] = rand() % 10;  
            printf("%d ", B[i][j]);
        }
        printf("\n");
    }

    pthread_t threads[N][N];
    ThreadData data[N][N];

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            data[i][j].row = i;
            data[i][j].col = j;
            pthread_create(&threads[i][j], NULL, multiply, (void*)&data[i][j]);
        }
    }

    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            pthread_join(threads[i][j], NULL);
        }
    }

    printf("Result matrix C:\n");
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }

    return 0;
}

