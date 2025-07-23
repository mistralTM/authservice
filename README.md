# authservice
Сервис предоставляет REST API для регистрации, аутентификации и управления пользователями с использованием JWT токенов. Реализована role-based авторизация с тремя ролями: ADMIN, PREMIUM_USER и GUEST.

Технологии:
1. Java 21
2. Spring Boot 3.2.4
3. Spring Security
4. JWT (JSON Web Tokens)
5. H2 Database (in-memory)
6. Lombok
7. Maven

Требования к запуску приложения:
1. Установленный JDK 21
2. Maven 3.9+
3. Доступ к порту 8080

Для доступа к встроенной базе данных H2:
1. Откройте в браузере с запущенным сервером: http://localhost:8080/h2-console
2. Введите параметры подключения:
JDBC URL: jdbc:h2:mem:authdb
User Name: admin
Password: admin
3. Нажмите "Connect"

Для проверки основных сценариев выполняла следующие запросы в cmd при запущенном сервере
1. Регистрация пользователя с ролью гость:
curl -X POST http://localhost:8080/api/auth/register -H "Content-Type: application/json" -d "{\"username\":\"testadmin\",\"password\":\"admin123\",\"email\":\"admin@test.com\"}"
2. Попытка повторной регистрации:
   curl -X POST http://localhost:8080/api/auth/register -H "Content-Type: application/json" -d "{\"username\":\"testadmin\",\"password\":\"admin123\",\"email\":\"admin@test.com\"}"
3. Авторизация:
   curl -X POST http://localhost:8080/api/auth/login -H "Content-Type: application/json" -d "{\"username\":\"testadmin\",\"password\":\"admin123\"}"
4. Обновление токена (подставить ваш refreshToken):
   curl -X POST http://localhost:8080/api/auth/refresh -H "Content-Type: application/json" -d "ВАШ_refresh_Token"
5. Доступ к защищенному API (подставить ваш accessToken):
   curl -X GET http://localhost:8080/api/users -H "Authorization: Bearer ВАШ_access_Token" -H "Content-Type: application/json"
6. Выход из системы (подставить ваш accessToken):
   curl -X POST http://localhost:8080/api/auth/logout -H "Authorization: Bearer ВАШ_access_Token" -H "Content-Type: application/json"
7. Проверка просроченного токена:
   curl -X GET http://localhost:8080/api/users -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.expired.token" -H "Content-Type: application/json"
8. Создание пользователя с ролью премиум-гость:
   curl -X POST http://localhost:8080/api/auth/register -H "Content-Type: application/json" -d "{\"username\":\"premiumuser\",\"password\":\"premium123\",\"email\":\"premium@test.com\",\"roles\":[\"PREMIUM_USER\"]}"
9. Создание пользователя с ролью админ:
   curl -X POST http://localhost:8080/api/auth/register -H "Content-Type: application/json" -d "{\"username\":\"adminn\",\"password\":\"adminn123\",\"email\":\"adminn@test.com\",\"roles\":[\"ADMIN\"]}"
10. Проверка доступа H2 Console:
    curl -X GET http://localhost:8080/api/h2-console
11. Проверка неверных данных:
    curl -X POST http://localhost:8080/api/auth/login -H "Content-Type: application/json" -d "{\"username\":\"wronguser\",\"password\":\"wrongpass\"}"

После выполнения запросов проверила данные в H2 Console:
1. Просмотр всех пользователей
SELECT * FROM USERS;
2. Просмотр ролей пользователей
SELECT u.username, r.role FROM users u JOIN user_roles r ON u.id = r.user_id;

Для отладки включено подробное логирование. Логи можно найти в консоли