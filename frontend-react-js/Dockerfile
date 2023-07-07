# Use a multi-stage build to reduce image size
# Stage 1: Build the application
FROM node:16.20 AS build

WORKDIR /frontend-react-js

COPY . /frontend-react-js

RUN npm install && \
    npm run build

# # Stage 2: Create the final image
# FROM node:19.6.1

# WORKDIR /frontend-react-js

ENV PORT=3000
ENV REACT_APP_BACKEND_URL="https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"

# COPY --from=build /frontend-react-js/build ./build

EXPOSE ${PORT}

CMD ["npm", "start"]

