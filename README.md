# Cпринт 16
![Yamdb workflow](https://github.com/Oorzhakau/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## CI и CD проекта API YaMDb :mag: с применением Docker и GitHub Actions.
## REST API для сервиса YaMDb — базы отзывов на произведения (фильмов, картин музыки и других сущностей).

Проект **YaMDb** собирает **отзывы (Review)** пользователей на **произведения (Titles)**. Произведения делятся на категории: «Книги», «Фильмы», «Музыка». Список категорий (*Category*) может быть расширен.

Сами произведения в **YaMDb** не хранятся, здесь нельзя посмотреть фильм или послушать музыку.

В каждой категории есть **произведения**: книги, фильмы или музыка. Например, в категории «Книги» могут быть произведения «Винни Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Насекомые» и вторая сюита Баха.

Произведению может быть присвоен **жанр (Genre)** из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»). Новые жанры может создавать только администратор.

Благодарные или возмущённые читатели оставляют к произведениям текстовые **отзывы (*Review*)** и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — **рейтинг** (целое число). На одно произведение пользователь может оставить только один отзыв.


## Техническое описание проекта

К проекту по адресу http://localhost/redoc/ подключена документация **API YaMDb**.
В ней описаны возможные запросы к API и структура ожидаемых ответов.
Для каждого запроса указаны уровни прав доступа: пользовательские роли, которым разрешён запрос.

## Технологии:
* Python 3.7
* Django 2
* Docker
* Nginx
* Github Action

##Описание Workflow
Workflow состоит из четырёх шагов:
1. Проверка кода на соответствие PEP8 и запуск тестов проекта;
2. Сборка и публикация образа на DockerHub;
3. Автоматический деплой на удаленный сервер;
4. Отправка telegram-ботом уведомления в чат.

## Установка:
1. Клонируйте репозиторий на локальную машину.
   ```https://github.com/Oorzhakau/yamdb_final.git```
2. Установите виртуальное окружение в папке проекта.
```
cd yamdb_final
python -m venv venv
```
3. Активируйте виртуальное окружение.
   ```source venv\Scripts\activate```
4. Установите зависимости.
```
python -m pip install --upgrade pip
pip install -r api_yamdb\requirements.txt
```
## Запуск проекта в контейнерах
1. Перейдите в директорию `infra/`, заполните файл .venv_example и после этого переименуйте его в .env
2. Выполните команду:
   ```docker-compose up -d --build```
3. Для остановки контейнеров из директории `infra/` выполните команду:
   ```docker-compose down -v```
4. Загрузка данных для примера из папки `infra/`
   ```docker-compose exec web python manage.py loaddata fixtures.json```

## Deploy проекта на удаленный сервер
Предварительно для автоматического деплоя необходимо подготовить сервер:
1. Установить docker: ```sudo apt install docker.io```
2. Установите docker-compose:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
3. Скопируйте файлы docker-compose.yaml и nginx/default.conf из проекта на сервер в
home/<ваш_username>/docker-compose.yaml и home/<ваш_username>/nginx/default.conf соответственно.
4. В Secrets GitHub Actions форкнутого репозитория добавить переменные окружения:
   * SSH_KEY - ssh private key для доступа к удаленному серверу
   * HOST - public id хоста
   * USER - имя user-а на удаленном сервере
   * PASSPHRASE - пароль подтверждения подключения по ssh-key
   * DOCKER_USERNAME - username на DockerHub
   * DOCKER_PASSWORD - пароль на DockerHub
   * POSTGRES_USER - имя пользователя для базы данных
   * POSTGRES_PASSWORD - пароль для подключения к базе
   * DB_ENGINE - настойка подключения django-проекта к postgresql
   * DB_NAME - имя базы данных
   * DB_HOST - название сервиса (контейнера)
   * DB_PORT - порт для подключения к БД
   * DJANGO_SU_ADMIN - имя суперюзера в django-проекте
   * DJANGO_SU_EMAIL - почта суперюзера в django-проекте
   * DJANGO_SU_PASSWORD - пароль суперюзера в django-проекте
   * TELEGRAM_TOKEN - token telegram-бота
   * TELEGRAM_TO - id пользователя, которому будут приходить оповещения
об успешном деплои

## Автор:

[Александр Ооржак](https://github.com/Oorzhakau)

