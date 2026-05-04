# Web-application infra template

[English](#english-version) | [Русский](#russian-version)

<a name="english-version"></a>

## English

### Getting Started

1) Create a single root directory for all services and infrastructure.

2) Clone all service repositories into that directory.

3) Adjust paths: 
    
    1) In `docker-compose.yml`: update `build: context` and `volumes`.
    2) *Note: k3s and k8s support will be added in future updates.*

4) Follow the launch instructions below.

### Working Rules

0) Recommended repository name: `PROJECT_NAME-infra`.

1) Do not use the `main` branch unless you are deploying to production.

2) Always ensure you are working with up-to-date code (this applies to all service repositories as well).

3) Always update the code before starting work:

```bash
make checkout-all BRANCH=branch_name
cd ../service-folder-name
git pull
cd  ../infra-repository-folder
```

4) Use the `develop` branch for new features.

6) To avoid merge conflicts, only work on your assigned files.

7) Mark task completion in your task tracker.

8) Contact your Team Lead immediately if you have any questions.

### Launching

1) Update the service code.

2) Copy the required `.env` file and configure the variables:
    1) Use `.env.dev` for the development version.
    2) Use `.env.prod` for production only. The `main` branch is for production only.

3) Start the project:

    1) For Production (via docker-compose):

    ```bash
    make up ENV=prod PLATFORM=docker-compose
    ```

    2) For Development (via docker-compose):

    ```bash
    make up ENV=dev PLATFORM=docker-compose
    ```

    > ❗ In the dev version, the Frontend is launched separately via `npm run dev`.

4) All routes are recommended to be built from `localhost` (especially in docker-compose setup).

    1) **Dev Version**
    
    | Service           | URL                                                            |
    |-------------------|----------------------------------------------------------------|
    | Frontend          | http://localhost:3000                                          |
    | Backend - auth v0 | http://localhost/api/auth/v0/                                  |
    | PSQL Database     | postgres://postgres:PASSWORD@db:5432/your-proj-name?sslmode=disable |

    2) **Prod Version (Server-side)**

    | Service           | URL                                                            |
    |-------------------|----------------------------------------------------------------|
    | Frontend          | https://localhost                                              |
    | Backend - auth v0 | https://localhost/api/auth/v0                                  |
    | PSQL Database     | postgres://postgres:PASSWORD@db:5432/your-proj-name?sslmode=require |

    3) **Prod Version (User View)**

    | Service           | URL                                      |
    |-------------------|------------------------------------------|
    | Frontend          | https://your-proj-name.com/              |
    | Backend - auth v0 | https://your-proj-name.com/api/auth/v0/  |

### Docker Compose Commands

> ❗ Always specify the `ENV` variable (`dev` or `prod`) and the `PLATFORM` (defaults to `docker-compose`).

1) Rebuild container(s). Specify `CONTAINERS` variable for specific ones, or omit it to rebuild all.

    1) Rebuild all containers:

    ```bash
    make rebuild ENV=dev PLATFORM=docker-compose
    ```

    2) Rebuild specific containers (e.g., auth and frontend):

    ```bash
    make rebuild CONTAINERS="auth frontend" ENV=prod PLATFORM=docker-compose
    ```

2) Stop containers:

    ```bash
    make stop ENV=dev PLATFORM=docker-compose
    ```

3) Show logs (containers run in detached mode by default):

    ```bash
    make logs ENV=dev PLATFORM=docker-compose
    ```

4) Clean up containers:

    ```bash
    make clean ENV=dev PLATFORM=docker-compose
    ```

### Git Commands

These commands simplify working with multiple repositories without manually entering each folder.

1) Switch all services to the same branch (e.g., `develop`):

```bash
make checkout-all BRANCH=develop
```

<a name="russian-version"></a>

## Русский

### Начало работы

1) Создайте одну папку для всех сервисов и инфраструктуры.

2) Склонируйте туда все репозитории сервисов. 

3) Поменяйте пути: 
    
    1) в docker-compose - для build: context и volumes.
    2) Поддержка k3s и k8s будет в будущих обновлениях.

4) Запускайте согласно инструкции

### Правила работы

0) Рекомендованное название репозитория - НАЗВАНИЕ_ПРОЕКТА-infra.

1) Не используте ветку main если не выкладываете на продакшен.

2) Всегда убеждайтесь что работаете с актуальным кодом (касается кода всех 
сервисов в том числе).

3) Всегда обновляйте код перед работой:

```bash
make checkout-all BRANCH=название_ветки
cd ../название-папки-сервиса-с-кодом
git pull
cd  ../название-папки-с-кодом-инфраструктуры
```

4) Для фич рекомендовано использовать ветку `develop`.

6) Во избежание лишних конфликтов работайте только в своих файлах.

7) Отметьте выполнение задачи в вашем таск-треккере.

8) Всегда сразу пишите вопросы вашему тимлиду.

### Запуск

1) Обновите код сервисов.

2) Скопируйте нужный .env файл и настройте переменные
    1) Для dev версии используйте `.env.dev`.
    2) Для продакшена только `.env.prod`. На main только prod (так как эта прод ветка)

3) Запустите проект

    1) Для прода (с docker-compose)

    ```bash
    make up ENV=prod PLATFORM=docker-compose
    ```

    2) Для dev версии (с docker-compose)

    ```bash
    make up ENV=dev PLATFORM=docker-compose
    ```

    > ❗ Фронт запускается отдельно в dev версии, через `nmp run dev`

4) Все роуты рекомендуется строить от localhost (особенно в сетапе с docker-compose)

    1) dev версия
    
    | За что отвечает   | url                                                            |
    |-------------------|----------------------------------------------------------------|
    | Frontend          | http://localhost:3000                                          |
    | Backend - auth v0 | http://localhost/api/auth/v0/                                  |
    | БД PSQL           | postgres://postgres:PASSWORD@db:5432/your-proj-name?sslmode=disable |

    2) prod версия (с сервера)

    | За что отвечает   | url                                                            |
    |-------------------|----------------------------------------------------------------|
    | Frontend          | https://localhost                                              |
    | Backend - auth v0 | https://localhost/api/auth/v0                                  |
    | БД PSQL           | postgres://postgres:PASSWORD@db:5432/your-proj-name?sslmode=require |

    3) prod версия (как видит пользователь)

    | За что отвечает   | url                              |
    |-------------------|----------------------------------|
    | Frontend          | https://your-proj-name.com/               |
    | Backend - auth v0 | https://your-proj-name.com/api/auth/v0/   |

### Команды для docker compose

> ❗ Всегда указывайте переменную ENV со значением dev или prod и уточняйте PLATFORM (по дефолту docker-compose)

1) Заребилдить контейнер(-ы) (указать в переменной CONTAINERS) или все если без переменной

    1) Заребилдить все контейнеры (просто не указывая параметр CONTAINERS)

    ```bash
    make rebuild ENV=dev PLATFORM=docker-compose
    ```

    2) Заребилдить определённые контейнеры (в примере auth и frontend)

    ```bash
    make rebuild CONTAINERS=auth frontend ENV=prod PLATFORM=docker-compose
    ```

    2) Остановить контейнеры PLATFORM=docker-compose

    ```bash
    make stop ENV=dev PLATFORM=docker-compose
    ```

    3) Показать логи (изначально запуск в режиме detached)

    ```bash
    make logs ENV=dev PLATFORM=docker-compose
    ```

    4) Очистить контейнеры

    ```bash
    make clean ENV=dev PLATFORM=docker-compose
    ```

### Команды для git

Эти команды позволяют упростить работу с git и позволяет выполнять действия с git
не входя в папки других сервисов

1) Переключится на всех сервисах на одну и ту же ветку (в данном случае на develop)

```bash
make checkout-all BRANCH=develop
```

> From [Niyaz Gimadiev](https://rovno.dev/u/niyazgim) with ❤️