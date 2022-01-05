# docker-compose-tutorial
Docker Compose tutorial for nodejs app and nginx reverse_proxy

![GitHub](https://img.shields.io/github/license/mdsa3d/docker-compose-tutorial?style=for-the-badge)
![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/mdsa3d/docker-compose-tutorial/pages%20build%20and%20deployment/gh-pages?style=for-the-badge)
https://img.shields.io/badge/docs-stable-blue?style=for-the-badge?link_&link_=https://mdsa3d.github.io/docker-compose-tutorial/

**1. NodeJS server**
Create a `app` directory and generate a file `index.js`.

Contents of `index.js` 

```javascript
// index.js
const express = require('express');
const app = express();
app.get('/', (req, res) => {
  res.send('Hello World!')
})
app.listen(5000, () => console.log('Server is up and running'));
```

To test the application you can open the terminal and run 

```javascript
node index.js
```
A nodejs server is initialized at http://localhost:5000

**Creating Dockerfile**
A text file with image assembling instruction for docker.
Create a blank file in the `app` directory and named it `Dockerfile`, this will automatically change the filetype. Under Dockerfile insert the following context.

```Dcokerfile
# pull the Node.js Docker image
FROM node:alpine

# create the directory inside the container
WORKDIR /usr/src/app

# copy the package.json files from local machine to the workdir in container
COPY package*.json ./

# run npm install in our local machine
RUN npm install
RUN npm install express
# copy the generated modules and all other files to the container
COPY . .

# our app is running on port 5000 within the container, so need to expose it
EXPOSE 5000

# the command that starts our app
CMD ["node", "index.js"]
```

In addition create `.dockerignore` file to speed up the docker build.
```bash
node_modules
npm-debug.log
```

**2. Nginx Docker**
```
server {
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_pass http://nodeserver:5000;
    }
}
```

Dockerfile
```
FROM nginx
COPY default.conf /etc/nginx/conf.d/default.conf
```



**3.Docker-Compose**
This tool will allow us to start multiple containers with single command script. Moreover, it builds a common netwrok between containers for container-container communication.
To generate this file, create an empty file in the main directory (outside `app` directory) and rename it to `docker-compose.yml`. This file will contain the configuration of each container and services. Detailed documentation can be find here `https://docs.docker.com/compose/`.

There are two containers being created in the below file, `nodeserver` which will run our application and `nginx` this will be the reverse proxy & load-balancer.
Under each container we have `build` variable which will build the image using the dockerfile present in each directory (`app` and `nginx`). Second variable is `ports`, for nodeserver we are forwardign internal (docker) port 5000 to 80 and our nginx is listening on port 80 on both ends.  
Docker-compose file:
```
version: "3.8"
services:
    nodeserver:
        build:
            context: ./app
        ports:
            - "5000:80"
    nginx:
        restart: always
        build:
            context: ./nginx
        ports:
            - "80:80"
```

Now, run the following command to start the containers:

*Linux (Ubuntu)*         `docker-compose up --build`
*Windows*                `docker compose up --build`

This should start your application at `http://localhost:80`.
