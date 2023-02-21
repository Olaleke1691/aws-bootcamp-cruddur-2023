FROM python:3.10-slim-buster

# inside the container
# make a new folder inside the container
WORKDIR /backend-flask

# outside the container ---copy to---> inside the container
# the container contains the libraries you need to install to run the app.
COPY requirements.txt requirements.txt

# inside the container
# install python liraries  to run the app
RUN pip3 install -r requirements.txt

# insoce container ---copy to---> outside the container
COPY . .

# set ENV VARs
# would remain set when the container is running.
ENV FLASK_ENV=development

EXPOSE ${PORT}

#this is the command we would use to run flask
# python3 -m flask run --host= 0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

