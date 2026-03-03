

| # | Категорія | Де (файл/рядок) | Проблема | Чому це погано | Як виправити |
|---|------------|----------------|-----------|----------------|--------------|
| 1 | Security | DB_URL / DB_USER / DB_PASS | Hardcoded credentials | Витік доступів при публікації коду | Винести у environment variables або config |
| 2 | Security | main(): логування login | Логується пароль | Витік секретів у логах | Не логувати пароль |
| 3 | Correctness | main(): email == "admin@local" | Неправильне порівняння рядків | Порівняння посилань замість значень | Використовувати equals() |
| 4 | Correctness | buildReport(): email.trim() | Можливий NullPointerException | Програма впаде якщо email == null | Додати перевірку на null |
| 5 | Maintainability | static cache, lastUserEmail | Глобальний mutable state | Складність тестування, race conditions | Інкапсулювати стан у сервісі |
| 6 | Security | cache.put("pw:" + email, password) | Зберігання пароля у відкритому вигляді | Ризик витоку даних | Не кешувати пароль, використовувати hashing |
| 7 | Security | buy(): відсутня перевірка qty | Можна передати від’ємне значення | Некоректні замовлення | Додати валідацію qty > 0 |
| 8 | Resource Leak | query(), update() | Не закриваються ресурси БД | Витік з'єднань, падіння БД | Використати try-with-resources |

