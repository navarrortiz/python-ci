FROM python:3.10.5-slim-bullseye

ENV PYTHONUNBUFFERED 1

RUN apt-get update && apt-get install --no-install-recommends -y build-essential libpq-dev gettext

RUN pip install --upgrade pip
RUN PIP_NO_CACHE_DIR=false pip install 'poetry' --no-cache-dir
RUN poetry config virtualenvs.create false

COPY pyproject.toml .
COPY poetry.lock .

RUN poetry install -n --no-root

COPY ./docker/local/start /start
RUN chmod +x /start

WORKDIR /app

EXPOSE 8000

ENTRYPOINT ["/start"]
