# Лабораторна робота №5 — UML Component Diagram

## 1. Мета роботи

Мета роботи — побудувати компонентну модель бібліотечної системи, показати її архітектуру, основні модулі, інтерфейси, залежності та межу системи.

Для моделювання обрано UML Component Diagram у PlantUML.

## 2. Опис системи

Бібліотечна система призначена для роботи з читачами, книгами та видачами.

Основний процес — UC-03 «Видача книги».

Бібліотекар вибирає читача, книгу та дату повернення. Система перевіряє дані, після чого створює Loan або повідомляє про відмову.

## 3. Компоненти системи

### Library UI

Інтерфейс бібліотекаря.

Відповідає за введення даних та відображення результатів операцій.

### Library API

Приймає запити від Library UI та передає їх відповідним сервісам.

### LoanService

Реалізує основну бізнес-логіку видачі книги.

Перевіряє читача, доступність книги та дату повернення. У разі успіху створює Loan.

### ReaderService

Надає операції для пошуку та перевірки читачів.

### BookService

Надає операції для пошуку книг та перевірки їх доступності.

### Data Access Layer

Містить компоненти:

- ReaderRepository
- BookRepository
- LoanRepository

Вони відповідають за доступ до даних читачів, книг і видач.

### Library Database

Зберігає сутності Reader, Book, Author, Category та Loan, визначені в лабораторній №1.

## 4. Інтерфейси

| Інтерфейс | Хто надає (provided) | Хто використовує (required) |
|---|---|---|
| ILibraryAPI | Library API | Library UI |
| ILoanService | LoanService | Library API |
| IReaderService | ReaderService | LoanService |
| IBookService | BookService | LoanService |
| IReaderRepository | ReaderRepository | ReaderService |
| IBookRepository | BookRepository | BookService |
| ILoanRepository | LoanRepository | LoanService |
| IDataStorage | Library Database | Репозиторії |

Provided-інтерфейс означає функціональність, яку компонент надає іншим.

Required-інтерфейс означає функціональність, від якої компонент залежить.

## 5. Залежності

Основний ланцюг викликів:

Library UI → Library API → LoanService.

LoanService використовує ReaderService, BookService та LoanRepository.

ReaderService звертається до ReaderRepository.

BookService звертається до BookRepository.

Репозиторії працюють із Library Database.

## 6. Межа системи

У межах бібліотечної системи знаходяться:

- Library UI
- Library API
- LoanService
- ReaderService
- BookService
- ReaderRepository
- BookRepository
- LoanRepository
- Library Database

Бібліотекар є зовнішнім користувачем системи.

## 7. Зовнішні інтеграції

У початкових сценаріях UC-03 немає обов'язкової інтеграції з платіжними системами, SMS, Email або OAuth.

Тому для цієї архітектурної моделі такі сервіси не додаються як реалізовані компоненти.

За потреби в майбутньому можна додати зовнішній Identity Provider для автентифікації або Email Gateway для повідомлень. Це буде окремим розширенням архітектури.

## 8. Зв'язок із попередніми лабораторними

### Лабораторна №1 — Модель даних

Library Database містить Reader, Book, Author, Category та Loan.

### Лабораторна №2 — Вимоги

LoanService реалізує UC-03 та чотири сценарії:

- SC-01 — успішна видача.
- SC-02 — книга вже видана.
- SC-03 — неправильна дата повернення.
- SC-04 — читача не знайдено.

### Лабораторна №3 — Sequence Diagram

Компоненти Library UI, LoanService та репозиторії відповідають основним учасникам sequence-діаграм.

Library API, ReaderService та BookService є архітектурною деталізацією.

### Лабораторна №4 — Activity Diagram

Компонентна модель підтримує процес видачі книги, перевірки, створення Loan та надсилання результату бібліотекарю.

## 9. Архітектурні рішення

Обрано багатошарову архітектуру:

1. Presentation Layer — Library UI.
2. API Layer — Library API.
3. Business Logic Layer — сервіси.
4. Data Access Layer — репозиторії.
5. Data Layer — Library Database.

Такий поділ дозволяє відокремити інтерфейс користувача, бізнес-правила та роботу з даними.

## 10. Висновок

Компонентна модель показує структуру бібліотечної системи, відповідальність модулів, їхні інтерфейси та залежності.

Архітектура підтримує процес UC-03 «Видача книги» та узгоджується з основними моделями попередніх лабораторних.
