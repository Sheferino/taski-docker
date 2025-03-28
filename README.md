# taski-docker

## Создание

docker build -t sheferino/taski_frontend frontend/
docker build -t sheferino/taski_backend backend/
docker build -t sheferino/taski_gateway gateway/ 
docker push sheferino/taski_frontend && docker push sheferino/taski_backend && docker push sheferino/taski_gateway

## Развертывание
1. Создать том для БД и статики
2. Скачать образ с postgres
3. Запуск контейнера с postgres
Создание сети
Подключение контейнера с postgres к сети
Создание и запуск контейнера с backend
Миграции в backend

'''sh
docker volume create pg_data
docker pull postgres:13.10
docker run --name db --env-file .env -v pg_data:/var/lib/postgresql/data postgres:13.10
docker run --env-file .env --net django-network --name taski_backend_container -p 8000:8000 sheferino/taski_backend
docker exec taski_backend_container python manage.py migrate
'''

## Запуск

### отладочный
docker compose up
docker compose exec backend python manage.py collectstatic
docker compose exec backend cp -r /app/collected_static/. /backend_static/static/ 

### Боевой

docker compose -f docker-compose.production.yml up
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend sh -c "cp -r /app/collected_static/. /backend_static/sta
tic/"
docker compose -f docker-compose.production.yml exec backend python manage.py migrate

## Разное
Запуск psql для работы с Postgres

docker exec -it db psql -U django_user -d django 

Запуск консоли контейнера

docker compose exec -it backend sh