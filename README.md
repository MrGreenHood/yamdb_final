[![Yamdb-app workflow](https://github.com/MrGreenHood/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/MrGreenHood/yamdb_final/actions/workflows/yamdb_workflow.yml)

# Проект YaMDb

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title).

Произведения делятся на категории: «Книги», «Фильмы», «Музыка».

Список категорий (Category) может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть произведения: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха. Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор. Благодарные или возмущённые читатели оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг (оценку в диапазоне от одного до десяти). Из множества оценок автоматически высчитывается средняя оценка произведения.

Документация доступна по ссылке:
- http://51.250.12.251/redoc/

### Ресурсы API YaMDb

_USERS_: пользователи.

_TITLES_: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).

_CATEGORIES_: категории (типы) произведений («Фильмы», «Книги», «Музыка»).

_GENRES_: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.

_REVIEWS_: отзывы на произведения. Отзыв привязан к определённому произведению.

_COMMENTS_: комментарии к отзывам. Комментарий привязан к определённому отзыву.

### Алгоритм регистрации пользователей

- Пользователь отправляет POST-запрос с параметрами username и email на /api/v1/auth/email/

- Yamdb отправляет письмо с кодом подтверждения на введенный адрес email. 
  
- Пользователь отправляет POST-запрос с параметрами username и confirmation_code (полученный по почте) на /api/v1/auth/token/

- В ответ на запрос ему приходит token (JWT-токен). Эти операции выполняются один раз, при регистрации пользователя.
  
- В результате пользователь получает токен и может работать с API, отправляя этот токен с каждым запросом.

Для обновления токена, нужно отправить повторный запрос на /api/v1/auth/email/ со своими username и email

### Пользовательские роли:

_Аноним_ — может просматривать описания произведений, читать отзывы и комментарии.

_Аутентифицированный пользователь_ (user) — может читать всё, как и Аноним, дополнительно может публиковать отзывы и ставить рейтинг произведениям (фильмам/книгам/песенкам), может комментировать чужие отзывы и ставить им оценки; может редактировать и удалять свои отзывы и комментарии.

_Модератор_ (moderator) — те же права, что и у Аутентифицированного пользователя плюс право удалять и редактировать любые отзывы и комментарии.

_Администратор_ (admin) — полные права на управление проектом и всем его содержимым. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.

_Cуперюзер(superuser)_ — те же права, что и у Администратора кроме того что даже если ему поменять роль на _user_ он сохранит права администратора

### Шаблон наполнения env-файла
- POSTGRES_USER = <POSTGRES_USER>
- POSTGRES_PASSWORD = <POSTGRES_PASSWORD>
- DB_HOST = <DB_HOST>
- DB_PORT = <DB_PORT>
- EMAIL = <example@gmail.com>
- PASSWORD = <PASSWORD>
- SECRET_KEY = <SECRET_KEY>
- ALLOWED_HOSTS = <HOST1 HOST2 ...>

### Как установить: 

Склонируйте репозиторий:
-  ```git clone https://github.com/MrGreenHood/yamdb_final.git```

Соберите контейнеры и запустите их:
- ```docker-compose up -d --build```

Запуск контейнеров:
- ```docker-compose up```

Выполните миграции:
- ```docker-compose exec web python manage.py migrate --noinput```

Создайте суперпользователя:
- ```docker-compose exec web python manage.py createsuperuser```

Сборка файлов статики:
- ```docker-compose exec web python manage.py collectstatic --no-input```

Остановка контейнеров:
- ```docker-compose down```

Создание дампа (резервной копии) базы, используя команду:
- ```docker-compose exec web python manage.py dumpdata > fixtures.json```

Команда для заполнения базы данными:
- ```docker-compose exec web python manage.py loaddata fixtures.json```


### Технологии
- Python 3.7
- Django 2.2.19
- djangorestframework
- djangorestframework-simplejwt
- PyJWT
- PostgreSQL
- gunicorn
- nginx
### Авторы
Иван

### Ссылка на сайт
- http://51.250.12.251/
### Ссылка на админку
- http://51.250.12.251/admin/