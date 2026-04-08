---

# Лабораторная работа № 1
## Тема
Математические операции. Переменные и их типы. Операторы. Циклы. Простые условные конструкции.

**Студент:** Ерохина Анастасия Андреевна, ИВТ 1-1

---

## Задача 1.2
### Постановка задачи
Написать простую программу. Ввести два числа с клавиатуры, вычислить их сумму и напечатавить результат. Использовать функцию printf для приглашений на ввод и для распечатки результата. Использовать функцию scanf для ввода каждого числа отдельно с клавиатуры. Для получения доступа к функциям printf и scanf включить в программу заголовочный файл stdio.h. Использовать корректные спецификаторы форматирования. Для распечатки надписей на экране использовать латинские буквы для избежания проблем с кодировками символов.

### Математическая модель
`sum = a + b`

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| a | float | Первое число |
| b | float | Второе число |
| sum | float | Результат сложения |

### Код программы
```c
#include <stdio.h>

int main() {
    float a, b, sum;

    printf("Vvedite pervoe chislo: ");
    scanf("%f", &a);

    printf("Vvedite vtoroe chislo: ");
    scanf("%f", &b);

    sum = a + b;

    printf("Summa: %f\n", sum);

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite pervoe chislo: 12
Vvedite vtoroe chislo: 32
Summa: 44.000000
```
![Результат задачи 1.2](img/Снимок%20экрана%202026-04-05%20193832.png)
---

## Задача 1.3
### Постановка задачи
Вычислить значение функции u(x,y) = (1 + sin²(x+y)) / (2 + |x - 2x²/(1 + |sin(x+y)|)|), введя x и y с клавиатуры. Включить в программу заголовочный файл math.h для доступа к математическим функциям.

### Математическая модель
\[u(x, y) = \frac{1 + \sin^2(x+y)}{2 + \left|x - \frac{2x^2}{1 + |\sin(x+y)|}\right|}\]

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| x | double | Первый аргумент |
| y | double | Второй аргумент |
| sin_xy | double | Значение sin(x+y) |
| numerator | double | Числитель |
| denominator | double | Знаменатель |

### Код программы
```c
#include <stdio.h>
#include <math.h>

int main() {
    double x, y, sin_xy, numerator, denominator, result;

    printf("Vvedite x: ");
    scanf("%lf", &x);

    printf("Vvedite y: ");
    scanf("%lf", &y);

    sin_xy = sin(x + y);
    numerator = 1 + pow(sin_xy, 2);
    denominator = 2 + fabs(x - (2 * pow(x, 2)) / (1 + fabs(sin_xy)));
    result = numerator / denominator;

    printf("Rezultat: u(x,y) = %lf\n", result);

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite x: 10
Vvedite y: 12
Rezultat: u(x,y) = 0.005257
```
![](img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202026-04-05%20193843.png)
---

## Задача 1.4
### Постановка задачи
Вычислить значение функции h(x) = -(x-a)/∛(x²+a²) - 4∜(x²+b²)³/(2+a+b+∛(x-c)²) для заданных параметров.

### Математическая модель
\[h(x) = -\frac{x-a}{\sqrt[3]{x^2+a^2}} - \frac{4\sqrt[4]{(x^2+b^2)^3}}{2+a+b+\sqrt[3]{(x-c)^2}}\]

Значения параметров:
- a = 0.12, b = 3.5, c = 2.4, x = 1.4
- a = 0.12, b = 3.5, c = 2.4, x = 1.6
- a = 0.27, b = 3.9, c = 2.8, x = 1.8

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| x | double | Основной аргумент |
| a, b, c | double | Параметры |
| part1 | double | Первая часть |
| part2 | double | Вторая часть |

### Код программы
```c
#include <stdio.h>
#include <math.h>

int main() {
    double x, a, b, c, part1, part2, result;

    printf("Vvedite x: ");
    scanf("%lf", &x);
    printf("Vvedite a: ");
    scanf("%lf", &a);
    printf("Vvedite b: ");
    scanf("%lf", &b);
    printf("Vvedite c: ");
    scanf("%lf", &c);

    part1 = -(x - a) / pow(x*x + a*a, 1.0/3.0);
    part2 = 4 * pow(pow(x*x + b*b, 3), 1.0/4.0) /
            (2 + a + b + pow((x-c)*(x-c), 1.0/3.0));
    result = part1 - part2;

    printf("Rezultat: h(x) = %lf\n", result);

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite x: 12
Vvedite a: 10
Vvedite b: 5
Vvedite c: 9
Rezultat: h(x) = -10.146468
```
![](img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202026-04-05%20193849.png)
---

## Задача 2.1
### Постановка задачи
Вычислить используя цикл координаты планеты Марс относительно Земли с течением времени t.

### Математическая модель
- `x = r1·cos(w1·t) - r2·cos(w2·t)`
- `y = r1·sin(w1·t) - r2·sin(w2·t)`
- `w1 = 2π/T1`, `w2 = 2π/T2`

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| R | double | Радиус орбиты |
| omega | double | Угловая скорость |
| t | double | Время |
| x, y | double | Координаты |

### Код программы
```c
#include <stdio.h>
#include <math.h>

int main() {
    double R = 227.9e6;
    double omega = 0.00524;
    double t;

    printf("Vvedite vremya (dni): ");
    scanf("%lf", &t);

    double x = R * cos(omega * t);
    double y = R * sin(omega * t);

    printf("Koordinaty Marsa:\n");
    printf("X = %.2f km\n", x);
    printf("Y = %.2f km\n", y);

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite vremya (dni): 364
Koordinaty Marsa:
X = -75262953.20 km
Y = 215113685.93 km
```
![](img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202026-04-05%20193857.png)
---

## Задача 2.2
### Постановка задачи
Вычислить определённый интеграл от заданной функции методом трапеций.

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| x | double | Координата X точки |
| y | double | Координата Y точки |
| result | bool | Результат проверки |

### Код программы
```c
#include <stdio.h>

int main() {
    double x, y;

    printf("Vvedite koordinatu X: ");
    scanf("%lf", &x);
    printf("Vvedite koordinatu Y: ");
    scanf("%lf", &y);

    if ((x <= 0 && y <= 0 && y >= -x - 2) || (x*x + y*y <= 1)) {
        printf("Tochka popadaet v oblast\n");
    } else {
        printf("Tochka ne popadaet v oblast\n");
    }

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite koordinatu X: 1
Vvedite koordinatu Y: 1
Tochka ne popadaet v oblast
```
![](img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202026-04-05%20193903.png)
---

## Задача 2.3
### Постановка задачи
Организовать и распечатать последовательность чисел Падована, не превосходящих число m, введенное с клавиатуры.

### Математическая модель
- P(0) = P(1) = P(2) = 1
- P(n) = P(n-2) + P(n-3)

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| m | int | Максимальное значение |
| p0, p1, p2 | int | Предыдущие значения последовательности |
| current | int | Текущее значение |
| n | int | Счетчик |

### Код программы
```c
#include <stdio.h>

int main() {
    int m;

    printf("Vvedite maksimalnoe chislo m: ");
    scanf("%d", &m);

    int p0 = 1, p1 = 1, p2 = 1;

    printf("Posledovatelnost chisel Padovana:\n");
    if (p0 <= m) printf("%d ", p0);
    if (p1 <= m) printf("%d ", p1);
    if (p2 <= m) printf("%d ", p2);

    for (int n = 3; ; n++) {
        int current = p1 + p0;
        if (current > m) break;
        printf("%d ", current);
        p0 = p1;
        p1 = p2;
        p2 = current;
    }
    printf("\n");

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite maksimalnoe chislo m: 100
Posledovatelnost chisel Padovana:
1 1 1 2 2 3 4 5 7 9 12 16 21 28 37 49 65 86
```
![](img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202026-04-05%20193936.png)
---

## Задача 2.4
### Постановка задачи
С клавиатуры вводится трёхзначное число, считается сумма его цифр. Если сумма цифр числа больше 10, то вводится следующее трёхзначное число, если сумма меньше либо равна 10 — программа завершается.

### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| number | int | Вводимое число |
| sum | int | Сумма цифр |
| a, b, c | int | Цифры числа |

### Код программы
```c
#include <stdio.h>

int main() {
    int number;
    int sum;

    do {
        printf("Vvedite trehznachnoe chislo: ");
        scanf("%d", &number);

        if (number < 100 || number > 999) {
            printf("Oshibka! Chislo dolzhno byt trehznachnym.\n");
            continue;
        }

        int a = number / 100;
        int b = (number / 10) % 10;
        int c = number % 10;
        sum = a + b + c;

        printf("Summa tsifr: %d\n", sum);

        if (sum > 10) {
            printf("Summa bolshe 10. Vvedite sleduyuschee chislo.\n");
        }
    } while (sum > 10);

    printf("\nRezultat: summa tsifr ne prevyshaet 10\n");

    return 0;
}
```

### Результаты выполненной работы
```text
Vvedite trehznachnoe chislo: 120
Summa tsifr: 3

Rezultat: summa tsifr ne prevyshaet 10
```
![](img/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202026-04-05%20193943.png)

## Комплект 3 - Работа с массивами
---

### Задача 3.1

#### Постановка задачи
Для некоторого числового вектора X, введённого с клавиатуры, вычислить значения вектора Y = X · X (yi = xi · xi — поэлементно).

#### Математическая модель
\[Y[i] = X[i] \cdot X[i]\]

#### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| n | int | Размер вектора |
| X[] | int[] | Исходный вектор |
| Y[] | int[] | Результирующий вектор |
| i | int | Счетчик цикла |

#### Код программы
```c
#include <stdio.h>

int main() {
    int n;

    printf("Vvedite razmer vektora: ");
    scanf("%d", &n);

    int X[n], Y[n];

    printf("Vvedite elementy vektora X:\n");
    for(int i = 0; i < n; i++) {
        printf("X[%d] = ", i);
        scanf("%d", &X[i]);
    }

    for(int i = 0; i < n; i++) {
        Y[i] = X[i] * X[i];
    }

    printf("Vektor Y:\n");
    for(int i = 0; i < n; i++) {
        printf("Y[%d] = %d\n", i, Y[i]);
    }

    return 0;
}
```

#### Результаты выполненной работы

![Результат 3.1](img/task3-1.png)

**Вывод программы:**
```
Vvedite razmer vektora: 2
Vvedite elementy vektora X:
X[0] = 1
X[1] = 2
Vektor Y:
Y[0] = 1
Y[1] = 4
```

---

### Задача 3.2

#### Постановка задачи
Для некоторого числового массива X, введённого с клавиатуры поэлементно, изменить порядок элементов на обратный и распечатать результат на экране.

#### Математическая модель
\[X[i] \leftrightarrow X[n-i-1]\]

#### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| n | int | Размер массива |
| X[] | int[] | Исходный массив |
| temp | int | Временная переменная |
| i | int | Счетчик цикла |

#### Код программы
```c
#include <stdio.h>

int main() {
    int n;

    printf("Vvedite razmer massiva: ");
    scanf("%d", &n);

    int X[n];

    printf("Vvedite elementy massiva:\n");
    for(int i = 0; i < n; i++) {
        printf("X[%d] = ", i);
        scanf("%d", &X[i]);
    }

    for(int i = 0; i < n/2; i++) {
        int temp = X[i];
        X[i] = X[n-i-1];
        X[n-i-1] = temp;
    }

    printf("Obraschennyy massiv:\n");
    for(int i = 0; i < n; i++) {
        printf("X[%d] = %d\n", i, X[i]);
    }

    return 0;
}
```

#### Результаты выполненной работы

![Результат 3.2](img/task3-2.png)

**Вывод программы:**
```
Vvedite razmer massiva: 5
Vvedite elementy massiva:
X[0] = 1
X[1] = 2
X[2] = 3
X[3] = 4
X[4] = 5
Obraschennyy massiv:
X[0] = 5
X[1] = 4
X[2] = 3
X[3] = 2
X[4] = 1
```

---

### Задача 3.3

#### Постановка задачи
Транспонировать матрицу А

\[A = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix}\]

#### Математическая модель
\[A_{ij}^T = A_{ji}\]

#### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| A[][] | int[3][3] | Исходная матрица |
| AT[][] | int[3][3] | Транспонированная матрица |
| i, j | int | Индексы |

#### Код программы
```c
#include <stdio.h>

int main() {
    int A[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    int AT[3][3];

    printf("Ishodnaya matritsa:\n");
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            printf("%d ", A[i][j]);
        }
        printf("\n");
    }

    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            AT[j][i] = A[i][j];
        }
    }

    printf("\nTransportirovannaya matritsa:\n");
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            printf("%d ", AT[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

#### Результаты выполненной работы

![Результат 3.3](img/task3-3.png)

**Вывод программы:**
```
Ishodnaya matritsa:
1 2 3
4 5 6
7 8 9

Transportirovannaya matritsa:
1 4 7
2 5 8
3 6 9
```

---

### Задача 3.4

#### Постановка задачи
Преобразовать исходную матрицу так, чтобы первый элемент каждой строки был заменён средним арифметическим элементов этой строки.

#### Математическая модель
\[A_{i0} = \frac{1}{n} \sum_{j=0}^{n-1} A_{ij}\]

#### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| A[][] | int[3][3] | Исходная матрица |
| sum | int | Сумма элементов строки |
| avg | float | Среднее арифметическое |
| i, j | int | Индексы |

#### Код программы
```c
#include <stdio.h>

int main() {
    int A[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    int n = 3;

    printf("Ishodnaya matritsa:\n");
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            printf("%d ", A[i][j]);
        }
        printf("\n");
    }

    for(int i = 0; i < n; i++) {
        int sum = 0;
        for(int j = 0; j < n; j++) {
            sum += A[i][j];
        }
        float avg = (float)sum / n;
        A[i][0] = avg;
    }

    printf("\nMatritsa posle zameny:\n");
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            printf("%.0f ", (float)A[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

#### Результаты выполненной работы

![Результат 3.4](img/task3-4.png)

**Вывод программы:**
```
Ishodnaya matritsa:
1 2 3
4 5 6
7 8 9

Matritsa posle zameny:
2 2 3
5 5 6
8 8 9
```

---

### Задача 3.5

#### Постановка задачи
Реализовать самостоятельно алгоритм сортировки вставками (без создания своих функций, внутри функции main).

#### Математическая модель
Алгоритм последовательно вставляет каждый элемент на его правильное место в отсортированной части массива.

#### Список идентификаторов

| Имя | Тип | Описание |
|---|---|---|
| arr[] | int[N] | Массив для сортировки |
| n | int | Размер массива |
| i | int | Внешний счетчик |
| j | int | Внутренний счетчик |
| key | int | Временное хранение значения |

#### Код программы
```c
#include <stdio.h>

int main() {
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Ishodnyy massiv: ");
    for(int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    for(int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        while(j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }

    printf("Otsortirovannyy massiv: ");
    for(int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return 0;
}
```

#### Результаты выполненной работы

![Результат 3.5](img/task3-5.png)

**Вывод программы:**
```
Ishodnyy massiv: 12 11 13 5 6
Otsortirovannyy massiv: 5 6 11 12 13
```
```

## Информация о студенте
**Ерохина Анастасия Андреевна**, ИВТ 1-1

---
