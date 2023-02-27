# Week 1 — App Containerization
# What is App Conterization? 

This is the process of packaging an application and all its dependencies into a container that can run consistently across different computing environments. App containerization uses containerization technology such as *Docker, Kubernetes, or other container platforms to encapsulate an application and all its dependencies, including libraries, configuration files, and system tools, into a single package.*

This week, we work with Docker to centralize our Crudder Application. To achieve this, we installed Docker on the Backend Flask and the Frontend-react-js. 
First, I right click on the backend  Flash and created a new file names "Dockerfile"

<img width="394" alt="image" src="https://user-images.githubusercontent.com/48904934/221440794-974c45c4-3629-4c67-bfd7-d9b93adf87f2.png">

After this was created, i used the following commands to install

FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

<img width="754" alt="image" src="https://user-images.githubusercontent.com/48904934/221507354-a63b6ef6-22aa-4142-8a50-f1247325150e.png">


When this is added to the Docker file, the next step I installed python on my machine using the $ pip3 install –r requirements.txt (this is a file found in the directory workspace/backend-flask) in the backend-flask. 

To avoid installing python in my backend everytime I start my GitPod, I added the follwing command to my Gidpod.yml file.

<img width="727" alt="image" src="https://user-images.githubusercontent.com/48904934/221441238-be76dfe9-1916-434d-8acd-05a3b1cc77ad.png">

Proceed by installing the python server flask on Docker, to do this use thee command $ python3 –m flask run –host=0.0.0.0 --port=4567 

After this process, a port link to the back end would be created and this port link must be made public to see if the Backend is working. You can as well click on the port tab to  see the opened port. 

<img width="560" alt="image" src="https://user-images.githubusercontent.com/48904934/221441546-cc267d37-9cc7-40f9-90d1-a7b15b8bf0aa.png">

When you click on the link, you might get a404 error response on the linux environment. If so, Kindly check if the environment variables are been set.

# What are environment variables?

*Environment variables are a type of variable that is set in the operating system or runtime environment and is accessible by programs and processes running on that system. These variables contain information about the environment in which a program or process is running, such as system configuration settings, user preferences, or other runtime parameters.*

*Environment variables are typically defined as key-value pairs, where the key is the name of the variable, and the value is the information stored in that variable. For example, the PATH variable in Windows and Linux operating systems contains a list of directories that the operating system searches when a user types a command in the command line interface.*

During this week 1 bootcamp, I learnet that there is a major difference between the RUN command and the CMD command in Docker. RUN command is used during the build process to execute commands, which can be termed to be as adding layers when executing commands in Docker and modify the image, while CMD is used to define the default command or process to be executed when a container is started.

After the above step was concluded, the next step to setup Docker is to build the container which should be installed in my root directory (aws-bootcamp-cruddur-2023). and then running it. Building the container comes first. To do this, I navigated to my Terminal and added the following code

docker build -t  backend-flask ./backend-flask

After this is donw, I ran the Docker file with the following code below

docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL="*"
unset BACKEND_URL="*"

After this is done, I navigated to my port and open the link. this worked, and I saw the profile details of users 

<img width="758" alt="image" src="https://user-images.githubusercontent.com/48904934/221501317-5a1cc04c-d445-4521-b586-8339c1dc65b2.png">

I proceeded with containerizing the backend.

<img width="337" alt="image" src="https://user-images.githubusercontent.com/48904934/221508094-7d8d9269-6b2e-4f23-a05b-3ae237791416.png">


After this is done, changed my directory to the frontend, in this case which is ‘$ cd frontend-react-js’ and ran an npm install. The npm install is a command used in the Node.js ecosystem to install packages (also known as modules) from the npm registry, which is necessary for our front end to work.

After this, I created a Docker file in the frontend-react-js, where I added the following code

FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]

<img width="719" alt="image" src="https://user-images.githubusercontent.com/48904934/221508613-8890bcf2-6bde-4a6a-bf77-4c58e1883792.png">


Like the backend container, to configure the Docker file on the frontend, I built the container using docker build -t frontend-react-js ./frontend-react-js and build the conatiner docker run -p 3000:3000 -d frontend-react-js which runs on port 3000.

<img width="263" alt="image" src="https://user-images.githubusercontent.com/48904934/221442372-07e7f77a-1fa6-4040-adaf-24815ff625d6.png">

Now that we have the two containers running individually, the next step is to install the file which would allow us run multiple containers at the same so we can get the real time experience of frontend and Backend. Also, to Know if our backend and frontend are communicating together.

Next. I created a docker-compose.yml at the root of my project.

# What is a Docker-compose-yml file?

A docker-compose.yml file is a configuration file used by Docker Compose, a tool for defining and running multi-container Docker applications. The docker-compose.yml file defines the services, networks, and volumes that make up the application and specifies how they should be configured and connected.

<img width="255" alt="image" src="https://user-images.githubusercontent.com/48904934/221442530-0a3346ba-03bd-49dc-b0b5-16b49777012a.png">

After creating the Docker compose file, I added the following code in the file 

version: "3.8"
services:
  backend-flask:
    environment:
      FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
      BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./backend-flask
    ports:
      - "4567:4567"
    volumes:
      - ./backend-flask:/backend-flask
  frontend-react-js:
    environment:
      REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
    build: ./frontend-react-js
    ports:
      - "3000:3000"
    volumes:
      - ./frontend-react-js:/frontend-react-js

*the name flag is a hack to change the default prepend folder*
*name when outputting the image names*
networks: 
  internal-network:
    driver: bridge
    name: cruddur
    name: cruddur
 




