FROM python:3.6-alpine

ENV MONGO mongodb
ENV MONGO_PORT 27017
ENV RMQ_HOST rabbitmq
ENV RMQ_QUEUE derp
ENV RMQ_USERNAME derp
ENV RMQ_PASSWORD derp
ENV CHECK_INTERVAL 60
ENV EXCLUDE_URLS '.*github.com'

copy ./src/* /app/
copy /requirements-test.txt /
copy /requirements.txt /
RUN pip install -r /requirements.txt -r /requirements-test.txt
WORKDIR /app
#CMD ["gunicorn", "-w 4", "main:app"]
CMD python -u crawler.py https://vitkhab.github.io/search_engine_test_site/
