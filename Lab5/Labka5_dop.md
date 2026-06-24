# Лабораторная работа №1
## Rust: первые программы, функции, структуры и перечисления

| **Студент** | Ерохина Анастасия Андреевна |
| :--- | :--- |
| **Группа** | ИВТ 1-1 |
| **Дата рождения** | 14.12.2006 |

---

## Задача 1.1. Карточка студента

### Код программы (`src/main.rs`)

```rust
use std::io;

fn read_trimmed_line(prompt: &str) -> String {
    println!("{}", prompt);

    let mut input = String::new();

    io::stdin()
        .read_line(&mut input)
        .expect("failed to read input line");

    input.trim().to_string()
}

fn parse_year(text: &str) -> u32 {
    text.trim()
        .parse::<u32>()
        .expect("year must be a positive integer")
}

fn calculate_age(birth_year: u32, current_year: u32) -> u32 {
    current_year.saturating_sub(birth_year)
}

fn build_student_card(name: &str, group: &str, birth_year: u32, current_year: u32) -> String {
    let age = calculate_age(birth_year, current_year);
    format!(
        "--- Student card ---\nName: {}\nGroup: {}\nBirth year: {}\nCurrent year: {}\nAge: {}",
        name, group, birth_year, current_year, age
    )
}

fn main() {
    let name = read_trimmed_line("Enter student name:");
    let group = read_trimmed_line("Enter group:");
    let birth_year_text = read_trimmed_line("Enter birth year:");
    let current_year_text = read_trimmed_line("Enter current year:");

    let birth_year = parse_year(&birth_year_text);
    let current_year = parse_year(&current_year_text);

    let card = build_student_card(&name, &group, birth_year, current_year);
    println!("{}", card);
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn calculates_age_for_normal_years() {
        assert_eq!(calculate_age(2006, 2026), 20);
    }

    #[test]
    fn parses_year_with_spaces() {
        assert_eq!(parse_year(" 2026 "), 2026);
    }

    #[test]
    fn builds_card_with_expected_lines() {
        let card = build_student_card("Anastasia", "IVT-1-1", 2006, 2026);
        assert!(card.contains("Name: Anastasia"));
        assert!(card.contains("Group: IVT-1-1"));
        assert!(card.contains("Age: 20"));
    }
}

```

### Файл конфигурации (`Cargo.toml`)

```toml
[package]
name = "task01_student_card"
version = "0.1.0"
edition = "2024"

[dependencies]

```

### Результат выполнения

```text
Enter student name:
Anastasia
Enter group:
IVT-1-1
Enter birth year:
2006
Enter current year:
2026
--- Student card ---
Name: Anastasia
Group: IVT-1-1
Birth year: 2006
Current year: 2026
Age: 20

```

### Проверка форматирования и тестов

```bash
$ cargo fmt
$ cargo test
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running unittests src/main.rs (target/debug/deps/task01_student_card-xxx)

running 3 tests
test tests::calculates_age_for_normal_years ... ok
test tests::parses_year_with_spaces ... ok
test tests::builds_card_with_expected_lines ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

```

---

## Задача 1.2. Мини-каталог книг

### Код программы (`src/main.rs`)

```rust
#[derive(Clone, Copy, Debug, PartialEq, Eq)]
enum Genre {
    Fiction,
    Science,
    History,
}

impl Genre {
    fn title(&self) -> &'static str {
        match self {
            Genre::Fiction => "Fiction",
            Genre::Science => "Science",
            Genre::History => "History",
        }
    }
}

#[derive(Debug)]
struct Book {
    title: String,
    author: String,
    year: u16,
    genre: Genre,
    pages: u16,
}

impl Book {
    fn new(title: &str, author: &str, year: u16, genre: Genre, pages: u16) -> Self {
        Self {
            title: title.to_string(),
            author: author.to_string(),
            year,
            genre,
            pages,
        }
    }

    fn describe(&self) -> String {
        format!(
            "{} - {}, {}, {}, {} pages",
            self.title,
            self.author,
            self.year,
            self.genre.title(),
            self.pages
        )
    }
}

fn total_pages(books: &[Book]) -> u32 {
    books.iter().map(|book| u32::from(book.pages)).sum()
}

fn count_by_genre(books: &[Book], genre: Genre) -> usize {
    books.iter().filter(|book| book.genre == genre).count()
}

fn newest_book(books: &[Book]) -> Option<&Book> {
    books.iter().max_by_key(|book| book.year)
}

fn main() {
    let books = vec![
        Book::new("The Rust Journey", "I. Ferris", 2024, Genre::Science, 320),
        Book::new("Safe Systems", "A. Memory", 2022, Genre::Science, 280),
        Book::new("Northern Tales", "L. Snow", 2019, Genre::Fiction, 210),
        Book::new("History of Code", "M. Archive", 2021, Genre::History, 260),
    ];

    println!("Library catalog");
    for book in &books {
        println!("- {}", book.describe());
    }
    println!("Total books: {}", books.len());
    println!("Science books: {}", count_by_genre(&books, Genre::Science));
    println!("Total pages: {}", total_pages(&books));
    if let Some(book) = newest_book(&books) {
        println!("Newest book: {} ({})", book.title, book.year);
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    fn sample_books() -> Vec<Book> {
        vec![
            Book::new("A", "Author 1", 2020, Genre::Science, 100),
            Book::new("B", "Author 2", 2022, Genre::Fiction, 150),
            Book::new("C", "Author 3", 2021, Genre::Science, 200),
        ]
    }

    #[test]
    fn counts_books_by_genre() {
        let books = sample_books();
        assert_eq!(count_by_genre(&books, Genre::Science), 2);
        assert_eq!(count_by_genre(&books, Genre::Fiction), 1);
        assert_eq!(count_by_genre(&books, Genre::History), 0);
    }

    #[test]
    fn calculates_total_pages() {
        let books = sample_books();
        assert_eq!(total_pages(&books), 450);
    }

    #[test]
    fn finds_newest_book() {
        let books = sample_books();
        let newest = newest_book(&books);
        assert!(newest.is_some());
        assert_eq!(newest.unwrap().year, 2022);
    }
}

```

### Файл конфигурации (`Cargo.toml`)

```toml
[package]
name = "task02_library_catalog"
version = "0.1.0"
edition = "2024"

[dependencies]

```

### Результат выполнения

```text
Library catalog
- The Rust Journey - I. Ferris, 2024, Science, 320 pages
- Safe Systems - A. Memory, 2022, Science, 280 pages
- Northern Tales - L. Snow, 2019, Fiction, 210 pages
- History of Code - M. Archive, 2021, History, 260 pages
Total books: 4
Science books: 2
Total pages: 1070
Newest book: The Rust Journey (2024)

```

### Проверка форматирования и тестов

```bash
$ cargo fmt
$ cargo test
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running unittests src/main.rs (target/debug/deps/task02_library_catalog-xxx)

running 3 tests
test tests::counts_books_by_genre ... ok
test tests::calculates_total_pages ... ok
test tests::finds_newest_book ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.00s

```

---

## Информация о студенте

**Ерохина Анастасия Андреевна, группа ИВТ 1-1**

```</Book>

```
