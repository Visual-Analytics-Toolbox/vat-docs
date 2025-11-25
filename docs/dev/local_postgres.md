# Developer Setup localy

Install postgres 17
https://dev.to/johndotowl/postgresql-17-installation-on-ubuntu-2404-5bfi    

TODO: how can I use postgres in docker for this?

```
docker run --name local-postgres17 \
-e POSTGRES_PASSWORD=$VAT_POSTGRES_PASS \
-e POSTGRES_USER=$VAT_POSTGRES_USER \
-e POSTGRES_DB=$VAT_POSTGRES_DB \
-p 4000:5432 \
-v pgdata:/var/lib/postgresql/data \
-d postgres:17
```