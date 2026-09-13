FROM python:3.10-alpine

WORKDIR /

RUN adduser -D -u 1000 -g app app

RUN apk update && \
    apk add \
    pcre \
    pcre-dev \
    build-base \
    gcc \
    linux-headers \
    openssl \
    libffi-dev \
    && pip install pipenv

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app

RUN cd /app && pipenv install --system --deploy

RUN pip install --no-cache-dir uwsgi

COPY . /app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --chdir /app --module __init__:app