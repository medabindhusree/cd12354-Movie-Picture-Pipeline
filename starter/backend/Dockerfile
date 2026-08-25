FROM public.ecr.aws/docker/library/python:3.10-alpine

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN adduser -D -u 1000 -D app

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

COPY . /app

RUN chown -R app:app /app /var/www/app

USER app

WORKDIR /app

CMD ["uwsgi", "--http", "0.0.0.0:5000", "--master", "--enable-threads", "--module", "movies.movies_api:app"]
