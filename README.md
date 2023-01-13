# Doсker + Yii2 + MySql + phpMyAdmin

Для начала необходимо клонировать репозиторий

[https://github.com/yiisoft/yii2-docker](https://github.com/yiisoft/yii2-docker)

После успешного клонирования переходим в папку `yii2-docker`

```bash
cd yii2-docker
```

В директории необходимо открыть файл `.env` с помощью редактора или используя редактор в консоли

```bash
nano .env
```

и поменять версию PHP на 8.1

```bash
HP_BASE_IMAGE_VERSION=8.1-apache
```

Для добавления mysql и phpMyAdmin добавить следующий код в `docker-compose.yml`

```bash
db:
    image: mysql:8.0
    restart: always
    environment:
      - MYSQL_DATABASE=demo_project
      - MYSQL_USER=demo_user
      - MYSQL_PASSWORD=123456
      - MYSQL_ROOT_PASSWORD=123456
    ports:
      - '3306:3306'
    expose:
      - '3306'
    volumes:
      - "./docker/mysql:/var/lib/mysql"
    networks:
      - default
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - '8888:80'
    environment:
      - PMA_ARBITRARY=1
      - PMA_HOST=db
    depends_on:
      - db
```

Таким образом, полный файл `docker-compose.yml` будет выглядеть так:

```bash
version: '2'
services:
  php:
    image: yiisoftware/yii2-php:8.1-apache
    volumes:
      - ~/.composer-docker/cache:/root/.composer/cache:delegated
      - ./:/app:delegated
    ports:
      - '8000:80'
  db:
    image: mysql:8.0
    restart: always
    environment:
      - MYSQL_DATABASE=demo_project
      - MYSQL_USER=demo_user
      - MYSQL_PASSWORD=123456
      - MYSQL_ROOT_PASSWORD=123456
    ports:
      - '3306:3306'
    expose:
      - '3306'
    volumes:
      - "./docker/mysql:/var/lib/mysql"
    networks:
      - default
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - '8888:80'
    environment:
      - PMA_ARBITRARY=1
      - PMA_HOST=db
    depends_on:
      - db
```

Запустить Docker если он не запущен и выполнить команду

```bash
docker-compose build
```

После чего будут установлены контейнеры

После необходимо установить зависимости командой

```bash
docker-compose run --rm php composer update --prefer-dist
```

И запустить скрипты (postInstall)

```bash
docker-compose run --rm php composer install    
```

После этого запускаем контейнеры

```bash
docker-compose up -d
```

В результате сайт будет доступен по адресу [http://localhost:8000/](http://localhost:8000/)


А phpMyAdmin по адресу [http://localhost:8888/](http://localhost:8888/)
