# Трасування компонентної архітектури

## 1. Зв'язок компонентів із лабораторними №1–4

| Компонент | Lab 1 — ER | Lab 2 — вимоги та сценарії | Lab 3 — Sequence | Lab 4 — Activity |
|---|---|---|---|---|
| Library UI | — | UC-03 | LibraryUI | Введення даних, відображення результату |
| Library API | — | UC-03, REQ-13 | Деталізація взаємодії UI та LoanService | Приймання запиту |
| LoanService | Loan | SC-01–SC-04 | issueBook(), findActiveLoan(), createLoan() | Перевірки та видача |
| ReaderService | Reader | SC-04 | findReaderById() | Перевірка читача |
| BookService | Book | UC-03 | findBookById() | Перевірка існування книги |
| ReaderRepository | Reader | SC-04 | ReaderRepository | Пошук читача |
| BookRepository | Book | UC-03 | BookRepository | Пошук книги |
| LoanRepository | Loan | SC-01, SC-02 | findActiveLoan(), createLoan() | Перевірка доступності та створення видачі |
| Clock | loan_date, due_date | SC-03 | today() | Перевірка дати |
| Library Database | Reader, Book, Author, Category, Loan | UC-03 | Сховище даних за репозиторіями | Збереження видачі |
| Identity Provider | — | REQ-13 | Передумова авторизації бібліотекаря | Передумова виконання процесу |

## 2. Трасування залежностей

| Залежність | Призначення | Джерело |
|---|---|---|
| Library UI → Library API | Передавання запиту та отримання результату | UC-03 |
| Library API → Identity Provider | Перевірка автентифікації | REQ-13 |
| Library API → LoanService | Запуск видачі після перевірки прав доступу | issueBook(), UC-03 |
| LoanService → ReaderService | Перевірка читача | SC-04 |
| LoanService → BookService | Перевірка існування книги | findBookById() |
| LoanService → LoanRepository | Перевірка активної видачі та створення Loan | SC-01, SC-02 |
| LoanService → Clock | Отримання поточної дати | SC-03, today() |
| ReaderService → ReaderRepository | Пошук читача | findReaderById() |
| BookService → BookRepository | Пошук книги | findBookById() |
| ReaderRepository → Library Database | Читання Reader | ER-модель |
| BookRepository → Library Database | Читання Book | ER-модель |
| LoanRepository → Library Database | Читання та створення Loan | ER-модель |

## 3. Узгодження результатів операції

| Lab 3 | Lab 4 та Lab 5 |
|---|---|
| LoanCreated | LoanIssued |
| LoanError | LoanRejected |
| ValidationError | LoanRejected |

LoanRejected повинен містити причину відмови, щоб не втрачати інформацію про тип помилки.

## 4. Усунення дублювання

Перевірку доступності книги виконує лише LoanService через LoanRepository.

BookService відповідає за пошук книги, але не перевіряє активні видачі.

## 5. Нові архітектурні компоненти

Library API, ReaderService та BookService є деталізацією архітектури попередніх моделей.

Identity Provider додається для виконання REQ-13 без зміни ER-моделі бібліотечної системи.

Clock належить до інфраструктурного шару.
