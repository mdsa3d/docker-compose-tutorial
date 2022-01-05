# Docker Compose Tutorial
This repository shows a simple demonstration of configuring docker compose service using nodejs app and nginx as reverse proxy.

<<<<<<< HEAD
1. Test nodejs app
=======
![GitHub](https://img.shields.io/github/license/mdsa3d/docker-compose-tutorial?style=for-the-badge)
![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/mdsa3d/docker-compose-tutorial/pages%20build%20and%20deployment/gh-pages?style=for-the-badge)
[![](https://img.shields.io/badge/docs-stable-blue?style=for-the-badge)](https://mdsa3d.github.io/docker-compose-tutorial/)

**1. NodeJS server**
Create a `app` directory and generate a file `index.js`.
>>>>>>> cad3479e826c461518a972d70cd5c8e688328a1b

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
