![example workflow](https://github.com/IlyaBoyur/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
# REST API для YamDB
Данное приложение предоставляет REST API для создания произведений с отзывами.

<br>

## Описание

Проект **YaMDb** собирает **отзывы** (**Review**) пользователей на **произведения** (**Titles**). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список **категорий**(**Category**) может быть расширен администратором (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).
Сами произведения в **YaMDb** не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

<br>

В каждой категории есть **произведения**: книги, фильмы или музыка. 
Произведению может быть присвоен **жанр** (**Genre**) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

<br>

Благодарные или возмущённые пользователи оставляют к произведениям текстовые **отзывы** (**Review**) и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — **рейтинг** (целое число). На одно произведение пользователь может оставить только один отзыв.

## Технологии
Django, Django REST Framework, PostgreSQL, Simple-JWT, Gunicorn, Docker, nginx, Google Cloud, git

## Запуск проекта
### 0. Клонировать репозиторий
### 1. Подготовить окружение
- Установить [Docker](https://docs.docker.com/get-docker/)
- Установите [docker-compose](https://docs.docker.com/compose/install/)
- Перейти в директорию проекта с docker-compose.yaml (по умолчанию: корень проекта)

### 2. Создать файл переменных среды
Создать в текущей папке файл .env со следующим содержимым:
```bash
SECRET_KEY=mysecretkey                  # секретный ключ Django (установите свой)
DJANGO_DEBUG_VALUE=True                 # запускаем в режиме отладки. False чтобы отключить 
DB_ENGINE=django.db.backends.postgresql # работаем с БД PostgreSQL
DB_NAME=postgres                        # имя базы данных
POSTGRES_USER=postgres                  # имя пользователя БД
POSTGRES_PASSWORD=postgres              # пароль пользователя БД (установите свой)
DB_HOST=db                              # название сервиса (контейнера)
DB_PORT=5432                            # порт для подключения к БД
```

### 3. Собрать и запустить контейнеры Docker
```bash
docker-compose up --build -d
```

### 4. Выполнить миграцию базы данных
```bash
docker-compose exec web python manage.py migrate
```

### 5. Заполнить базу данных тестовыми данными
Определить ID контейнера web
```bash
docker-compose ps
```
Тестовые данные (расположение по умолчанию: в корне проекта) перенести в контейнер и загрузить их в базу:
```bash
docker cp fixtures.json <ID контейнера web>:/
docker-compose exec web python manage.py loaddata /fixtures.json -v 3 --force-color
```
По окончании импорта консоль покажет примерно такое сообщение:
```bash
Installed 5504 object(s) from 1 fixture(s)
```

### 6. Собрать статические данные
```bash
docker-compose exec web python manage.py collectstatic
```

### 7. Профит! Проверить работоспособность проекта
- Корень проекта: http://localhost/api/v1/
- Админка: http://localhost/admin
- Документация: http://localhost/redoc

<br>

### Развернутый проект:
- Деплой на Google Cloud [здесь](http://api-yamdb.ml/api/v1/)
- Админка: http://localhost/admin, логин admin@admin.su, пароль admin

<br>

### Авторы
- [Илья Боюр](https://github.com/IlyaBoyur): тимлид, отвественен за сдачу проекта, за модели Category, Genre, Title, Review, Comment, рейтинги для Title
- [Артем Третьяков](https://github.com/ingvior-inc): кросс-ревьюер, ответственен за авторизацию, модели пользователей


