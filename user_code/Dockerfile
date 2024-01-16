FROM python:3.10-slim

# Checkout and install dagster libraries needed to run the gRPC server
# exposing your repository to dagster-webserver and dagster-daemon, and to load the DagsterInstance

RUN pip install \
    dagster \
    dagster-postgres \
    dagster_dbt \
    dbt-duckdb

# Add repository code
WORKDIR /opt/dagster/app

ENV DAGSTER_DBT_PARSE_PROJECT_ON_LOAD=1

COPY src/ /opt/dagster/app/src