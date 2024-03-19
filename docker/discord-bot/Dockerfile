# Install git stage
FROM alpine:3.14 as git

RUN apk update && apk add --no-cache git

# Base stage
FROM python:3.12-alpine as base

WORKDIR /discord-bot

COPY docker/discord-bot/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

WORKDIR /discord-bot

COPY discord-bot/app ./app

# Test stage
FROM base as test

WORKDIR /discord-bot

COPY discord-bot/pyproject.toml .

RUN pip install --no-cache-dir black

RUN pip install --no-cache-dir isort

RUN pip install --no-cache-dir mypy

RUN pip install --no-cache-dir pylint

# Development stage
FROM test as development

COPY --from=git /usr/ /usr/

COPY .git /discord-bot/.git

WORKDIR /discord-bot

COPY .vscode/cspell.json .
