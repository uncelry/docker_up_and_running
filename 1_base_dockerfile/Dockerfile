FROM python:3.12-slim

ENV PATH="/venv/bin:$PATH" \
    UV_PYTHON_DOWNLOADS=never \
    UV_COMPILE_BYTECODE=1 \
    UV_PROJECT_ENVIRONMENT=/venv

WORKDIR /app

COPY --link --from=ghcr.io/astral-sh/uv:0.7.8 /uv /usr/local/bin/uv
COPY pyproject.toml uv.lock .
RUN uv sync --locked --no-dev

COPY . .

CMD ["fastapi", "dev", "main.py", "--host", "0.0.0.0", "--port", "80"]
# CMD ["fastapi", "run", "main.py", "--host", "0.0.0.0", "--port", "80"] <- FOR PROD!!!
