# Домашнее задание к работе 16
## Условие задачи
Вариант 22. Массив d0, d1, d2, ..., dh из элементов массивов а0, а1, а2, ..., an, b0, b1, b2 ...,
bn, с0, с1,с2, ..., сn расположенные до последнего отрицательного элемента, в
обратном порядке.

## 1. Алгоритм и блок-схема
## Алгоритм
```
1. Начало.
2. Установить локаль и инициализировать генератор случайных чисел:
   srand(time(NULL));
3. Сгенерировать размеры трёх массивов случайно от 10 до 50:
   size_a = rand() % 41 + 10;
   size_b = rand() % 41 + 10;
   size_c = rand() % 41 + 10;
4. Выделить динамическую память для массивов a, b, c:
   a = malloc(size_a * sizeof(double));
   b = malloc(size_b * sizeof(double));
   c = malloc(size_c * sizeof(double));
5. Заполнить массивы случайными числами:
   full_elements(a, size_a);
   full_elements(b, size_b);
   full_elements(c, size_c);
6. Вывести массивы a, b, c на экран с указанием размеров:
   put_elements(a, size_a);
   put_elements(b, size_b);
   put_elements(c, size_c);
7. Сформировать массив D по заданному варианту:
   d = build_d(a, size_a, b, size_b, c, size_c, &size_d);
8. Проверить успешность выделения памяти для D.
9. Вывести массив D на экран:
   put_elements(d, size_d);
10. Освободить память для всех массивов:
    free(a); free(b); free(c); free(d);
11. Конец.
```
   
### Блок-схема

<img width="583" height="789" alt="{4896F898-A941-4943-AB9F-DB0469045DD3}" src="https://github.com/user-attachments/assets/483ac5a2-4dfd-4eb0-8046-b8674587b57f" />



## 2. Реализация программы

```
#define _CRT_SECURE_NO_WARNINGS
#define _USE_MATH_DEFINES
#include <locale.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h> 
#include <conio.h>
#include <math.h>
#define CLASSES 4      
#define PARALLELS 11   
double* full_elements(double* ptr_array, int n) {
    for (int i = 0; i < n; i++) {
        double r = (double)rand() / RAND_MAX;
        ptr_array[i] = r * 2.0 - 1.0;
    }
    return ptr_array;
}
int put_elements(double* ptr_array, int size) {
    for (int i = 0; i < size; i++) {
        printf("%.6lf ", ptr_array[i]);
    }
    printf("\n");
    return 0;
}
double* build_d(double* a, int size_a, double* b, int size_b, double* c, int size_c, int* size_d) {
    int last_neg_index_a = -1, last_neg_index_b = -1, last_neg_index_c = -1;
    for (int i = 0; i < size_a; i++) if (a[i] < 0) last_neg_index_a = i;
    for (int i = 0; i < size_b; i++) if (b[i] < 0) last_neg_index_b = i;
    for (int i = 0; i < size_c; i++) if (c[i] < 0) last_neg_index_c = i;
    if (last_neg_index_a == -1) last_neg_index_a = size_a - 1;
    if (last_neg_index_b == -1) last_neg_index_b = size_b - 1;
    if (last_neg_index_c == -1) last_neg_index_c = size_c - 1;

    *size_d = (last_neg_index_a + 1) + (last_neg_index_b + 1) + (last_neg_index_c + 1);
    double* d = (double*)malloc((*size_d) * sizeof(double));
    if (!d) return NULL;

    int idx = 0;
    for (int i = last_neg_index_a; i >= 0; i--) d[idx++] = a[i];
    for (int i = last_neg_index_b; i >= 0; i--) d[idx++] = b[i];
    for (int i = last_neg_index_c; i >= 0; i--) d[idx++] = c[i];
    return d;
}
int main() {
    setlocale(LC_ALL, "RUS");
    srand(time(NULL));
    int size_a = rand() % 41 + 10;
    int size_b = rand() % 41 + 10;
    int size_c = rand() % 41 + 10;
    double* a = (double*)malloc(size_a * sizeof(double));
    double* b = (double*)malloc(size_b * sizeof(double));
    double* c = (double*)malloc(size_c * sizeof(double));
    if (!a || !b || !c) {
        puts("Ошибка выделения памяти");
        return -1;
    }
    full_elements(a, size_a);
    full_elements(b, size_b);
    full_elements(c, size_c);
    printf("Массив A (size=%d):\n", size_a);
    put_elements(a, size_a);
    printf("\nМассив B (size=%d):\n", size_b);
    put_elements(b, size_b);
    printf("\nМассив C (size=%d):\n", size_c);
    put_elements(c, size_c);
    int size_d;
    double* d = build_d(a, size_a, b, size_b, c, size_c, &size_d);
    if (!d) {
        free(a); free(b); free(c);
        puts("Ошибка выделения памяти для массива D");
        return -1;
    }
    printf("\nМассив D (до последнего отрицательного элемента всех массивов, в обратном порядке):\n");
    put_elements(d, size_d);

    free(a);
    free(b);
    free(c);
    free(d);

    return 0;
}
```


## 3. Результаты работы программы

```
Массив A (size=15):
-0,527696 -0,541307 0,609363 0,979431 -0,050996 -0,836909 -0,934629 -0,014863 0,563890 0,160619 0,331217 -0,942930 -0,193762 -0,140294 0,553575

Массив B (size=16):
-0,361248 0,694327 0,159459 0,172521 0,996521 0,668386 0,021332 -0,542772 -0,136753 -0,090732 0,763298 -0,127110 0,586535 0,145177 -0,111057 -0,157079

Массив C (size=28):
0,092196 -0,990417 0,264870 0,468062 0,977783 0,011811 0,457564 -0,235389 -0,953551 -0,815912 -0,956847 0,985290 0,596179 0,466475 0,702872 -0,415937 -0,226539 0,892392 0,914914 0,388531 -0,692312 -0,361919 -0,942137 -0,458663 0,936033 -0,002533 0,736015 0,773980

Массив D (до последнего отрицательного элемента всех массивов, в обратном порядке):
-0,140294 -0,193762 -0,942930 0,331217 0,160619 0,563890 -0,014863 -0,934629 -0,836909 -0,050996 0,979431 0,609363 -0,541307 -0,527696 -0,157079 -0,111057 0,145177 0,586535 -0,127110 0,763298 -0,090732 -0,136753 -0,542772 0,021332 0,668386 0,996521 0,172521 0,159459 0,694327 -0,361248 -0,002533 0,936033 -0,458663 -0,942137 -0,361919 -0,692312 0,388531 0,914914 0,892392 -0,226539 -0,415937 0,702872 0,466475 0,596179 0,985290 -0,956847 -0,815912 -0,953551 -0,235389 0,457564 0,011811 0,977783 0,468062 0,264870 -0,990417 0,092196
```


<img width="1183" height="492" alt="{CFB6EEC9-6C4C-408E-B4F2-70A6918BDD8A}" src="https://github.com/user-attachments/assets/a2b5d407-9265-42cf-8d62-55bfdc4e30af" />
