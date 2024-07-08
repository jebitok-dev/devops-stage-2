# Dockerfile for FastAPI app
FROM python3.10-slim-buster AS runtime

WORKDIR /app

RUN pip install poetry

# Install needed packages directly
RUN pip install --no-cache-dir \
    fastapi==0.65.2 \
    uvicorn==0.13.4 \
    sqlalchemy==1.4.15 \
    psycopg2-binary==2.8.6 \
    pydantic==1.8.2

COPY pyproject.toml poetry.lock ./
COPY app ./app

RUN poetry config virtualenvs.create false && poetry install --only main

COPY --from=build-backend /app /app

COPY . .

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
