FROM python:3.13.2-slim-bookworm

LABEL maintainer="developers@agentsupport.com"

ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1

RUN apt-get update \
  # dependencies for building Python && pgpt packages
  && apt-get install -y build-essential libpq-dev gettext git \
  # dependencies for building Python packages
  && apt-get install -y python3-setuptools python3-wheel \
  # upgrade local packages on container
  && apt-get dist-upgrade -y \
  # upgrade setuptools to ensure pkg_resources is available
  && pip install --upgrade setuptools \
  # cleaning up unused files
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && rm -rf /var/lib/apt/lists/*

RUN pip install poetry-core poetry

COPY ./docker/local/pgpt/entrypoint.sh /entrypoint.sh
RUN sed -i 's/\r$//g' /entrypoint.sh
RUN chmod +x /entrypoint.sh

COPY ./docker/local/pgpt/start.sh /start.sh
RUN sed -i 's/\r$//g' /start.sh
RUN chmod +x /start.sh

WORKDIR /app

ENTRYPOINT ["/entrypoint.sh"]
