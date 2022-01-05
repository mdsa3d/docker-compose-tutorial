# docker-compose-tutorial
Docker Compose tutorial for nodejs app and nginx reverse_proxy

## NodeJS server

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
A nodejs server is initialized at [Link](http://localhost:5000)

**Creating Dockerfile**
A text file with image assembling instruction for docker.
Create a blank file in the `app` directory and named it `Dockerfile`, this will automatically change the filetype. Under Dockerfile insert the following context.

```Dockerfile
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
```.dockerignore
node_modules
npm-debug.log
```

## Nginx Docker
```sh
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

_Dockerfile_
```Dockerfile
FROM nginx
COPY default.conf /etc/nginx/conf.d/default.conf
```



## Docker-Compose

This tool will allow us to start multiple containers with single command script. Moreover, it builds a common netwrok between containers for container-container communication.
To generate this file, create an empty file in the main directory (outside `app` directory) and rename it to `docker-compose.yml`. This file will contain the configuration of each container and services. Detailed documentation can be find here [Link](https://docs.docker.com/compose/).

There are two containers being created in the below file, `nodeserver` which will run our application and `nginx` this will be the reverse proxy & load-balancer.
Under each container we have `build` variable which will build the image using the dockerfile present in each directory (`app` and `nginx`). Second variable is `ports`, for nodeserver we are forwardign internal (docker) port 5000 to 80 and our nginx is listening on port 80 on both ends.  
Docker-compose file:
```yml
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

This should start your application at [Link](http://localhost:80).