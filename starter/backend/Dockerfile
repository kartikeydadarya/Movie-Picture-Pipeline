FROM python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /app

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    gcc \
    libpcre3-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pipenv

COPY Pipfile Pipfile.lock ./

RUN pipenv install --system --deploy

COPY . .

EXPOSE 5000

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]
