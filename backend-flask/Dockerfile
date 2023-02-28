#FROM python:3.10-slim-buster -> Removed by Snyk recommendation
FROM python:3.12.0a5-slim

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=developent

EXPOSE ${PORT}
#python3 -m flask run --host=.0.0.0.0 --port=4567
#CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0", "--port=4567"]
RUN chmod +x backend.sh
RUN ./backend.sh