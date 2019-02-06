FROM python:3.6-alpine

COPY ./src/ /app/
COPY ./tests /tests
COPY /requirements-test.txt /
COPY /requirements.txt /
RUN pip install -r /requirements.txt -r /requirements-test.txt
WORKDIR /app
CMD FLASK_APP=ui.py gunicorn ui:app -b 0.0.0.0
