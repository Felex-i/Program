# Лабораторная работа № 3

## Тема
Структуры. Объединения. Перечисления.

**Студент:** Ерохина Анастасия Андреевна, ИВТ 1-1

---

## Комплект 1: Структуры

### Задача 1.1: Указатель на функцию в структуре

#### Постановка задачи
Создать структуру с указателем на функцию в качестве поля. Вызвать функцию через переменную структуры.

#### Список идентификаторов
| Имя | Тип | Описание |
|-----|-----|----------|
| func_ptr | void (*)(void) | Указатель на функцию |
| my_struct | StructWithFunc | Структура с полем-указателем |

#### Код программы
```c
#include <stdio.h>

void hello(void) {
    printf("Hello from function pointer!\n");
}

typedef struct {
    void (*func_ptr)(void);
} StructWithFunc;

int main(void) {
    StructWithFunc s = { hello };
    s.func_ptr();
    return 0;
}
Результаты выполненной работы
text
Hello from function pointer!
Задача 1.2: Вектор в 3D пространстве
Постановка задачи
Создать структуру для вектора в 3D пространстве с именем вектора. Реализовать скалярное и векторное произведение, модуль вектора, распечатку.

Математическая модель
Скалярное произведение: a·b = axbx + ayby + az*bz

Векторное произведение: c = a × b = (aybz - azby, azbx - axbz, axby - aybx)

Модуль вектора: |a| = √(ax² + ay² + az²)

Список идентификаторов
Имя	Тип	Описание
Vector	struct	Вектор с полями name, x, y, z
dot	функция	Скалярное произведение
cross	функция	Векторное произведение
magnitude	функция	Модуль вектора
print_vector	функция	Вывод вектора
Код программы
c
#include <stdio.h>
#include <math.h>

typedef struct {
    char name;
    double x, y, z;
} Vector;

double dot(Vector a, Vector b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

Vector cross(Vector a, Vector b) {
    Vector res;
    res.x = a.y * b.z - a.z * b.y;
    res.y = a.z * b.x - a.x * b.z;
    res.z = a.x * b.y - a.y * b.x;
    res.name = 'c';
    return res;
}

double magnitude(Vector a) {
    return sqrt(a.x * a.x + a.y * a.y + a.z * a.z);
}

void print_vector(Vector v) {
    printf("%c = (%.2f, %.2f, %.2f)\n", v.name, v.x, v.y, v.z);
}

int main(void) {
    Vector a = {'a', 1, 2, 3};
    Vector b = {'b', 4, 5, 6};
    
    print_vector(a);
    print_vector(b);
    
    printf("Dot product: %.2f\n", dot(a, b));
    
    Vector c = cross(a, b);
    print_vector(c);
    
    printf("Magnitude of a: %.2f\n", magnitude(a));
    printf("Magnitude of b: %.2f\n", magnitude(b));
    
    return 0;
}
Результаты выполненной работы
text
a = (1.00, 2.00, 3.00)
b = (4.00, 5.00, 6.00)
Dot product: 32.00
c = (-3.00, 6.00, -3.00)
Magnitude of a: 3.74
Magnitude of b: 8.77
Задача 1.3: Комплексная экспонента
Постановка задачи
Вычислить exp(z) для комплексного числа z, используя структуру комплексного числа и ряд Тейлора.

Математическая модель
exp(z) = Σ (z^n / n!) для n = 0..N

Список идентификаторов
Имя	Тип	Описание
Complex	struct	Поля re, im
cexp	функция	Вычисление экспоненты
factorial	функция	Вычисление факториала
Код программы
c
#include <stdio.h>

typedef struct {
    double re, im;
} Complex;

double factorial(int n) {
    double res = 1;
    for (int i = 2; i <= n; i++) res *= i;
    return res;
}

Complex cexp(Complex z, int N) {
    Complex sum = {1, 0};
    Complex term = {1, 0};
    
    for (int n = 1; n <= N; n++) {
        Complex new_term;
        new_term.re = term.re * z.re - term.im * z.im;
        new_term.im = term.re * z.im + term.im * z.re;
        term = new_term;
        
        double fact = factorial(n);
        sum.re += term.re / fact;
        sum.im += term.im / fact;
    }
    return sum;
}

int main(void) {
    Complex z = {1.0, 1.0};
    Complex result = cexp(z, 20);
    
    printf("exp(%.2f + %.2fi) = %.6f + %.6fi\n", z.re, z.im, result.re, result.im);
    return 0;
}
Результаты выполненной работы
text
exp(1.00 + 1.00i) = 1.468694 + 2.287355i
Задача 1.4: Битовые поля для даты
Постановка задачи
Создать экономичную структуру для хранения даты (день, месяц, год) с использованием битовых полей.

Список идентификаторов
Имя	Тип	Описание
Date	struct	Битовые поля: day(5), month(4), year(7)
Код программы
c
#include <stdio.h>

typedef struct {
    unsigned int day : 5;
    unsigned int month : 4;
    unsigned int year : 7;
} Date;

int main(void) {
    Date birthday = {15, 6, 30};
    
    printf("Size of Date: %lu bytes\n", sizeof(Date));
    printf("Birthday: %02d.%02d.%04d\n", birthday.day, birthday.month, 2000 + birthday.year);
    
    return 0;
}
Результаты выполненной работы
text
Size of Date: 4 bytes
Birthday: 15.06.2030
Задача 1.5: Двунаправленный связный список
Постановка задачи
Реализовать двунаправленный связный список, выполнить обход в прямом и обратном направлениях.

Список идентификаторов
Имя	Тип	Описание
Node	struct	Поля data, prev, next
create_node	функция	Создание узла
print_forward	функция	Прямой обход
print_backward	функция	Обратный обход
Код программы
c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *prev;
    struct Node *next;
} Node;

Node* create_node(int data) {
    Node *new_node = (Node*)malloc(sizeof(Node));
    new_node->data = data;
    new_node->prev = NULL;
    new_node->next = NULL;
    return new_node;
}

void print_forward(Node *head) {
    Node *current = head;
    while (current) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}

void print_backward(Node *tail) {
    Node *current = tail;
    while (current) {
        printf("%d ", current->data);
        current = current->prev;
    }
    printf("\n");
}

int main(void) {
    Node *head = create_node(10);
    Node *second = create_node(20);
    Node *third = create_node(30);
    Node *tail = third;
    
    head->next = second;
    second->prev = head;
    second->next = third;
    third->prev = second;
    
    printf("Forward: ");
    print_forward(head);
    
    printf("Backward: ");
    print_backward(tail);
    
    free(head); free(second); free(third);
    return 0;
}
Результаты выполненной работы
text
Forward: 10 20 30 
Backward: 30 20 10 
Комплект 2: Объединения и перечисления
Задача 2.1: Указатель на объединение
Постановка задачи
Написать программу, использующую указатель на объединение.

Код программы
c
#include <stdio.h>

typedef union {
    int i;
    float f;
} Data;

int main(void) {
    Data d;
    Data *ptr = &d;
    
    ptr->i = 42;
    printf("int: %d\n", ptr->i);
    
    ptr->f = 3.14;
    printf("float: %.2f\n", ptr->f);
    
    return 0;
}
Результаты выполненной работы
text
int: 42
float: 3.14
Задача 2.2: Побайтовая печать unsigned long
Постановка задачи
Использовать union для побайтовой печати типа unsigned long.

Код программы
c
#include <stdio.h>

typedef union {
    unsigned long value;
    unsigned char bytes[sizeof(unsigned long)];
} LongBytes;

int main(void) {
    LongBytes lb;
    lb.value = 0x12345678;
    
    printf("unsigned long: %lu\n", lb.value);
    printf("Bytes: ");
    for (int i = 0; i < sizeof(unsigned long); i++) {
        printf("%02X ", lb.bytes[i]);
    }
    printf("\n");
    
    return 0;
}
Результаты выполненной работы
text
unsigned long: 305419896
Bytes: 78 56 34 12 
Задача 2.3: Перечисление дней недели
Постановка задачи
Создать enum для семи дней недели и распечатать значения как целые числа.

Код программы
c
#include <stdio.h>

typedef enum {
    MONDAY = 1,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
} Weekday;

int main(void) {
    for (int i = MONDAY; i <= SUNDAY; i++) {
        printf("Day %d\n", i);
    }
    return 0;
}
Результаты выполненной работы
text
Day 1
Day 2
Day 3
Day 4
Day 5
Day 6
Day 7
Задача 2.4: Размеченное объединение
Постановка задачи
Создать размеченное объединение (структура с union и enum для указания типа данных). Создать динамический массив таких структур и распечатать.

Список идентификаторов
Имя	Тип	Описание
DataType	enum	INT, FLOAT, STRING
TaggedUnion	struct	Поля type и data (union)
Код программы
c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef enum {
    INT_TYPE,
    FLOAT_TYPE,
    STRING_TYPE
} DataType;

typedef union {
    int i;
    float f;
    char *s;
} Data;

typedef struct {
    DataType type;
    Data data;
} TaggedUnion;

void print_tagged(TaggedUnion *arr, int size) {
    for (int i = 0; i < size; i++) {
        switch (arr[i].type) {
            case INT_TYPE:
                printf("[%d] int: %d\n", i, arr[i].data.i);
                break;
            case FLOAT_TYPE:
                printf("[%d] float: %.2f\n", i, arr[i].data.f);
                break;
            case STRING_TYPE:
                printf("[%d] string: %s\n", i, arr[i].data.s);
                break;
        }
    }
}

int main(void) {
    int n = 4;
    TaggedUnion *arr = (TaggedUnion*)malloc(n * sizeof(TaggedUnion));
    
    arr[0].type = INT_TYPE;
    arr[0].data.i = 100;
    
    arr[1].type = FLOAT_TYPE;
    arr[1].data.f = 3.14159;
    
    arr[2].type = STRING_TYPE;
    arr[2].data.s = "Hello, world!";
    
    arr[3].type = INT_TYPE;
    arr[3].data.i = 42;
    
    print_tagged(arr, n);
    
    free(arr);
    return 0;
}
Результаты выполненной работы
text
[0] int: 100
[1] float: 3.14
[2] string: Hello, world!
[3] int: 42
Информация о студенте
Ерохина Анастасия Андреевна, ИВТ 1-1

text

