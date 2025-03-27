# taski-docker


## Развертывание
1. Создать том для БД и статики
2. Скачать образ с postgres
3. Запуск контейнера с postgres

'''sh
docker volume create pg_data
docker pull postgres:13.10
docker run --name db --env-file .env -v pg_data:/var/lib/postgresql/data postgres:13.10
'''

## Разное
Запуск psql для работы с Postgres

docker exec -it db psql -U django_user -d django 