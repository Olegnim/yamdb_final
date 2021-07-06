
![yamdb_final workflow](https://github.com/Olegnim/yamdb_final/workflows/yamdb_workflow/badge.svg)

[![GitHub%20Actions](https://img.shields.io/badge/-GitHub%20Actions-464646??style=flat-square&logo=GitHub%20actions)](https://github.com/features/actions)
[![GitHub](https://img.shields.io/badge/-GitHub-464646??style=flat-square&logo=GitHub)](https://github.com/)
[![docker](https://img.shields.io/badge/-Docker-464646??style=flat-square&logo=docker)](https://www.docker.com/)
[![NGINX](https://img.shields.io/badge/-NGINX-464646??style=flat-square&logo=NGINX)](https://nginx.org/ru/)
[![Python](https://img.shields.io/badge/-Python-464646??style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646??style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646??style=flat-square&logo=PostgreSQL)](https://www.postgresql.org/)



# О проекте

База отзывов о фильмах, книгах и музыке

## Технологии

Проект на Django REST framework
- Python 3.8.5
- Django 3.0.5
- Nginx
- Gunicorn 20.0.4
- PostgreSQL
- Docker

## Рабочий пример доступен по адресу

http://178.154.226.216/api/v1/

# Документация к API

http://178.154.226.216/redoc/

# Развертывание проекта YaMDB в docker. 

## Установка docker

Проверяем установлен ли docker и docker-compose

```
docker -v
docker-compose -v
```

Если не установлен ставим

```
sudo apt update && sudo apt upgrade
sudo apt install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common -y
```

Добавляем репозиторий в список репозиторев Ubuntu  

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
```

Еще раз обновляем индекс пакетов и выполняем установку и проверяем статус

```
sudo apt update
sudo apt install docker-ce docker-compose -y
sudo systemctl status docker
```

### Подготовка контейнера

Клонируем репозиторий для пользователя nameuser

```
git clone https://github.com/nameuser/infra_sp2
```

Создаем файл .env в клонированной директории

```
DB_ENGINE=django.db.backends.postgresql 
DB_NAME= введите имя базы данных
POSTGRES_USER= введите имя пользователя
POSTGRES_PASSWORD= введите пароль
DB_HOST=db 
DB_PORT=5432 
```

### Запуск контейнера

Запуск контейнера

```
docker-compose up
```

Для входа в контейнер выполните команду

```
docker-compose exec web bash
```

Выполняем миграцию базы данных

```
python manage.py migrate
```

Создаем суперпользователя

```
python manage.py createsuperuser
```

Соберием всю статику внутри докера 

```
sudo docker-compose exec web python manage.py collectstatic
```

Загрузка тестовых данных 

```
sudo docker-compose exec web python manage.py loaddata fixtures.json
```

### Доступные методы

```
api/v1/titles (GET, POST)
api/v1/titles/<title_id>/ (GET, POST, PUT, PATCH, DELETE)
api/v1/titles/<title_id>/comments/ (GET, POST)
api/v1/titles/<title_id>/comments/<comment_id> (GET, POST, PUT, PATCH, DELETE)
api/v1/genre/ (GET, POST)
api/v1/category/ (GET, POST)
```

### Выключение контейнера

```bash
docker-compose down

```

### Удаление всех Docker контейнеров

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```


#### Проект разработан командой:

Проект выполнялся в рамках учебного курса Яндекс.Практикум

* [Игорь Маркин](https://github.com/igor-markin/)
  - Роль: TL, Developer; develop: приложение User
* [Михаил Якубенко](https://github.com/vbifaa)
  - Роль: Developer; develop: приложение Api: модели, вьюхи, пермишены, сериалайзеры, эндпоинты для Category, Genre, Title
* [Олег Соколов](https://github.com/olegnim)
  - Роль: Developer; develop: приложение Api: модели, вьюхи, пермишены, сериалайзеры, эндпоинты для Review, Comment
