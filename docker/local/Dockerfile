FROM python:3.10.5-alpine

ENV PYTHONUNBUFFERED 1

RUN apk update \
  # psycopg2 dependencies
  && apk add --virtual build-deps gcc python3-dev musl-dev alpine-sdk libmemcached-dev \
  && apk add postgresql-dev \
  # Pillow dependencies
  && apk add jpeg-dev zlib-dev freetype-dev lcms2-dev openjpeg-dev tiff-dev tk-dev tcl-dev \
  # CFFI dependencies
  && apk add libffi-dev py-cffi \
  # Translations dependencies
  && apk add gettext \
  # https://docs.djangoproject.com/en/dev/ref/django-admin/#dbshell
  && apk add postgresql-client \
  # Rust dependencies
  && apk add openssl-dev cargo

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
