# First stage: build stage
FROM python:3.11-slim-bullseye AS build-backend

WORKDIR /app

# Install build dependencies 
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev 

RUN pip install poetry

# Copy the project files
COPY pyproject.toml poetry.lock ./

# Install dependencies
RUN poetry config virtualenvs.create false && poetry install --no-dev

COPY app ./app

# Second stage: runtime stage
FROM python:3.11-slim-bullseye AS runtime

WORKDIR /app

# Install needed packages directly
RUN apt-get update && apt-get install -y \
    libpq-dev \
    gcc

# Install needed packages directly
RUN pip install --no-cache-dir \
    fastapi==0.65.2 \
    uvicorn==0.13.4 \
    sqlalchemy==1.4.15 \
    psycopg2-binary==2.8.6 \
    pydantic==1.8.2

# Copy the dependencies and application code from the build stage
COPY --from=build-backend /app /app

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
