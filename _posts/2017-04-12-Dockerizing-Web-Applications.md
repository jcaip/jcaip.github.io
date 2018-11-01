---
layout: post
tags: [infra/web-dev]

images:

    - url: /images/post_covers/web_app.png
      alt: CICD
      title: CICD
---

This blog post is about the anatomy of a sample docker project you can find [here](https://github.com/jcaip/react_flask_dockerized).
Make sure you have Docker installed - if you don't follow the instructions [here](https://www.docker.com/) to install it.
<!--more-->

## Makefile
The Makefile is not neccessary, but it makes using this project a bit nicer. Since most people have had some experience with Makefiles, it's an easy way to bring the Docker containers up.

The make commands just call corresponding docker-compose commands. 

## docker-compose.yml
docker-compose allows us to easily manage web applicatoins that use multiple docker containers. Docker-compose has commands to start, stop , and rebuild containers and see the outputs of containers. You can follow [these](https://docs.docker.com/compose/install/) instructions to install docker-compose.

Our docker-comose.yml is pretty simple. It spins up two services, src and static, and binds src to port 5000, and static to port 80.

```yml
version: '2'
services:
    src:
        build: ./src
        ports:
            - "5000:5000"
    static:
        build: ./static
        ports:
            - "80:80"
```

## src
The src folder contains the backend of the web application. This is a simple python backend using [Flask](http://flask.pocoo.org/) and [Gevent](http://www.gevent.org/). 

#### Dockerfile
```
FROM ubuntu:latest
MAINTAINER Jesse Cai "jcjessecai@gmail.com"

RUN apt-get update -y
RUN apt-get install -y python-pip python-dev build-essential

COPY . /app
WORKDIR /app

RUN pip install -r requirements.txt

ENTRYPOINT ["python"]
CMD ["server.py"]
```
Our dockerfile for the src container is very simple. It simply pulls down the latest ubuntu image, installs the necessary depndencies, and then runs server.py

#### server.py
```python
from flask import Flask
from flask_cors import CORS, cross_origin
from gevent.wsgi import WSGIServer

app = Flask(__name__)
CORS(app)

@app.route('/')
def test():
    return "Hello World!"

@app.route('/two')
def test_two():
    return "Working!"

if __name__ == '__main__':
    http_server = WSGIServer(('', 5000), app)
    http_server.serve_forever()
```
`server.py` exposes two endpoints, `/` and `/two` that simply return a string. Gevent is used here to get around the global interperter lock in python and enable us to have multiple threads running at the same time.

## static
The static folder contains the frontend of the project, and it's a bit more complicated than the backend. For this project, we're using [React](https://facebook.github.io/react/) and [Nginx](https://www.nginx.com/resources/wiki/). The core of the project was written by [Kevin Wang](https://xorkevin.github.io/) and you can check out the not dockerized version [here](https://github.com/xorkevin/reactant). I'm not too familar with React, so I'm just going to go over how I dockerized the application.

#### Dockerfile
This docker container essentially builds the React application, and then lets Nginx serve it. 

We start out by pulling the node image from docker.
```Dockerfile
FROM node:latest
MAINTAINER Jesse Cai <jcjessecai@gmail.com>
```

Next we install nginx so we can serve our files and also copy over our configuration file
```Dockerfile
RUN apt-get update -y && apt-get install -y nginx

COPY ./nginx/default.conf /etc/nginx/nginx.conf
```

Here we cache the npm install and then install all the necessary node dependencies.
```Dockerfile
# Prepare app directory
RUN mkdir -p /static

# cache so npm install doesn't take ages every time
ADD package.json /static/package.json
RUN cd /static && npm install

ADD . /static
```

Now we use node to build our page. and then copy it over to the folder that nginx watches.
```Dockerfile
#build the paage
WORKDIR /static
RUN npm run build

# copy  over the necessary files
RUN mkdir -p public && mkdir -p public/build
RUN cp -r pages/* public && cp lib/build/main.css public/build && cp lib/build/main.js public/build
```

Now we expose port 80 so that Nginx can listen to traffic, and also start it with the daemon off. It's important to add daemon off because the Docker container will exit when it's initial process end. If you run without daemon off - the Nginx process will spin up some child processes and then exit, causing the container to end as well.
``` Dockerfile
WORKDIR /static/public
# Expose the app port
EXPOSE 80

# Start the app
CMD ["nginx", "-g", "daemon off;"]
```
Now our frontend is ready and able to serve webpages.
