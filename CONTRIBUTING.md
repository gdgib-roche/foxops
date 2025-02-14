# Contributing

This project uses [Poetry](https://python-poetry.org/)
and [poetry-dynamic-versioning](https://pypi.org/project/poetry-dynamic-versioning/)
for dependency management and the package building process.

## Running tests

The test suite uses `pytest` as a test runner and it's located under `tests/`.

The unit tests can be executed by excluding the `e2e` tests:

```
pytest -m 'not e2e'
```

which doesn't require any external database nor GitLab instance.

To run the `e2e` tests a test GitLab instance needs to be available. It can be started using `docker compose`:

```
docker compose up -d
```

Be aware that an **initial startup of the Gitlab instance can take some time** (5 minutes). Check the logs with `docker-compose logs` to verify it if reached a stable state.

Then, the tests can be run using `pytest`:

```
pytest -m 'e2e'
```

## Running foxops locally

The foxops API can be run locally using `uvicorn`:

```
uvicorn foxops.__main__:create_app --host localhost --port 5001 --reload --factory
```

For this to work foxops needs a few configuration settings to be available.
These are at least a GitLab address and token. To use the test instance you can run the following

```
FOXOPS_STATIC_TOKEN=dummy FOXOPS_GITLAB_ADDRESS=http://localhost:5002/api/v4 FOXOPS_GITLAB_TOKEN=ACCTEST1234567890123 uvicorn foxops.__main__:create_app --host localhost --port 5001 --reload --factory
```

### Documentation

run `make live` in the `docs/` subfolder to start a web server that hosts a live-build of the documentation. It even auto-reloads in case of changes!

## Changing the database schema

This project uses [Alembic](https://alembic.sqlalchemy.org/en/latest/) for database migrations. If you change the database schema, you need to create a new migration script that contains steps to migrate existing databases to the new schema.

To create a new migration script, run the following command (adjusting the message):

```
poetry run alembic revision --autogenerate -m "Add a new table"
```
