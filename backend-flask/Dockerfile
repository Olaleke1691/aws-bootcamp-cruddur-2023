FROM python:3.10-slim-buster

# Container
# make a new folder inside container
WORKDIR /backend-flask

# Outside Container -> Inside Container
# Copy the requirement files.txt inside the container. 
# This is because it contains the Libraries want to install to run the app
COPY requirements.txt requirements.txt

# Inside-Container
# install python libraries used for the app.
RUN pip3 install -r requirements.txt

# Outside Conainer -> Inside Container
# . means everything in the current directory
# . first period means /backend-flask (outside the container).
# .. secound period means /backend-flash (inside the container)
COPY . .

# set ENV Vars
# set inside the container and would remain set when the conatiner is running.

ENV FLASK_ENV=development

EXPOSE ${PORT}


# python3 -m flask run --host=0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]