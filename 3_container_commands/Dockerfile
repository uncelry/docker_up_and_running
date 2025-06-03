FROM python:3.12-slim AS builder

ENV UV_PYTHON_DOWNLOADS=never \
    UV_COMPILE_BYTECODE=1 \
    UV_PROJECT_ENVIRONMENT=/venv \
    UV_LINK_MODE=copy

WORKDIR /app

COPY --link --from=ghcr.io/astral-sh/uv:0.7.8 /uv /usr/local/bin/uv

RUN --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev --no-install-project
COPY . /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-dev

FROM python:3.12-slim

WORKDIR /app

COPY --from=builder /app /app
COPY --from=builder /venv /venv

ENV PATH="/venv/bin:$PATH"

CMD ["fastapi", "dev", "main.py", "--host", "0.0.0.0", "--port", "80"]

# 1. проверить появится ли /root/.cache/uv если убрать --mount=type=cache -> нет, не появится; следовательно при использовании маунта кэша делаем итоговый образ меньше
# 2. проверить как работает конструкция с двойным uv sync -> если зависимости не изменялись, то весь слой с ними (первый uv sync) будет браться из кэша. При выполнении второго uv sync, uv уже знает, что зависимости актуальны и не проверяет их вообще - только развертывает проект
# 3. разобраться как именно вообще работает --mount=type=cache -> каталог кэша сохраняется между слоями, между образами (в multistage build) и даже между сборками. !!! Не работает с docker image build --no-cache (так как docker image build --no-cache инвалидирует предыдущий кэш)
# 4. понять в чем смысл использования кэша вместе с multistage build -> в том что мы делаем итоговый образ не только без кэша, но и без самого UV, т.е. делаем его еще меньше
