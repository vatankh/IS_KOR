# Отчет по реализации базы данных

## 1. Введение

В рамках данной работы была разработана и реализована база данных для управления библиотекой. На основе анализа предметной области и описания бизнес-процессов была создана ER-диаграмма (/ER-diagram), которая включает не менее 10 сущностей и содержит хотя бы одно отношение "многие-ко-многим".
![Image 1](./er.png)

На основе ER-диаграммы была построена даталогическая модель (/DL-diagram), которая впоследствии была реализована в реляционной СУБД PostgreSQL.
![Image 2](./dl.png)

---

## 2. Реализация логической модели

Логическая модель включает следующие таблицы:
- `Book` — книги;
- `Member` — члены библиотеки;
- `Librarian` — библиотекари;
- `Transaction` — транзакции (выдача/возврат книг);
- `Genre` — жанры книг;
- `Author` — авторы;
- `Book_Author` — связь книг и авторов (многие-ко-многим);
- `Reservation` — бронирования книг;
- `Fine` — штрафы;
- `Report` — отчеты.

Каждая таблица была спроектирована с учетом требований целостности данных: определены первичные ключи, внешние ключи, ограничения, а также создан ряд триггеров для автоматизации процессов.
```sql
-- Table: Book
CREATE TABLE Book (
    book_id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    genre_id INT REFERENCES Genre(genre_id),
    status ENUM('available', 'issued'),
    publication_year YEAR
);

-- Table: Member
CREATE TABLE Member (
    member_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    email VARCHAR(255) UNIQUE
);

-- Table: Librarian
CREATE TABLE Librarian (
    librarian_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    role ENUM('librarian', 'admin')
);

-- Table: Transaction
CREATE TABLE Transaction (
    transaction_id SERIAL PRIMARY KEY,
    book_id INT REFERENCES Book(book_id),
    member_id INT REFERENCES Member(member_id),
    librarian_id INT REFERENCES Librarian(librarian_id),
    issue_date DATE NOT NULL,
    return_date DATE,
    fine DECIMAL(10, 2) DEFAULT 0
);

-- Table: Genre
CREATE TABLE Genre (
    genre_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

-- Table: Author
CREATE TABLE Author (
    author_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

-- Table: Book_Author (Many-to-Many)
CREATE TABLE Book_Author (
    book_id INT REFERENCES Book(book_id) ON DELETE CASCADE,
    author_id INT REFERENCES Author(author_id) ON DELETE CASCADE,
    PRIMARY KEY (book_id, author_id)
);

-- Table: Reservation
CREATE TABLE Reservation (
    reservation_id SERIAL PRIMARY KEY,
    book_id INT REFERENCES Book(book_id),
    member_id INT REFERENCES Member(member_id),
    reservation_date DATE NOT NULL
);

-- Table: Fine
CREATE TABLE Fine (
    fine_id SERIAL PRIMARY KEY,
    member_id INT REFERENCES Member(member_id),
    amount DECIMAL(10, 2) NOT NULL CHECK (amount >= 0),
    description TEXT
);

-- Table: Report
CREATE TABLE Report (
    report_id SERIAL PRIMARY KEY,
    type VARCHAR(255) NOT NULL,
    generated_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    data TEXT
);
```
---

## 3. Обеспечение целостности данных

Для обеспечения целостности данных реализованы следующие триггеры:
1. **Обновление статуса книги при выдаче** — триггер `trigger_update_book_status_on_issue` обновляет статус книги на `issued`.
2. **Обновление статуса книги при возврате** — триггер `trigger_update_book_status_on_return` возвращает статус книги на `available`.
3. **Удаление бронирования при выдаче книги** — триггер `trigger_delete_reservation_on_issue` удаляет запись о бронировании, если книга была выдана.
4. **Проверка доступности книги** — триггер `trigger_check_book_availability` запрещает выдачу книги, если она уже выдана.

---

## 4. Скрипты для управления базой данных

### Создание базы данных
Для создания базы данных был реализован скрипт, включающий определение всех таблиц и ограничений.

### Удаление базы данных
Предоставлен скрипт для полного удаления базы данных.

### Заполнение тестовыми данными
Скрипт заполнения включает тестовые данные для всех таблиц:
- Жанры;
- Авторы;
- Книги;
- Члены библиотеки;
- Библиотекари;
- Транзакции;
- Бронирования.

---

## 5. Функции и процедуры

Для выполнения ключевых операций в системе были разработаны следующие PL/pgSQL-функции:
1. **Выдача книги** (`issue_book`) — проверяет доступность книги, создает запись о выдаче и обновляет статус книги.
2. **Возврат книги** (`return_book`) — обновляет дату возврата и статус книги, а также фиксирует штраф (если есть).
3. **Генерация отчета по задолженностям** (`generate_overdue_report`) — формирует JSON-отчет о книгах, которые не были возвращены в срок.
4. **Поиск книг** (`search_books`) — позволяет искать книги по названию или имени автора.

---

## 6. Создание индексов

Для оптимизации производительности запросов были добавлены индексы:
1. **Индексы для таблицы `Book`:**
   - `idx_book_title` — для ускорения поиска книг по названию.
   - `idx_book_status` — для фильтрации книг по статусу.

2. **Индексы для таблицы `Transaction`:**
   - `idx_transaction_book_id` — для поиска транзакций по книге.
   - `idx_transaction_member_id` — для поиска транзакций по члену библиотеки.

3. **Индексы для таблицы `Member`:**
   - `idx_member_name` — для быстрого поиска членов по имени.

---

## 7. Заключение

Разработанная база данных полностью соответствует требованиям:
- Содержит не менее 10 сущностей.
- Реализовано отношение "многие-ко-многим" через таблицу `Book_Author`.
- Обеспечена целостность данных с помощью ограничений, триггеров и индексов.
- Созданы функции и процедуры для выполнения критически важных операций.

Данная база данных готова к дальнейшему использованию и масштабированию.
