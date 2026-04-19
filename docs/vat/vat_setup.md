# Developer Setup
The source code for all the tools under the Visual Analytics banner can be found on github at https://github.com/orgs/Visual-Analytics-Toolbox/repositories

The core part is the django webserver and the postgres database.

## Setup direnv
In order to setup everything correctly you need to set up a bunch of environment variables. We recommend to use [direnv](https://direnv.net/). However this is optional.

```bash
sudo apt install direnv
```

Add this to the .bashrc file
```bash
eval "$(direnv hook bash)"
```
you need to run `direnv allow` in every folder you have an .envrc file after every change before you can use the variables defined there.

You need to set the variables for setting up the visual analytics tool:
```bash
export VAT_POSTGRES_DB=
export VAT_POSTGRES_USER=
export VAT_POSTGRES_PASS=
export VAT_POSTGRES_HOST=
export VAT_POSTGRES_PORT=

```


## Setup Postgres locally

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

To cleanup you can remove the container and volume and start new.

### Setup Visual Analytics Tool
Install all the dependencies with:
```bash
uv sync
```
setup the db and run django migrations
```bash
# set postgres vars to the env vars you set up earlier (only needed for this script)
export PGHOST=$VAT_POSTGRES_HOST
export PGPORT=$VAT_POSTGRES_PORT      
export PGUSER=$VAT_POSTGRES_USER 
export PGPASSWORD=$VAT_POSTGRES_PASS

cd utils
./db_reset.sh
```

inside the django folder run:
```
python manage.py createsuperuser
```
and then
```
python manage.py runserver
```