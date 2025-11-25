# Developer Setup localy

Install postgres 17
https://dev.to/johndotowl/postgresql-17-installation-on-ubuntu-2404-5bfi    

TODO: how can I use postgres in docker for this?

```
docker run --name local-postgres17 \
-e POSTGRES_PASSWORD= \
-e POSTGRES_USER= \
-e POSTGRES_DB= \
-p 4000:5432 \
-v pgdata:/var/lib/postgresql/data \
-d postgres:17
```