FROM python:3.7-slim

LABEL author="Zhbakov A" version=2

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt --upgrade pip

COPY .. .

CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000"]