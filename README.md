# Kittygram — Социальная сеть для котиков 🐱

Kittygram — это веб-приложение, где пользователи могут делиться фотографиями своих котиков, ставить лайки и оставлять комментарии.

Проект развернут по адресу: [https://xn--80aaitaehnh2bta.xn--p1ai](https://xn--80aaitaehnh2bta.xn--p1ai)

## Содержание
- [Технологии](#технологии)
- [Функционал](#функционал)
- [Установка и запуск](#установка-и-запуск)
- [Примеры запросов к API](#примеры-запросов-к-api)
- [Автор](#автор)

---

## Технологии

**Backend:**
- Python 3.12
- Django 5.1.1
- Django REST Framework
- Gunicorn 23.0.0
- PostgreSQL 13.10
- python-dotenv

**Frontend:**
- React
- Docker

**Инфраструктура:**
- Docker / Docker Compose
- Nginx
- Certbot (SSL сертификаты)
- GitHub Actions (CI/CD)

---

## Функционал

- Регистрация и авторизация пользователей
- Загрузка фотографий котиков
- Просмотр ленты с фотографиями
- SSL-шифрование (HTTPS)
- Docker-контейнеризация

---

## Установка и запуск

### Требования
- Docker 20.10+
- Docker Compose 2.0+
- Python 3.12 (для локальной разработки)

### Локальный запуск

1. **Клонируйте репозиторий:**
```bash
git clone https://github.com/Andrey75rr/kittygram_final.git
cd kittygram
```

2. **Создайте файл `.env` в корне проекта:**
```env
# Django
DEBUG=False
SECRET_KEY=your-secret-key-here
ALLOWED_HOSTS=localhost,домен,backend

# Database
POSTGRES_DB=kittygram_db
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_HOST=db
DB_PORT=5432
```

3. **Запустите проект через Docker Compose:**
```bash
docker compose -f docker-compose.production.yml up -d
```

4. **Выполните миграции и сбор статики:**
```bash
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

5. **Создайте суперпользователя (для доступа к админке):**
```bash
docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```

### Деплой на сервер

1. **Подготовка сервера:**
```bash
# Установка Docker
sudo apt update
sudo apt install docker.io docker-compose -y

# Установка Nginx и Certbot
sudo apt install nginx certbot python3-certbot-nginx -y
```

2. **Копирование файлов на сервер:**
```bash
scp -r ./* user@server-ip:/home/user/kittygram/
```

3. **Запуск проекта:**
```bash
cd /home/user/kittygram
docker compose -f docker-compose.production.yml up -d
```

4. **Настройка SSL сертификата:**
```bash
sudo certbot --nginx -d xn--80aaitaehnh2bta.xn--p1ai
```

---

## Примеры запросов к API

### Аутентификация

**Регистрация пользователя:**
```bash
POST /api/users/
Content-Type: application/json

{
  "username": "catlover",
  "email": "cat@example.com",
  "password": "securepassword123"
}
```

**Получение токена:**
```bash
POST /api/token/login/
Content-Type: application/json

{
  "username": "catlover",
  "password": "securepassword123"
}
```

**Пример ответа:**
```json
{
  "auth_token": "9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b"
}
```

---

### Котики

**Получение списка котиков:**
```bash
GET /api/cats/
Authorization: Token <ваш-токен>
```

**Пример ответа:**
```json
[
  {
    "id": 1,
    "name": "Барсик",
    "color": "рыжий",
    "birth_year": 2020,
    "image": "/media/cats/barsik.jpg",
    "owner": {
      "id": 1,
      "username": "catlover"
    },
    "achievements": []
  }
]
```

**Добавление котика:**
```bash
POST /api/cats/
Authorization: Token <ваш-токен>
Content-Type: multipart/form-data

{
  "name": "Мурка",
  "color": "серый",
  "birth_year": 2021,
  "image": <файл-изображения>,
  "achievements": []
}
```

**Получение конкретного котика:**
```bash
GET /api/cats/1/
Authorization: Token <ваш-токен>
```

**Удаление котика:**
```bash
DELETE /api/cats/1/
Authorization: Token <ваш-токен>
```

---

### Достижения

**Получение списка достижений:**
```bash
GET /api/achievements/
Authorization: Token <ваш-токен>
```

**Пример ответа:**
```json
[
  {
    "id": 1,
    "name": "Самый пушистый",
    "description": "Котик с самой густой шерстью"
  }
]
```

---

### Админ-панель

**Доступ к админке:** `https://xn--80aaitaehnh2bta.xn--p1ai/admin/`

---

## CI/CD

Проект использует GitHub Actions для автоматического деплоя:

1. Пуш в ветку `main` запускает тесты
2. После успешных тестов собираются Docker-образы
3. Образы пушатся на Docker Hub
4. На сервере автоматически обновляются контейнеры

---

## Автор

**Тихончук Андрей**  
- GitHub: [@andrey75rr](https://github.com/andrey75rr)  
- Telegram: [@andrey1q](https://t.me/andrey1q)
