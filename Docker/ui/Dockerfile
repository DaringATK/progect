FROM python:3.6-alpine

ENV MONGO mongodb
ENV MONGO_PORT 27017

copy ./src/* /app/
CMD mkdir /tests
COPY ./tests /tests
#copy /ui.py /app/
copy /requirements-test.txt /
copy /requirements.txt /
#copy /templates /
RUN pip install -r /requirements.txt -r /requirements-test.txt
WORKDIR /app
#CMD ["gunicorn", "-w 4", "main:app"]
CMD FLASK_APP=ui.py gunicorn ui:app -b 0.0.0.0
