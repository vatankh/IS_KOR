# Отчет по Этапу 1: Библиотечная информационная система

## Предметная область
Предметной областью является управление библиотечными ресурсами. Разрабатываемая информационная система предназначена для автоматизации процессов управления книгами, членами библиотеки и их активностями, включая выдачу и возврат книг.

## Описание предметной области
Библиотека предоставляет пользователям (читателям) возможность получать книги на определенный срок. Для этого библиотекарь ведет учет доступных книг, регистрирует новых читателей и отслеживает историю выдачи/возврата книг. Система должна учитывать текущую доступность книг, их состояние, а также сроки возврата.

### Основные объекты предметной области:
- **Книга (Book)**: уникальный идентификатор, название, автор, жанр, год издания, статус (доступна/на руках).
- **Член библиотеки (Member)**: уникальный идентификатор, имя, контактные данные, список текущих книг на руках.
- **Транзакция (Transaction)**: информация о выдаче/возврате книги, включая даты и участников (члена и книгу).

---

## Цель информационной системы
Информационная система предназначена для упрощения управления библиотечными процессами, устранения ошибок ручного учета и обеспечения быстрого доступа к информации о книгах и членах библиотеки.

### Задачи системы:
1. **Учет книг**:
   - Создание, обновление, удаление информации о книгах.
   - Управление статусом книги (доступна, выдана).
2. **Управление членами**:
   - Регистрация новых членов.
   - Отображение текущих задолженностей (книги на руках, просрочки).
3. **Работа с транзакциями**:
   - Оформление выдачи и возврата книг.
   - Хранение истории всех операций.
4. **Отчетность**:
   - Генерация отчетов по популярности книг.
   - Отчеты о задолженностях.

---

## Функциональные требования
1. **Создание, редактирование и удаление записей о книгах.**
2. **Регистрация и управление членами библиотеки.**
3. **Возможность поиска книг и фильтрации по автору, жанру, статусу и году издания.**
4. **Оформление транзакций (выдача и возврат книг).**
5. **Просмотр истории операций по конкретным книгам и членам.**
6. **Авторизация пользователей с разделением ролей (библиотекарь и администратор).**

## Нефункциональные требования
1. **Высокая производительность**: система должна быстро обрабатывать запросы на поиск и фильтрацию.
2. **Безопасность данных**: хеширование паролей пользователей, ограничение доступа по ролям.
3. **Надежность**: резервное копирование базы данных.
4. **Юзабилити**: удобный и интуитивно понятный пользовательский интерфейс.
5. **Масштабируемость**: возможность добавления новых функций без значительных изменений.

---

## Основные прецеденты использования

Прецеденты использования описывают ключевые сценарии взаимодействия пользователя с системой. Для каждого прецедента приведено подробное описание, включая участников, последовательность действий и результат. Ниже также представлены модели основных прецедентов, которые можно согласовать с преподавателем.

---

### 1. **Добавление новой книги**
**Участники**: Библиотекарь  
**Описание**: Библиотекарь добавляет в систему информацию о новой книге, чтобы она стала доступной для выдачи.  

**Шаги**:  
1. Библиотекарь открывает интерфейс для добавления книги.  
2. Заполняет поля: название, автор, жанр, год издания, количество экземпляров.  
3. Сохраняет данные.  
4. Система проверяет корректность данных и записывает книгу в базу данных.  

**Результат**: Новая книга доступна в списке для поиска и выдачи.

![Image 1](./11.png)

---

### 2. **Регистрация нового члена библиотеки**
**Участники**: Библиотекарь  
**Описание**: Новый пользователь регистрируется в системе для получения доступа к услугам библиотеки.  

**Шаги**:  
1. Библиотекарь открывает интерфейс регистрации нового члена.  
2. Вводит данные: имя, контактная информация (телефон, email).  
3. Назначает уникальный идентификатор.  
4. Сохраняет запись.  

**Результат**: Новый член библиотеки добавлен в систему, ему присваивается учетная запись.

---

### 3. **Выдача книги**
**Участники**: Библиотекарь, Член библиотеки  
**Описание**: Член библиотеки получает книгу на определенный срок.  

**Шаги**:  
1. Библиотекарь выбирает книгу из доступного списка.  
2. Указывает члена библиотеки, которому выдается книга.  
3. Система фиксирует транзакцию с датой выдачи и сроком возврата.  
4. Статус книги обновляется на «Выдана».  

**Результат**: Книга закреплена за членом библиотеки, срок возврата отслеживается системой.

---

### 4. **Возврат книги**
**Участники**: Библиотекарь, Член библиотеки  
**Описание**: Член библиотеки возвращает книгу, и она снова становится доступной для выдачи.  

**Шаги**:  
1. Библиотекарь сканирует или выбирает книгу из списка.  
2. Система обновляет статус книги на «Доступна».  
3. В случае просрочки система фиксирует штраф и уведомляет библиотекаря.  

**Результат**: Книга возвращена в библиотеку и доступна для других пользователей.

![Image 2](./44.png)


---

### 5. **Поиск книги**
**Участники**: Член библиотеки, Библиотекарь  
**Описание**: Пользователь ищет книгу по различным параметрам (название, автор, жанр).  

**Шаги**:  
1. Пользователь вводит параметры поиска в интерфейсе.  
2. Система отображает результаты поиска.  
3. Пользователь уточняет запрос или выбирает книгу из результатов.  

**Результат**: Пользователь находит нужную книгу.

![Image 2](./55.png)

---

### 6. **Удаление книги**
**Участники**: Администратор  
**Описание**: Администратор удаляет книгу из базы данных (например, при ее утере или повреждении).  

**Шаги**:  
1. Администратор выбирает книгу из списка.  
2. Подтверждает удаление.  
3. Система проверяет наличие активных транзакций с этой книгой.  
4. Если книга не выдана, она удаляется из базы.  

**Результат**: Книга удалена из системы.

---

### 7. **Создание отчета**
**Участники**: Администратор  
**Описание**: Администратор генерирует отчет о деятельности библиотеки.  

**Шаги**:  
1. Администратор открывает интерфейс отчетов.  
2. Выбирает тип отчета: популярность книг, список задолженностей, транзакции за период.  
3. Система обрабатывает данные и отображает готовый отчет.  
4. Администратор может сохранить или распечатать отчет.  

**Результат**: Сформирован отчет, доступный для анализа.

---

### Модели прецедентов использования

#### 1. **Use Case Diagram**
- **Акторы**:
  - Библиотекарь
  - Член библиотеки
  - Администратор  
- **Основные действия**:
  - Управление книгами: добавление, удаление, поиск.
  - Управление членами: регистрация, просмотр.
  - Управление транзакциями: выдача, возврат.
  - Создание отчетов.  

```plaintext


---

## Архитектура системы
### Выбранные технологии и фреймворки:
- **Frontend**: React (для динамического пользовательского интерфейса).
- **Backend**: Spring MVC (реализация бизнес-логики и API).
- **База данных**: PostgreSQL (хранение данных о книгах, членах и транзакциях).

### Компоненты системы:
1. **Frontend**:
   - React-приложение для работы пользователей.
   - Интерактивные формы для создания/редактирования данных.
2. **Backend**:
   - REST API для взаимодействия с клиентом.
   - Сервисы для обработки бизнес-логики.
3. **База данных**:
   - Таблицы для хранения данных о книгах, членах, транзакциях.
   - Индексы для оптимизации поиска.

### Модели данных:
- **Книга (Book)**: таблица с атрибутами книги.
- **Член (Member)**: таблица для информации о членах библиотеки.
- **Транзакция (Transaction)**: таблица для хранения операций выдачи/возврата.

### Разделение уровней:
1. **Controller Layer**: обработка запросов пользователей.
2. **Service Layer**: реализация бизнес-логики.
3. **Repository Layer**: доступ к базе данных через Hibernate.

---

## Вывод
Разработанная архитектура и функциональные возможности системы обеспечат эффективное управление библиотечными ресурсами, снизят нагрузку на библиотекарей и повысят удобство работы для пользователей. Система соответствует требованиям курса и может быть успешно развернута на сервере Helios.