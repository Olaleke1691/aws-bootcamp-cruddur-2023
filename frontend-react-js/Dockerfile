#FROM node:16.18 -> Removed by Snyk recommendation
#FROM node:19.6.0-bullseye-slim
FROM node:19.6.1

WORKDIR /frontend-react-js

ENV PORT=3000
ENV REACT_APP_BACKEND_URL="https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"

COPY . /frontend-react-js

RUN npm install
EXPOSE ${PORT}

#CMD ["npm", "start"]
RUN chmod +x frontend.sh
RUN ./frontend.sh

FROM nginx
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /frontend-react-js/build /usr/share/nginx/html