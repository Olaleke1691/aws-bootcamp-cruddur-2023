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

![image](https://user-images.githubusercontent.com/48904934/221510788-0583f823-6307-4caf-aaf0-026472312a70.png)


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
    
 ![image](https://user-images.githubusercontent.com/48904934/221520463-9b0546e1-c893-4046-9f00-500b10848020.png)
 
Once this is done, the next step would be to add databases, where information would be stored locally and retrived.

Doing this, we would be using the DynamoDB Local and Postgres. The DynamoDB local would be installed in the Docker-compose-yml file, using the following code below.

services:
  dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
    
The DynamoDB Local is the first to install in your Docker-composer-yml file, then followed by the Postgres DB.

Then added the Postgres Db with the code below ##services:
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
volumes:
  db:
    driver: local
    
After this, I added the Postgres client to Gitpod.yml file, so I do not keep installing this everytime I start my Gitpod.

  - name: postgres
    init: |
      curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
      echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
      sudo apt update
      sudo apt install -y postgresql-client-13 libpq-dev
      
![image](https://user-images.githubusercontent.com/48904934/221512948-b948662b-4163-4271-a727-f2ccd9693833.png)

Next, I installed the Postgres extension on my Gitpod, from your extension market.

![image](https://user-images.githubusercontent.com/48904934/221513877-9e018e0a-399d-4f14-bdca-cc39bf57a097.png)

After this has been installed, locate the Database icon on the left panel, rightclick and click add a new connection.

<img width="260" alt="image" src="https://user-images.githubusercontent.com/48904934/221514995-e3d946ed-df00-447f-afef-0dbc5bf9e97a.png">

![image](https://user-images.githubusercontent.com/48904934/221522210-39ff3b0f-fdde-426d-9d5f-31b3346d11ee.png)

# Container Security Considerations 

After watching the video about security considerations we analized the project with Snyk tool. (snyk.io)

First we sign up and link to our github account to select a repository.

![image](https://user-images.githubusercontent.com/48904934/221524261-a4bc0acb-12c5-4d6d-92e7-754ae336125f.png)


After configuring the Database, proceed to installing the Open API extension on your Gitpod.

Next is to add an andpoint for the notifications tab, to do this click on the hanburger icon located on the right side of the Path. 

![image](https://user-images.githubusercontent.com/48904934/221518328-11fae309-736f-4f6c-a529-1a21b5daf62d.png)

When a new API Path is created, this would create a new line of code in the API.  

Next I renamed the Path to... /api/activities/notifications endpoint.

/api/activities/notifications:
    get:
      description: 'Return a feed of activities for all notifications'
      tags:
       - activities
      parameters: []
      responses:
        '200':
          description: Returns an array of activities
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Activity'
                  
AFter this, I Created the service and route for the backend notification feature, this was done from the backend-flask/app.py

import os

from services.home_activities import *
from services.notifications_activities import *
from services.user_activities import *
from services.create_activity import *
from services.create_reply import *
@@ -65,6 +66,11 @@ def data_home():
  data = HomeActivities.run()
  return data, 200

@app.route("/api/activities/notifications", methods=['GET'])
def data_notifications():
  data = NotificationsActivities.run()
  return data, 200

@app.route("/api/activities/@<string:handle>", methods=['GET'])
def data_handle(handle):
  model = UserActivities.run(handle)
  
backend-flask/services/notifications_activities.py

from datetime import datetime, timedelta, timezone
class NotificationsActivities:
  def run():
    now = datetime.now(timezone.utc).astimezone()
    results = [{
      'uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
      'handle':  'Enyioma Nwadibia',
      'message': 'Cloud is fun!',
      'created_at': (now - timedelta(days=2)).isoformat(),
      'expires_at': (now + timedelta(days=5)).isoformat(),
      'likes_count': 5,
      'replies_count': 1,
      'reposts_count': 0,
      'replies': [{
        'uuid': '26e12864-1c26-5c3a-9658-97a10f8fea67',
        'reply_to_activity_uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
        'handle':  'Worf',
        'message': 'This post has no honor!',
        'likes_count': 0,
        'replies_count': 0,
        'reposts_count': 0,
        'created_at': (now - timedelta(days=2)).isoformat()
      }],
    },
    ]
    return results
    
  Created the notification page and route for the frontend app also.
  
  ![image](https://user-images.githubusercontent.com/48904934/221520099-46cea84a-7aba-4b06-bae3-b076d894a4fe.png)

I validated the access to the databases, by creating a table, and put items into them and then query the items.

![image](https://user-images.githubusercontent.com/48904934/221521011-a7c73b93-914d-4410-bcb0-2848eb2481ca.png)

![image](https://user-images.githubusercontent.com/48904934/221521608-e6114ffc-fdf0-4530-8a84-fbfc45fe1b8b.png)

![image](https://user-images.githubusercontent.com/48904934/221521860-329c49fd-5c64-4b6f-9505-27c14a156947.png)

![image](https://user-images.githubusercontent.com/48904934/221521990-932de1c9-7844-4867-9c9e-d9a9b50408dd.png)

In the dashboard of my Synk, I can see the status of my project and it's vulnerablities.

![image](https://user-images.githubusercontent.com/48904934/221557855-30212200-efad-4ac7-b4b3-9576f82fda15.png)

To fix the issue of pending task, I clicked on the fix vulnerablities.

The Recommendations here is for upgrading the base image.

![image](https://user-images.githubusercontent.com/48904934/221559496-51cc6e16-14e1-4600-9627-4852c3e01ece.png)

After applying the suggestions, the my dashboard still shows there are pending tasks.

![image](https://user-images.githubusercontent.com/48904934/221597832-7f2bf98a-5c8a-4a2c-baa6-c3d5b20ddc3e.png)

Would ask for help on fixing this.

# Wrote a Flask Backend Endpoint for Notifications

The endpoint is going to be added in the start point of the Backend flask, which is /backend-flask/app.py

![image](https://user-images.githubusercontent.com/48904934/221599416-b9b45bb0-1004-48d2-9c20-ae0089171bf1.png)

from services.notifications_activities import *

@app.route("/api/activities/notifications", methods=['GET'])
def data_notifications():
  data = NotificationsActivities.run()
  return data, 200

Next I added a notification function call as seen below

![image](https://user-images.githubusercontent.com/48904934/221599924-a462518e-d2dd-4826-8625-daa452f15556.png)

Next, I added /backend-flask/services/notifications_activities.py 

   
    '''
      from datetime import datetime, timedelta, timezone
      class NotificationsActivities:
       def run():
    now = datetime.now(timezone.utc).astimezone()
    results = [{
      'uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
      'handle':  'Coco',
      'message': 'I am a white unicorn!',
      'created_at': (now - timedelta(days=2)).isoformat(),
      'expires_at': (now + timedelta(days=5)).isoformat(),
      'likes_count': 5,
      'replies_count': 1,
      'reposts_count': 0,
      'replies': [{
        'uuid': '26e12864-1c26-5c3a-9658-97a10f8fea67',
        'reply_to_activity_uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
        'handle':  'Worf',
        'message': 'This post has no honor!',
        'likes_count': 0,
        'replies_count': 0,
        'reposts_count': 0,
        'created_at': (now - timedelta(days=2)).isoformat()
      }],
    }
    ]
    return results
    
# React Page for Notifications

This was achieved by creating the notification page, notification css and the route using the code below.

It was implemeted in the **/frontend-react-js/src/App.js:**

'''
   import NotificationsFeedPage from './pages/NotificationsFeedPage';
   const router = createBrowserRouter([
  ...
  ...
  {
    path: "/notifications",
    element: <NotificationsFeedPage />
  },
  ...
  ...
]);

Next, I added in the /frontend-react-js/src/pages/NotificationsFeedPage.js

'''
    import './NotificationsFeedPage.css';
    import React from "react";

import DesktopNavigation  from '../components/DesktopNavigation';
import DesktopSidebar     from '../components/DesktopSidebar';
import ActivityFeed from '../components/ActivityFeed';
import ActivityForm from '../components/ActivityForm';
import ReplyForm from '../components/ReplyForm';

// [TODO] Authenication
import Cookies from 'js-cookie'

export default function NotificationsFeedPage() {
  const [activities, setActivities] = React.useState([]);
  const [popped, setPopped] = React.useState(false);
  const [poppedReply, setPoppedReply] = React.useState(false);
  const [replyActivity, setReplyActivity] = React.useState({});
  const [user, setUser] = React.useState(null);
  const dataFetchedRef = React.useRef(false);

  const loadData = async () => {
    try {
      const backend_url = `${process.env.REACT_APP_BACKEND_URL}/api/activities/notifications`
      const res = await fetch(backend_url, {
        method: "GET"
      });
      let resJson = await res.json();
      if (res.status === 200) {
        setActivities(resJson)
      } else {
        console.log(res)
      }
    } catch (err) {
      console.log(err);
    }
  };

  const checkAuth = async () => {
    console.log('checkAuth')
    // [TODO] Authenication
    if (Cookies.get('user.logged_in')) {
      setUser({
        display_name: Cookies.get('user.name'),
        handle: Cookies.get('user.username')
      })
    }
  };

  React.useEffect(()=>{
    //prevents double call
    if (dataFetchedRef.current) return;
    dataFetchedRef.current = true;

    loadData();
    checkAuth();
  }, [])

  return (
    <article>
      <DesktopNavigation user={user} active={'notifications'} setPopped={setPopped} />
      <div className='content'>
        <ActivityForm  
          popped={popped}
          setPopped={setPopped} 
          setActivities={setActivities} 
        />
        <ReplyForm 
          activity={replyActivity} 
          popped={poppedReply} 
          setPopped={setPoppedReply} 
          setActivities={setActivities} 
          activities={activities} 
        />
        <ActivityFeed 
          title="Notifications" 
          setReplyActivity={setReplyActivity} 
          setPopped={setPoppedReply} 
          activities={activities} 
        />
      </div>
      <DesktopSidebar user={user} />
    </article>
  );
}









