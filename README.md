# pinot-demo
School project - Simple Pinot demo with Superset

## How to run (Docker)
### Docker compose and config
```bash
docker compose --project-name pinot-demo up -d
```
```bash
docker exec -it superset bash
cd pythonpath/
touch superset_config.py
```
#### superset_config.py
```python
# Superset specific config
ROW_LIMIT = 5000

SUPERSET_WEBSERVER_PORT = 8088

# Flask App Builder configuration
# Your App secret key will be used for securely signing the session cookie
# and encrypting sensitive information on the database
# Make sure you are changing this key for your deployment with a strong key.
# You can generate a strong key using `openssl rand -base64 42`.
# Alternatively you can set it with `SUPERSET_SECRET_KEY` environment variable.
SECRET_KEY = 'YOUR_OWN_RANDOM_GENERATED_SECRET_KEY'

# The SQLAlchemy connection string to your database backend
# This connection defines the path to the database that stores your
# superset metadata (slices, connections, tables, dashboards, ...).
# Note that the connection information to connect to the datasources
# you want to explore are managed directly in the web UI
# SQLALCHEMY_DATABASE_URI = 'sqlite:////path/to/superset.db'

# Flask-WTF flag for CSRF
WTF_CSRF_ENABLED = True
# Add endpoints that need to be exempt from CSRF protection
WTF_CSRF_EXEMPT_LIST = []
# A CSRF token that expires in 1 year
WTF_CSRF_TIME_LIMIT = 60 * 60 * 24 * 365

# Set this API key to enable Mapbox visualizations
MAPBOX_API_KEY = ''
```
>### 2.1. (First time) Setup Admin account by running below command and follow instructions to set password.
```bash
docker exec -it superset superset fab create-admin \
               --username admin \
               --firstname Superset \
               --lastname Admin \
               --email admin@superset.com \
               --password admin
```
>### 2.2. (First time) DB upgrade and Initialize Superset
```bash
docker exec -it superset superset db upgrade
docker exec -it superset superset init
```
>### 3. Import Pre-defined Pinot Datasources and Dashboard
```bash
docker exec \
    -t superset \
    bash -c 'superset import_datasources -p /etc/examples/pinot/pinot_example_datasource_quickstart.yaml && \
             superset import_dashboards -p /etc/examples/pinot/pinot_example_dashboard.json'
```
>### 4. Go to SuperSet UI: http://localhost:8088/ to play around with dashboard.

Sources:
https://docs.pinot.apache.org/basics/getting-started/running-pinot-in-docker
https://docs.pinot.apache.org/integrations/superset