HAS_MACHINE := $(shell [ -e /Applications/Docker.app/Contents/Resources/bin/docker ] && echo "0" || (hash docker-machine &>/dev/null && echo "1" || echo "0"))
ifeq ($(HAS_MACHINE), 1)
  IP:=$(shell docker-machine env docker-machine-dev >/dev/null 2>&1 && docker-machine ip docker-machine-dev || echo "")
else
  IP:=localhost
endif

ifeq ("$(shell uname)", "Darwin")
  IS_LINUX:=0
  OPEN=open
else
  IS_LINUX:=1
  OPEN=xdg-open
endif

.PHONY: help

help:
  @echo "General:"
  @echo
  @echo "  make help              Prints this help."
  @echo
  @echo "  make open              Open www in your browser."
  @echo "  make open-phpmyadmin   Open phpmyadmin in your browser."
  @echo
  @echo "Docker:"
  @echo
  @echo "  make build             Build the docker containers."
  @echo "  make serve             Start async the docker containers."
  @echo "  make start             Start the docker containers."
  @echo "  make down              Down the docker containers."
  @echo "  make stop              Stop the docker containers."
  @echo "  make status            Status the docker containers."
  @echo "  make restart           Restart the docker containers."
  @echo
  @echo "  make shell             Shell in the docker containers."
  @echo "  make migrate           Migration in the docker containers."
  @echo "  make rollback          Migration rollback in the docker containers."
  @echo "  make routes            Check Laravel routing."
  @echo "  make composer-install  Composer install in the docker containers."
  @echo "  make composer          Command for composer. ex:) make composer CM='migrate'"
  @echo "  make artisan           Command for Laravel artisan. ex:) make artisan CM='migrate'"
  @echo "  make npm-install       npm install in the docker containers."
  @echo "  make npm-dev           Build npm files to pulic directory."
  @echo "  make npm-watch         Build assets files and watch modify."
  @echo
  @echo "  make setup             Setup for laravel project."
  @echo


.PHONY: open open-phpmyadmin

open:
  $(OPEN) http://$(IP):$(shell docker-compose port nginx 80 | cut -d':' -f2)

open-phpmyadmin:
  $(OPEN) http://$(IP):$(shell docker-compose port phpmyadmin 80 | cut -d':' -f2)

.PHONY: build serve start down stop status restart setup

build:
  @docker-compose build

serve:
  @docker-compose up

start:
  @docker-compose up -d

down:
  @docker-compose down

stop:
  @docker-compose stop

status:
  @docker-compose ps

restart: stop start

.PHONY: shell composer-install npm-install migrate rollback seed routes composer composer-autoload artisan npm-dev npm-watch

shell:
  @docker-compose exec fpm /bin/ash

migrate:
  @docker-compose exec fpm php ./artisan migrate

rollback:
  @docker-compose exec fpm php ./artisan migrate:rollback

seed:
  @docker-compose exec fpm php ./artisan db:seed

routes:
  @docker-compose run --rm fpm php ./artisan route:list

composer-install:
  @docker-compose run --rm composer install

composer:
  @docker-compose run --rm composer ${CM}

composer-autoload:
  @docker-compose run --rm composer dump-autoload

artisan:
  @docker-compose exec fpm php ./artisan ${CM}

artisan-key-generate:
  @docker-compose exec fpm php ./artisan key:generate

artisan-storage-link:
  @docker-compose exec fpm php ./artisan storage:link

npm-install:
  @docker-compose run --rm npm install

npm-dev:
  @docker-compose run --rm npm run dev

npm-watch:
  @docker-compose run --rm npm run watch

.PHONY: copy-env setup

copy-env:
  cp .env.docker .env

setup: copy-env build start composer-install npm-install artisan-key-generate artisan-storage-link migrate seed open

.PHONY: phpcs docker-phpcs

phpcs:
  php ./vendor/bin/phpcs -p -s --colors --report-full --report-summary --standard=./phpcs.xml ./app

docker-phpcs:
  @docker-compose exec fpm php ./vendor/bin/phpcs -p -s --colors --report-full --report-summary --standard=./phpcs.xml ./app