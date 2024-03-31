# Docker for Beginners: Challenges Guide

## Challenge 1: Static Website with Docker

**Objective**: Learn how to containerize a static website using Docker and NGINX.

## Prerequisities:
- Install Docker for windows.
- A computer restart is needed once instlled.  
  
### Step 1: Prepare Your Workspace

- Create a directory called `challenge1`.
- Inside `challenge1`, create a `public` directory for static assets like HTML, CSS, and JavaScript files.
- For the purpose of this challenge we will create an index.html in the public folder. Crate `index.html` and add you name and student ID.  

### Step 2: Write Your Dockerfile

- In `challenge1`, create a `Dockerfile`. Root of challenge1 folder not in the public folder.
- Use the NGINX image from Docker Hub as the base image. [NGINX image from Docker Hub](https://hub.docker.com/_/nginx).  
- In your dockerfile the image at `/usr/share/nginx/html`. it should point to `./public /usr/share/nginx/html`

### Step 3: Build and Run Your Container

- Use the `docker build -t my-nginx-image` command to build your Docker image.
- Run your container with `docker run`, mapping port 8080 on your host to port 80 on the container. `docker run --name my-nginx-container -d -p 8080:80 my-nginx-image`


### Step 4: Access Your Static Website

- Open a browser and navigate to `http://localhost:8080`.
- Verify that your static website is being served by the container.
- Congrats you completed challenge1 :).
  
## Challenge 2: Dynamic Node.js Application with Docker Compose

**Objective**: Deploy a dynamic Node.js application using Docker Compose, with NGINX as a reverse proxy.

### Step 1: Unzip and Organize

- Create a directory called `challenge2`.
- Extract the contents of `challenge2.zip` into this directory.

### Step 2: Write Your Dockerfile for Node.js App

- In `challenge2`, create a `Dockerfile` for your Node.js app.
- Use the Node.js image from Docker Hub.
  
### Step 4: Configure Docker Compose

- Write a `docker-compose.yml` file.
- Define services for your app and NGINX.
- Map port 8080 on the host to NGINX in the container.

```
yaml
version: "3"
services:
  app:
    build: .
    container_name: challenge2_app
    expose:
      - "3000"

  webserver:
    image: nginx:latest
    container_name: challenge2_webserver
    ports:
      - "8080:8080"
    depends_on:
      - app
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
  ```

### Step 4: Set Up NGINX Configuration

- Create an `nginx.conf` file. in `challenge2`
- Configure NGINX to reverse proxy requests to the Node.js app on port 3000.
- Here is what your `nginx.conf` should look like.

```
nginx
events {}

http {
    server {
        listen ????; # This is the port NGINX listens on the host. it should match the ports in `docker-compose.yml`

        location / {
            # The address of your app; replace 'app:3000' with your app's address and port.
            proxy_pass http://app:3000; 
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            # Add additional configuration here if necessary.
        }
    }
}

```


### Step 5: Build and Start Your Services  

- Navgate to `challenge2` directory with `cd` in terminal.
- Run `docker-compose up` to start your multi-container application.
- You might get an error here. This is due to the fact that the container in challenge1 is still using port 8080. In your docker desktop; stop the `challenge1` container.

### Step 6: Verify Your Dynamic Application

- Open a browser and navigate to http://localhost:8080/api/books and http://localhost:8080/api/books/1
- Confirm you can access the Node.js app through NGINX. You should be able to read the .JSON files on this page, displaying books.

## References

- [Official Docker documentation](https://docs.docker.com/)
- [NGINX Documentation](http://nginx.org/en/docs/)

## Screenshots

![challenge1-localhost](https://github.com/Ccrewe92/docker-challenge-template/assets/119900792/58df2163-d9f2-4c4e-88d1-035d2e393958)
![challenge1-terminal](https://github.com/Ccrewe92/docker-challenge-template/assets/119900792/35fc6ee3-6f40-41f5-bdea-af8093dd5de8)
![challenge2-terminal](https://github.com/Ccrewe92/docker-challenge-template/assets/119900792/b01301db-add7-44c1-a495-28f5c66e3135)
![challenge2-book-1](https://github.com/Ccrewe92/docker-challenge-template/assets/119900792/34fe9b44-6b7c-48f5-8755-30530329ed36)
![image](https://github.com/Ccrewe92/docker-challenge-template/assets/119900792/32f33fc7-8600-43bd-815c-9a1857323780)

