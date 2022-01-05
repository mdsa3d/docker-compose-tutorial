# Docker Compose Tutorial
This repository shows a simple demonstration of configuring docker compose service using nodejs app and nginx as reverse proxy.

1. Test nodejs app

To test the nodejs application, open the terminal and go to the `app` directory and run `index.js` file.

```sh
cd ~/app
node index.js
```
A nodejs server will be initialized at http://localhost:5000

2. Run docker compose

This tool will allow us to start multiple containers with single command. To initiate the containers run following command:

*Linux (Ubuntu)*         `docker-compose up --build`
*Windows*                `docker compose up --build`

This should start your application at `http://localhost:80`.