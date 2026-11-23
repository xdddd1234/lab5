# Специфікація UML Component Diagram

## 1. Загальна інформація

**Лабораторна робота:** №5 — UML Component Diagram.

**Система:** бібліотечна система.

**Основний процес:** UC-03 «Видача книги».

**Мета:** визначити архітектурні компоненти системи, їхні інтерфейси, залежності, межі та зв'язок із попередніми моделями.

## 2. Межа системи

Межа системи позначена як **Library System**.

Усередині розміщені програмні компоненти, що забезпечують видачу книги.

Зовнішній користувач — **Бібліотекар**. Він взаємодіє з Library UI.

## 3. Архітектурні шари

### Presentation Layer

**Library UI** — інтерфейс бібліотекаря, через який надсилається запит на видачу книги та відображається результат.

### API Layer

**Library API** — приймає запити від Library UI, передає їх до LoanService та повертає результат.

### Business Logic Layer

**LoanService** — координує процес видачі книги, перевіряє бізнес-правила та створює запис Loan.

**ReaderService** — надає операції пошуку та перевірки читачів.

**BookService** — надає операції пошуку книг і перевірки їхньої доступності.

**Clock** — надає поточну дату для перевірки строку повернення.

### Data Access Layer

**ReaderRepository** — доступ до даних читачів.

**BookRepository** — доступ до даних книг.

**LoanRepository** — пошук активних видач і створення записів Loan.

### Data Layer

**Library Database** — зберігає сутності Reader, Book, Author, Category та Loan.

## 4. Provided та Required Interfaces

| Інтерфейс | Надає (Provided) | Використовує (Required) | Основні операції |
|---|---|---|---|
| ILibraryAPI | Library API | Library UI | submitLoan() |
| ILoanService | LoanService | Library API | issueBook() |
| IReaderService | ReaderService | LoanService | findReaderById() |
| IBookService | BookService | LoanService | findBookById(), isBookAvailable() |
| IReaderRepository | ReaderRepository | ReaderService | findById() |
| IBookRepository | BookRepository | BookService | findById() |
| ILoanRepository | LoanRepository | LoanService | findActiveLoan(), createLoan() |
| IClock | Clock | LoanService | today() |
| IDataStorage | Library Database | Три репозиторії | read(), write() |

Операції інтерфейсів наведені на концептуальному рівні. Їхні параметри та типи результатів можуть бути деталізовані під час реалізації.

## 5. Основні залежності

- Library UI → Library API.
- Library API → LoanService.
- LoanService → ReaderService.
- LoanService → BookService.
- LoanService → LoanRepository.
- LoanService → Clock.
- ReaderService → ReaderRepository.
- BookService → BookRepository.
- ReaderRepository → Library Database.
- BookRepository → Library Database.
- LoanRepository → Library Database.

Залежності спрямовані від компонентів, які потребують функціональності, до інтерфейсів компонентів, які її надають.

## 6. Підтримка UC-03 «Видача книги»

Система підтримує чотири сценарії.

**SC-01 — Успішна видача.**

LoanService перевіряє читача, книгу, її доступність та дату повернення. Після успішних перевірок створюється запис Loan.

**SC-02 — Книга вже видана.**

Система виявляє активну видачу та відхиляє запит.

**SC-03 — Неправильна дата повернення.**

LoanService перевіряє, що дата повернення не передує даті видачі.

**SC-04 — Читача не знайдено.**

ReaderService через ReaderRepository перевіряє існування читача. Якщо його немає, видача не виконується.

Для невдалих сценаріїв новий запис Loan не створюється.

## 7. Результати операції

Після обробки запиту система формує результат:

- **LoanIssued** — успішна видача.
- **LoanRejected** — відхилення запиту із зазначенням причини.

Результат передається за маршрутом:

LoanService → Library API → Library UI.

У цій архітектурі ці назви описують результати операції. Окремий зовнішній сервіс повідомлень не передбачається.

## 8. Зовнішні інтеграції

Payment, SMS/Email, OAuth/Identity Provider та зовнішні API не включені до поточного обсягу UC-03.

Попередні моделі не встановлюють обов'язкової залежності процесу видачі книги від таких інтеграцій.

Їх можна додати в майбутньому за наявності відповідних вимог.

## 9. Зв'язок із попередніми лабораторними

**Лабораторна №1 — ER-модель.**

Library Database зберігає сутності Reader, Book, Author, Category та Loan.

**Лабораторна №2 — Use Case.**

LoanService реалізує основні правила UC-03 та підтримує сценарії SC-01–SC-04.

**Лабораторна №3 — Sequence Diagram.**

Збережено основні ролі Library UI, LoanService та репозиторіїв. Library API, ReaderService і BookService деталізують архітектуру. Clock забезпечує отримання поточної дати.

**Лабораторна №4 — Activity Diagram.**

Компоненти забезпечують перевірки читача, книги та дати, створення Loan і повернення результату LoanIssued або LoanRejected.

## 10. Результати AI-аудиту

Під час аудиту визначено три основні питання:

1. Узгодження нових компонентів із лабораторною №3.
2. Деталізація provided/required-інтерфейсів.
3. Відображення Clock і способу передавання результатів операції.

Для їх усунення уточнено опис компонентів, додано IClock та основні операції інтерфейсів.

## 11. Висновок

Компонентна архітектура розділяє відповідальність між UI, API, бізнес-логікою, доступом до даних та базою даних.

Запропонована модель підтримує процес UC-03 «Видача книги» та пов'язана з результатами лабораторних №1–4.
