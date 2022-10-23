# YaMDb REST API

## Описание

Проект реализует REST API для сервиса YaMDb — базы отзывов о различных произведениях (фильмах, книгах, музыке и др.). API предоставляет следующие возможности:

**Для неавторизованных пользователей**

- Самостоятельная регистрация с отправкой кода подтверждения на e-mail;
- Получение JWT токена для авторизации;
- Получение информации о произведениях, отзывах к ним, просмотр комментариев.

**Для авторизованных пользователей**

- Получение и изменение данных своей учетной записи;
- Добавление, редактирование и удаление своих отзывов на произведения;
- Добавление, редактирование и удаление своих комментариев к отзывам.

**Для модераторов**

- Редактирование и удаление отзывов и комментариев других пользователей.

**Для администраторов**

- Добавление и удаление категорий и жанров произведений;
- Добавление, изменение и удаление информации о произведениях;
- Регистрация новых пользователей;
- Просмотр и изменение информации о зарегистрированных пользователях;
- Удаление пользователей.

## Как запустить проект:

1. Установить docker и docker-compose согласно [инструкции](https://docs.docker.com/engine/install/).

2. Клонировать репозиторий и перейти в директорию развёртывания инфраструктуры:

```
git clone git@github.com:hikjik/api_yamdb.git
```

```
cd api_yamdb/infra
```

3. Создать файл .env с переменными окружения для работы с базой данных по следующему образцу:

```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
```

4. Запустить контейнеры:

```
docker-compose up -d
```

5. Выполнить миграции, создать суперпользователя и собрать статику с помощью следующих команд:

```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```

6. (Опционально) Загрузить резервную копию базы данных:

```
docker-compose run web python manage.py loaddata fixtures.json
```

## Документация

После запуска проекта документация доступна по [ссылке.](http://127.0.0.1/redoc)

## Примеры запросов к API

1. Самостоятельная регистрация с отправкой кода подтверждения на e-mail

```
POST /api/v1/auth/signup/
```

Тело запроса:

```json
{
    "username": "martin",
    "email": "martin@mail.ru"
}
```

Пример ответа:

```json
{
    "username": "martin",
    "email": "martin@mail.ru"
}
```

2. Получение JWT токена

```
POST /api/v1/auth/signup/
```

Тело запроса:

```json
{
    "username": "martin",
    "confirmation_code": "62v-e034fd8e1432fb4c2701"
}
```

Пример ответа:

```json
{
    "token": "string"
}
```

3. Получение списка произведений

```
GET /api/v1/titles?year=1971&category=music&genre=rock&name=deep
```

Пример ответа:

```json
{
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 30,
            "name": "Deep Purple — Smoke on the Water",
            "year": 1971,
            "rating": 10,
            "description": "",
            "genre": [
                {
                    "name": "Рок",
                    "slug": "rock"
                }
            ],
            "category": {
                "name": "Музыка",
                "slug": "music"
            }
        }
    ]
}
```
