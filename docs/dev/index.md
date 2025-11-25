# Developer Setup
As shown in the vat overview our system consists of different tools that are exposed via the central vat api.

TODO: show overview image here

To run everything locally you need to set the environment variables:

List of environment variables that are needed for all systems
```
# postgres connection parameters
VAT_POSTGRES_DB
VAT_POSTGRES_USER
VAT_POSTGRES_PASS
VAT_POSTGRES_HOST
VAT_POSTGRES_PORT

# API parameter 
VAT_API_URL
VAT_API_TOKEN

# LogCrawler parameter
VAT_LOG_ROOT

# Labelstudio parameter
VAT_LABELSTUDIO_API_URL
VAT_LABELSTUDIO_TOKEN

# MlFlow
```

## Dev Guidelines
- always use django-debug-toolbar when changing something in the admin dashboards to check that there are no excessive queries being done