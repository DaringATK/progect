FROM python:3.6-alpine

ENV MONGO localhost
ENV MONGO_PORT 27017

copy /__init__.py /app/
copy /ui.py /app/
copy /requirements-test.txt /
copy /requirements.txt /
copy /templates /
RUN pip install -r /requirements.txt
WORKDIR /app
CMD ["gunicorn", "-w 4", "main:app"]

