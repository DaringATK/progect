FROM python:3.6-alpine

ENV MONGO localhost
ENV MONGO_PORT 27017
ENV RMQ_HOST localhost
ENV RMQ_QUEUE 1
ENV RMQ_USERNAME 1
ENV RMQ_PASSWORD 1
ENV CHECK_INTERVAL 1
ENV EXCLUDE_URLS

copy /__init__.py /app/
copy /crawler.py /app/
copy /requirements-test.txt /
copy /requirements.txt /
RUN pip install -r /requirements.txt
WORKDIR /app
CMD ["gunicorn", "-w 4", "main:app"]


