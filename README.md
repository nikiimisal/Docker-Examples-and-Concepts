# Docker-Example-s

### Ex. 1 .  Deploying a Static Portfolio with Docker and Nginx


```
sudo -i
docker ps
docker ps -a
docker run -d -p 80:80 --name mynginx nginx
docker ps
exit


```
   <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20132346.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

```
scp -i my-cal1-key.pem -r iportfolio-1.0.0/ ec2-user@13.57.11.160:/home/ec2-user/
```

   <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20132045.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

```
sudo -i
cd /home/ec2-user/
mv iPortfolio-1.0.0/ myportfolio
ls
docker ps
ls
docker cp myportfolio/ mynginx:/usr/share/nginx/html/
```

   <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20132221.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

   <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20132909.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

  

---

### Ex. 2 .  I want to share the container. Until now, we have been using GitHub to share code, but here we will use a `Docker repository`

>First of all, you need to have an account on Docker Hub to push to a Docker repository.[click here](https://hub.docker.com/)

| **Create Account**    | **Home page**          | **Create a repo**          |
|--------------------------------|------------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20141018.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20141150.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20155109.png?raw=true) |


### This is the basic concept of how to pull code from Docker Hub. 


```
sudo -i
cd /home/ec2-user/
docker pull httpd:trixie
docker images
docker run -d -P --name newhttpd httpd
docker ps

```

| **Docker-Hub Repo**    | **Terminal**          | **Terminal**          |
|--------------------------------|------------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20160137.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20160227.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20160714.png?raw=true) |

- If you want to run this image, extract it from the .tar folder first; otherwise, it will throw an error.

- I pulled the static portfolio code, but it was inside folders. Since folders can’t be run, extract the files first and then run it.
- something like this
<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20161339.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

### Just like we pulled the image, we should also be able to push it.


- We cannot push a container directly. We first need to create an image from the container, and then push that image.
  
  ```
    docker commit 123                         # to create images from container
  ```
- See this screenshot: the image has been created, but we didn’t give it a name, and there is a reason behind that.

<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20163622.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

- The reason we didn’t give a name to the image is that earlier we were pulling files directly from GitHub after doing `git init`. <br>
- But Docker Hub doesn’t work the same way. While pushing, the image name must match the repository name.

```
docker tag 2fa nikiimisal/test_repo               # to name the img
docker login                                      # to login (init) docker hub
```
- Now that you have changed the name, the next step is to authenticate and log in to Docker Hub.
  
<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20164604.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

```
docker push nikiimisal/test_repo             # to push the code
```

<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20164618.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>


<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Example-s/blob/main/img/Screenshot%202026-01-03%20164654.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>



---
---
---



























































