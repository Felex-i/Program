# Лабораторная работа № 5

## Тема
Сборка C-проектов с GNU Make и Meson

**Студент:** Ерохина Анастасия Андреевна, ИВТ 1-1

---

## Теоретическая справка

### Системы сборки

**GNU Make** – система автоматизации сборки, использующая файл `Makefile` с правилами, целями и зависимостями. Команды рецепта должны начинаться с символа табуляции.

**Meson** – современная система сборки с декларативным DSL. Работает в два этапа: настройка (`meson setup build`) и компиляция (`meson compile -C build`). Использует отдельный каталог сборки.

### Структура проектов

В данной работе используются проекты с плоской структурой: все файлы (.c, .h, Makefile/meson.build) находятся в одном каталоге.

---

## Подготовка рабочей среды

### MSYS2 (UCRT64)
```bash
pacman -Syu
pacman -S --needed make mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-meson mingw-w64-ucrt-x86_64-ninja
WSL2 (Ubuntu 24.04+) / Linux
bash
sudo apt update
sudo apt install -y build-essential meson ninja-build
Проверка установки
bash
gcc --version
make --version
meson --version
ninja --version
Структура лабораторной работы
text
05-lab-build-make-meson/
├── task01_make_students/
├── task02_make_html_title/
├── task03_meson_matrix_stats/
└── task04_meson_log_stats/
Комплект 1: Проекты с GNU Make
Задача 1.1: Лучший студент по среднему баллу (task01_make_students)
Постановка задачи
Программа читает текстовый файл со списком студентов и оценок, вычисляет средний балл каждого студента и выводит имя студента с максимальным средним баллом.

Структура проекта
text
task01_make_students/
├── main.c
├── students.c
├── students.h
├── students.txt
└── Makefile
Список идентификаторов
Имя	Тип	Описание
Student	struct	Структура студента (name, score1, score2, score3, avg)
load_students	int ()(const char, Student[], int)	Загрузка данных из файла
find_best_student	int (*)(const Student[], int)	Поиск студента с max avg
Код программы
students.h

c
#ifndef STUDENTS_H
#define STUDENTS_H

typedef struct {
    char name[64];
    int score1;
    int score2;
    int score3;
    double avg;
} Student;

int load_students(const char *filename, Student arr[], int max_count);
int find_best_student(const Student arr[], int count);

#endif
students.c

c
#include "students.h"
#include <stdio.h>

int load_students(const char *filename, Student arr[], int max_count) {
    FILE *f = fopen(filename, "r");
    int count = 0;
    
    if (!f) {
        return -1;
    }
    
    while (count < max_count) {
        Student s;
        int n = fscanf(f, "%63[^,],%d,%d,%d", s.name, &s.score1, &s.score2, &s.score3);
        if (n != 4) {
            break;
        }
        s.avg = (s.score1 + s.score2 + s.score3) / 3.0;
        arr[count] = s;
        count++;
    }
    fclose(f);
    return count;
}

int find_best_student(const Student arr[], int count) {
    int best = 0;
    for (int i = 1; i < count; i++) {
        if (arr[i].avg > arr[best].avg) {
            best = i;
        }
    }
    return best;
}
main.c

c
#include "students.h"
#include <stdio.h>

int main(int argc, char *argv[]) {
    Student students[100];
    const char *path = "students.txt";
    int count;
    int best;
    
    if (argc > 1) {
        path = argv[1];
    }
    
    count = load_students(path, students, 100);
    if (count <= 0) {
        printf("Cannot read students file: %s\n", path);
        return 1;
    }
    
    best = find_best_student(students, count);
    printf("Count: %d\n", count);
    printf("Best: %s (avg=%.2f)\n", students[best].name, students[best].avg);
    
    return 0;
}
students.txt

text
Ivan,5,4,5
Olga,5,5,5
Petr,3,4,4
Nina,4,4,5
Makefile

makefile
CC = gcc
CFLAGS = -std=c11 -Wall -Wextra -pedantic
TARGET = task01_students
SRC = main.c students.c

all: $(TARGET)

$(TARGET): $(SRC)
	$(CC) $(CFLAGS) $(SRC) -o $(TARGET)

run: $(TARGET)
	./$(TARGET) students.txt

clean:
	rm -f $(TARGET)
Команды сборки и запуска
bash
make
make run
make clean
Ожидаемый результат
text
Count: 4
Best: Olga (avg=5.00)
https://img/25.png

Задача 1.2: Извлечение <title> из HTML (task02_make_html_title)
Постановка задачи
Программа читает локальный HTML-файл и извлекает текст между тегами <title> и </title>. Добавлена отдельная цель debug для сборки с флагом -g.

Структура проекта
text
task02_make_html_title/
├── main.c
├── html_title.c
├── html_title.h
├── page.html
└── Makefile
Список идентификаторов
Имя	Тип	Описание
load_text_file	int ()(const char, char*, size_t)	Загрузка файла в буфер
extract_title	int ()(const char, char*, size_t)	Извлечение заголовка
Код программы
html_title.h

c
#ifndef HTML_TITLE_H
#define HTML_TITLE_H

#include <stddef.h>

int load_text_file(const char *filename, char *buffer, size_t buffer_size);
int extract_title(const char *html, char *title, size_t title_size);

#endif
html_title.c

c
#include "html_title.h"
#include <stdio.h>
#include <string.h>

int load_text_file(const char *filename, char *buffer, size_t buffer_size) {
    FILE *f;
    size_t n;
    
    f = fopen(filename, "r");
    if (!f) {
        return -1;
    }
    
    n = fread(buffer, 1, buffer_size - 1, f);
    buffer[n] = '\0';
    fclose(f);
    return 0;
}

int extract_title(const char *html, char *title, size_t title_size) {
    const char *open_tag = "<title>";
    const char *close_tag = "</title>";
    const char *start;
    const char *end;
    size_t len;
    
    start = strstr(html, open_tag);
    if (!start) {
        return -1;
    }
    start += strlen(open_tag);
    
    end = strstr(start, close_tag);
    if (!end) {
        return -1;
    }
    
    len = (size_t)(end - start);
    if (len >= title_size) {
        len = title_size - 1;
    }
    
    memcpy(title, start, len);
    title[len] = '\0';
    return 0;
}
main.c

c
#include "html_title.h"
#include <stdio.h>

int main(int argc, char *argv[]) {
    char html[8192];
    char title[256];
    const char *path = "page.html";
    
    if (argc > 1) {
        path = argv[1];
    }
    
    if (load_text_file(path, html, sizeof(html)) != 0) {
        printf("Cannot read html file: %s\n", path);
        return 1;
    }
    
    if (extract_title(html, title, sizeof(title)) != 0) {
        printf("Tag <title> not found\n");
        return 1;
    }
    
    printf("Title: %s\n", title);
    return 0;
}
page.html

html
<!doctype html>
<html>
<head>
    <title>Simple Demo Page</title>
</head>
<body>
    <h1>Hello</h1>
    <p>Training file for task02.</p>
</body>
</html>
Makefile

makefile
CC = gcc
CFLAGS = -std=c11 -Wall -Wextra -pedantic
TARGET = task02_title
SRC = main.c html_title.c

all: $(TARGET)

$(TARGET): $(SRC)
	$(CC) $(CFLAGS) $(SRC) -o $(TARGET)

debug:
	$(CC) $(CFLAGS) -g $(SRC) -o $(TARGET)

clean:
	rm -f $(TARGET)
Команды сборки и запуска
bash
make
./task02_title page.html

make debug
./task02_title page.html

make clean
Ожидаемый результат
text
Title: Simple Demo Page
https://img/26.png

Комплект 2: Проекты с Meson
Задача 2.1: Статистика матрицы 2x2 (task03_meson_matrix_stats)
Постановка задачи
Программа читает матрицу 2x2 из текстового файла и вычисляет сумму элементов, след и определитель.

Структура проекта
text
task03_meson_matrix_stats/
├── main.c
├── matrix_stats.c
├── matrix_stats.h
├── matrix.txt
└── meson.build
Список идентификаторов
Имя	Тип	Описание
read_matrix_2x2	int ()(const char, int[2][2])	Чтение матрицы из файла
matrix_sum_2x2	int (*)(const int[2][2])	Сумма всех элементов
matrix_trace_2x2	int (*)(const int[2][2])	След матрицы
matrix_det_2x2	int (*)(const int[2][2])	Определитель матрицы
Код программы
matrix_stats.h

c
#ifndef MATRIX_STATS_H
#define MATRIX_STATS_H

int read_matrix_2x2(const char *filename, int m[2][2]);
int matrix_sum_2x2(const int m[2][2]);
int matrix_trace_2x2(const int m[2][2]);
int matrix_det_2x2(const int m[2][2]);

#endif
matrix_stats.c

c
#include "matrix_stats.h"
#include <stdio.h>

int read_matrix_2x2(const char *filename, int m[2][2]) {
    FILE *f;
    
    f = fopen(filename, "r");
    if (!f) {
        return -1;
    }
    
    if (fscanf(f, "%d %d %d %d", &m[0][0], &m[0][1], &m[1][0], &m[1][1]) != 4) {
        fclose(f);
        return -1;
    }
    
    fclose(f);
    return 0;
}

int matrix_sum_2x2(const int m[2][2]) {
    return m[0][0] + m[0][1] + m[1][0] + m[1][1];
}

int matrix_trace_2x2(const int m[2][2]) {
    return m[0][0] + m[1][1];
}

int matrix_det_2x2(const int m[2][2]) {
    return m[0][0] * m[1][1] - m[0][1] * m[1][0];
}
main.c

c
#include "matrix_stats.h"
#include <stdio.h>

int main(int argc, char *argv[]) {
    int m[2][2];
    const char *path = "matrix.txt";
    
    if (argc > 1) {
        path = argv[1];
    }
    
    if (read_matrix_2x2(path, m) != 0) {
        printf("Cannot read matrix file: %s\n", path);
        return 1;
    }
    
    printf("Sum: %d\n", matrix_sum_2x2(m));
    printf("Trace: %d\n", matrix_trace_2x2(m));
    printf("Det: %d\n", matrix_det_2x2(m));
    
    return 0;
}
matrix.txt

text
1 2 3 4
meson.build

meson
project('task03_meson_matrix_stats', 'c',
    default_options : ['c_std=c11', 'warning_level=2'])

executable('task03_matrix_v2', ['main.c', 'matrix_stats.c'])
Команды сборки и запуска
bash
rm -rf build
meson setup build
meson compile -C build
./build/task03_matrix_v2 matrix.txt
Ожидаемый результат
text
Sum: 10
Trace: 5
Det: -2
https://img/27.png

Задача 2.2: Подсчёт INFO/WARN/ERROR (task04_meson_log_stats)
Постановка задачи
Программа читает текстовый лог-файл и подсчитывает количество строк с метками INFO, WARN и ERROR. В одном meson.build объявлены две исполняемые цели.

Структура проекта
text
task04_meson_log_stats/
├── main.c
├── log_stats.c
├── log_stats.h
├── app.log
└── meson.build
Список идентификаторов
Имя	Тип	Описание
LogStats	struct	Структура со счётчиками (info_count, warn_count, error_count)
analyze_log_file	int ()(const char, LogStats*)	Анализ лог-файла
Код программы
log_stats.h

c
#ifndef LOG_STATS_H
#define LOG_STATS_H

typedef struct {
    int info_count;
    int warn_count;
    int error_count;
} LogStats;

int analyze_log_file(const char *filename, LogStats *stats);

#endif
log_stats.c

c
#include "log_stats.h"
#include <stdio.h>
#include <string.h>

int analyze_log_file(const char *filename, LogStats *stats) {
    FILE *f;
    char line[512];
    
    f = fopen(filename, "r");
    if (!f) {
        return -1;
    }
    
    stats->info_count = 0;
    stats->warn_count = 0;
    stats->error_count = 0;
    
    while (fgets(line, sizeof(line), f)) {
        if (strstr(line, "INFO") != NULL) {
            stats->info_count++;
        }
        if (strstr(line, "WARN") != NULL) {
            stats->warn_count++;
        }
        if (strstr(line, "ERROR") != NULL) {
            stats->error_count++;
        }
    }
    
    fclose(f);
    return 0;
}
main.c

c
#include "log_stats.h"
#include <stdio.h>

int main(int argc, char *argv[]) {
    LogStats stats;
    const char *path = "app.log";
    
    if (argc > 1) {
        path = argv[1];
    }
    
    if (analyze_log_file(path, &stats) != 0) {
        printf("Cannot read log file: %s\n", path);
        return 1;
    }
    
    printf("INFO: %d\n", stats.info_count);
    printf("WARN: %d\n", stats.warn_count);
    printf("ERROR: %d\n", stats.error_count);
    
    return 0;
}
app.log

text
[INFO] Application started
[INFO] Loading configuration
[WARN] Deprecated option used
[ERROR] Connection refused
[INFO] Retry in 5 seconds
[WARN] Timeout reached
meson.build

meson
project('task04_meson_log_stats', 'c',
    default_options : ['c_std=c11', 'warning_level=2'])

executable('task04_logstats', ['main.c', 'log_stats.c'])
executable('task04_logstats_copy', ['main.c', 'log_stats.c'])
Команды сборки и запуска
bash
rm -rf build
meson setup build
meson compile -C build
./build/task04_logstats app.log
./build/task04_logstats_copy app.log
Ожидаемый результат
text
INFO: 3
WARN: 2
ERROR: 1
https://img/28.png

Мини-шпаргалка по командам
GNU Make
Команда	Действие
make	Собрать проект по основной цели
make run	Собрать и запустить задачу 1.1
make debug	Собрать задачу 1.2 с отладочной информацией
make clean	Удалить результаты сборки
Meson
Команда	Действие
meson setup build	Создать и настроить каталог сборки
meson compile -C build	Выполнить сборку
rm -rf build	Удалить старый каталог сборки
Финальный чек-лист сдачи
Создан корневой каталог 05-lab-build-make-meson

Информация о студенте
Ерохина Анастасия Андреевна, ИВТ 1-1
