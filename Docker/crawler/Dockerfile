FROM python:3.6-alpine

copy ./src/* /app/
copy /requirements-test.txt /
copy /requirements.txt /
RUN pip install -r /requirements.txt -r /requirements-test.txt
WORKDIR /app
#CMD ["gunicorn", "-w 4", "main:app"]
CMD python -u crawler.py https://vitkhab.github.io/search_engine_test_site/
