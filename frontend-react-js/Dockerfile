#FROM node:16.18 -> Removed by Snyk recommendation
#FROM node:19.6.0-bullseye-slim
FROM node:19.6.1

WORKDIR /frontend-react-js

ENV PORT=3000

COPY . /frontend-react-js

RUN npm install
EXPOSE ${PORT}

#CMD ["npm", "start"]
RUN chmod +x frontend.sh
RUN ./frontend.sh