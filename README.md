# API для Yatube

## Описание

**Yatube API** — REST API для социальной сети Yatube. Позволяет работать с
публикациями, комментариями, сообществами и подписками через HTTP-запросы.

Возможности API:

- получение, создание, редактирование и удаление публикаций (постов);
- получение, создание, редактирование и удаление комментариев к постам;
- просмотр списка сообществ и информации об отдельном сообществе;
- подписка на других авторов и просмотр своих подписок;
- аутентификация по JWT-токену.

Права доступа:

- неаутентифицированные пользователи имеют доступ к API только на чтение;
- аутентифицированные пользователи могут изменять и удалять только свой контент;
- эндпоинт `/follow/` доступен только аутентифицированным пользователям.

## Технологии

- Python 3.10
- Django 3.2
- Django REST Framework 3.12
- Simple JWT (djangorestframework-simplejwt)

## Установка

1. Клонируйте репозиторий и перейдите в него:

   ```bash
   git clone https://github.com/<username>/api-final-yatube-ad.git
   cd api-final-yatube-ad
   ```

2. Создайте и активируйте виртуальное окружение:

   ```bash
   # Windows
   python -m venv venv
   venv\Scripts\activate

   # Linux / macOS
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Установите зависимости:

   ```bash
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   ```

4. Выполните миграции:

   ```bash
   cd yatube_api
   python manage.py migrate
   ```

5. Запустите сервер разработки:

   ```bash
   python manage.py runserver
   ```

После запуска документация API будет доступна по адресу
<http://127.0.0.1:8000/redoc/>.

## Примеры запросов

### Получение JWT-токена

```
POST /api/v1/jwt/create/
```

Тело запроса:

```json
{
  "username": "user",
  "password": "password"
}
```

Ответ:

```json
{
  "refresh": "<refresh_token>",
  "access": "<access_token>"
}
```

Полученный `access`-токен передаётся в заголовке каждого запроса:

```
Authorization: Bearer <access_token>
```

### Получение списка публикаций

```
GET /api/v1/posts/
```

С пагинацией:

```
GET /api/v1/posts/?limit=10&offset=0
```

### Создание публикации

```
POST /api/v1/posts/
```

Тело запроса:

```json
{
  "text": "Текст моего поста",
  "group": 1
}
```

Ответ:

```json
{
  "id": 1,
  "author": "user",
  "text": "Текст моего поста",
  "pub_date": "2026-06-01T20:00:00Z",
  "image": null,
  "group": 1
}
```

### Добавление комментария

```
POST /api/v1/posts/{post_id}/comments/
```

Тело запроса:

```json
{
  "text": "Отличный пост!"
}
```

### Подписка на пользователя

```
POST /api/v1/follow/
```

Тело запроса:

```json
{
  "following": "another_user"
}
```

### Поиск по подпискам

```
GET /api/v1/follow/?search=another_user
```

## Эндпоинты

| Метод | Адрес | Описание |
| --- | --- | --- |
| GET, POST | `/api/v1/posts/` | Список публикаций / создание |
| GET, PUT, PATCH, DELETE | `/api/v1/posts/{id}/` | Работа с публикацией |
| GET, POST | `/api/v1/posts/{post_id}/comments/` | Комментарии к публикации |
| GET, PUT, PATCH, DELETE | `/api/v1/posts/{post_id}/comments/{id}/` | Работа с комментарием |
| GET | `/api/v1/groups/` | Список сообществ |
| GET | `/api/v1/groups/{id}/` | Информация о сообществе |
| GET, POST | `/api/v1/follow/` | Подписки пользователя |
| POST | `/api/v1/jwt/create/` | Получить JWT-токен |
| POST | `/api/v1/jwt/refresh/` | Обновить JWT-токен |
| POST | `/api/v1/jwt/verify/` | Проверить JWT-токен |

## Автор

matveimart1
