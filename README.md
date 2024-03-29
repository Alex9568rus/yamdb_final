# CI и CD проекта api_yamdb

![workflow](https://github.com/Alex9568rus/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Описание проекта

Настроика Continuous Integration и Continuous Deployment для приложения [YaMDB](https://github.com/Alex9568rus/api_yamdb), которая включает в себя:

* автоматический запуск тестов;
* обновление образов на Docker Hub;
* автоматический деплой на боевой сервер при пуше в главную ветку main.

Сам проект [YaMDB](https://github.com/Alex9568rus/api_yamdb)  представляет собой сервис по сбору отзывов (Reviews) пользователей на произведения (Titles). Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (Categories) может быть расширен Администратором.

Произведению может быть присвоен жанр (Genre) из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только Администратор.

Пользователи оставляют к произведениям текстовые отзывы (Review) и ставят произведению оценку (Score) в диапазоне от 1 до 10; из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (Raiting). На одно произведение пользователь может оставить только один отзыв.

Для авторизации пользователей используется код подтверждения высылаемый на email. Для аутентификации пользователей используются JWT-токены.

---

## Технологии

* Django
* Django Rest Framework
* Nginx
* Postgresql
* Docker
* Docker compose
* Яндекс.Облако

---

## Запуск проекта

Для запуска проекта вам понадобится:
* зарегистрироваться на DockerHub;
* создать сервер (например, на Яндекс.Облаке).

Настройка удаленного сервера будет описана далее.

1. ___Клонировать репозиторий:___

` git clone git@github.com:Alex9568rus/yamdb_final.git`

2. ___Добавить секретные данные:___

Для добавления секретной информации нужно зайти в склонированный репозиторий -> раздел settings -> Secrets -> Actions
и добавить поочередно следующие переменные:
| Название переменной | Значение |
|:----:|:----:|
|SECRET_KEY| SECRET_KEY из settings.py|
|YOUR_TELEGRAM_ID|ID чата, в который будут приходить уведомления|
|TELEGRAM_BOT_TOKEN|Токен телеграм-бота|
|USERNAME|Ваш логин на DockerHub|
|PASSWORD|Ваш пароль от DockerHub|
|HOST|Публичный IP сервера на Яндекс.Облаке|
|USER|Ваш логин на Яндекс.Облаке|
|SSH_KEY|Приватный ключ с локальной машины|
|PASSPHRASE|Пароль, указанный при создании ключей|
|DB_ENGINE|В проекте использовался postgresql|
|DB_NAME|Имя вашей базы данных|
|POSTGRES_USER|Ваш логин для подключения к базе данных|
|POSTGRES_PASSWORD|Пароль для подключения к БД|
|DB_HOST|Название сервиса (контейнера)|
|DB_PORT|Порт для подключения к БД |

3. ___Настройка сервера:___

* На боевом сервере установите docker и docker-compose
* Остановите службу nginx
* Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер в home/<ваш_username>/docker-compose.yaml и home/<ваш_username>/nginx/default.conf соответственно

4. ___Deploy:___

При git pus запускается скрипт GitHub actions который выполняет автоматический deploy на сервер.

После успешного деплоя необходимо на сервере выполнить следующие дествия:
* ` sudo docker-compose exec web python manage.py migrate` - сделать миграции;
* ` sudo docker-compose exec web python manage.py createsuperuser` - создать суперюзера;
* ` sudo docker-compose exec web python manage.py collectstatic --no-input` - сабрать статику

Чтобы создать резервную копию базы данных воспользуйтесь командой:

` sudo docker-compose exec web python manage.py dumpdata > fixtures.json`

Документация доступна по адресу: 
* _http://ваш_HOST/redoc/_ (только после запуска проекта )

---

## Автор
___Жбаков А.___

_https://github.com/Alex9568rus_
