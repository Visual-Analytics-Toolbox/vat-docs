# Developer Setup localy
Django needs a postgres database to function. You can simply run this on your machine and the default development settings of django can communicate with the docker based postgres.

Set the env vars:
```bash
export VAT_POSTGRES_PASS=""
export VAT_POSTGRES_USER=""
export VAT_POSTGRES_DB=""
```

```bash
docker run --name local-postgres17 --restart always \
-e POSTGRES_PASSWORD=$VAT_POSTGRES_PASS \
-e POSTGRES_USER=$VAT_POSTGRES_USER \
-e POSTGRES_DB=$VAT_POSTGRES_DB \
-e POSTGRES_HOST_AUTH_METHOD=trust \
-p 4000:5432 \
-v pgdata:/var/lib/postgresql/data \
-d postgres:17 
```