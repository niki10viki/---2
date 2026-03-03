# Code Review Report — BadStoreApp

## 🔎 Знайдені проблеми

| # | Категорія | Де (файл/рядок) | Проблема | Чому це погано | Як виправити |
|---|------------|----------------|-----------|----------------|--------------|
| 1 | Security | DB_URL / DB_USER / DB_PASS | Hardcoded credentials | Витік доступів при публікації коду | Винести у environment variables або config |
| 2 | Security | main(): логування login | Логується пароль | Витік секретів у логах | Не логувати пароль |
| 3 | Security | login(), buy(), getPrice(), buildReport() | SQL injection через конкатенацію | Можливе виконання довільного SQL | Використовувати PreparedStatement |
| 4 | Correctness | main(): email == "admin@local" | Неправильне порівняння рядків | Порівняння посилань замість значень | Використовувати equals() |
| 5 | Correctness | buildReport(): email.trim() | Можливий NullPointerException | Програма впаде якщо email == null | Додати перевірку на null |
| 6 | Correctness | buildReport(): for (i <= lines.size()) | Off-by-one помилка | IndexOutOfBoundsException | Використовувати i < lines.size() |
| 7 | Maintainability | Порожні catch блоки | Ігноруються винятки | Складно дебажити | Логувати або пробросити exception |
| 8 | Maintainability | static cache, lastUserEmail | Глобальний mutable state | Складність тестування, race conditions | Інкапсулювати стан у сервісі |
| 9 | Architecture | Весь код у BadStoreApp | God class | Порушення SRP, складність підтримки | Розділити на шари (UI / Service / Repository) |
|10 | Security | cache.put("pw:" + email, password) | Зберігання пароля у відкритому вигляді | Ризик витоку даних | Не кешувати пароль, використовувати hashing |
|11 | Security | buy(): відсутня перевірка qty | Можна передати від’ємне значення | Некоректні замовлення | Додати валідацію qty > 0 |
|12 | Resource Leak | query(), update() | Не закриваються ресурси БД | Витік з'єднань, падіння БД | Використати try-with-resources |

---

## 📌 Висновок

Під час code review було виявлено низку критичних проблем у категоріях **Security, Correctness, Maintainability та Architecture**.

Найбільш небезпечними є:
- SQL injection через конкатенацію SQL-запитів
- Логування та зберігання паролів у відкритому вигляді
- Hardcoded credentials
- Витоки ресурсів БД

Ці проблеми можуть призвести до компрометації системи, втрати даних або падіння застосунку.
Архітектурно застосунок має ознаки *God class* та порушує принцип розділення відповідальностей (SRP), оскільки UI, бізнес-логіка та доступ до даних знаходяться в одному класі.


