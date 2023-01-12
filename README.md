# Проект api_yamdb
### Описание
Проект YaMDb собирает отзывы пользователей на произведения. 

_Благодаря этому проекту можно:_
- оставлять к произведениям текстовые отзывы 
- ставить произведению оценку
- комментировать отзывы других пользователей
- получить список всех произведений, категорий, жанров, комментариев, отзывов

### Используемые технологии

- [Python 3.7 ](https://www.python.org/downloads/release/python-379/)
- [Django REST framework 3.12](https://www.django-rest-framework.org/community/3.12-announcement/)
- [Simple JWT-аутентификация с реализацией через код подтверждения](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/)
- [Docker](https://docs.docker.com/engine/reference/builder/#from)
- [Gunicorn](https://docs.gunicorn.org/en/stable/)
- [nginx](https://nginx.org/en/docs/)
- [PostgreSQL](https://postgrespro.ru/docs/postgresql/12/)
- [GIT](https://git-scm.com/docs/git)


### Установка
Для развертывания проекта используется виртуальная среда Docker и
Docker-compose. Docker "упаковывает" приложение и все его зависимости в
контейнер и может быть перенесен при использовании на месте ОС Linux. Docker-compose это инструмент для запуска многоконтейнерных приложений.

#### Клонируем и разворачиваем репозиторий
```git clone git@github.com:Olesyacur/infra_sp2.git```

#### Создаем в директории infra/ файл .env по шаблону
DB_ENGINE= # указываем базу данных, с которой будем работать
DB_NAME= # имя базы данных
POSTGRES_USER= # логин для подключения к базе данных
POSTGRES_PASSWORD= # пароль для подключения к БД (установите свой)
DB_HOST= # название сервиса (контейнера)
DB_PORT= # порт для подключения к БД
#### Запускаем приложение в контейнерах
- Переходим в папку с файлом docker-compose.yaml:
```cd infra/```
- Собираем контейнеры (infra_db_1, infra_web_1, infra_nginx_1):
```docker-compose up -d --build```
- Выполняем миграции
```docker-compose exec web python manage.py migrate```
- Создаем суперпользователя
```docker-compose exec web python manage.py createsuperuser```
- Собираем статику со всего проекта
```docker-compose exec web python manage.py collectstatic --no-input```
#### Для заполнения тестовыми данными можно выполнить команду из директории api_yamdb 
```docker-compose exec web python manage.py loaddata fixtures.json```

Теперь проект доступен по адресу http://localhost/admin. Можно войти под
логином и паролем суперпользователя и наполнять базу данных.

#### Другие возможности
Для создания дампа данных из БД
```docker-compose exec web python manage.py dumpdata > dump.json```
Просмотр запущенных контейнеров
```docker stats```
Остановка и удаление контейнеров, томов, образов
```docker-compose down -v```

### Примеры запросов и результат
```
GET http://127.0.0.1:8000/api/v1/categories/
[
    {
        "count": 3,
        "next": null,
        "previous": null,
        "results": [
            {
                "name": "Книга",
                "slug": "book"
            },
            {
                "name": "Музыка",
                "slug": "music"
            },
            {
                "name": "Фильм",
                "slug": "movie"
            }
        ]
    }
]
```
```
GET http://127.0.0.1:8000/api/v1/genres/?search=Rock
[
    {
        "count": 1,
        "next": null,
        "previous": null,
        "results": [
            {
                "name": "Rock-n-roll",
                "slug": "rock-n-roll"
            }
        ]
    }
]
```
---
### Авторы
Студенты Я.Практикум - _Олеся Чурсина,_ _Денис Костив,_ _Юлия Орлова_

