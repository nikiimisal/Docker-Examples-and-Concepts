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


# Docker Volume


#### What is a Docker Volume?

A Docker volume is a mechanism used by Docker to persist and share data generated by containers.<br>
By default, when a container is deleted, all data inside it is lost. Volumes solve this problem by storing data outside the container’s writable layer.


### Why Docker Volumes are needed

- 📦 Data persistence – data remains even if the container stops or is removed<br>
- 🔄 Container independence – volumes live beyond the container lifecycle<br>
- 🔗 Data sharing – multiple containers can use the same volume<br>
- ⚡ Better performance – optimized for Docker storage<br>
- 🧹 Easy backup & restore

---

### How Docker Volumes Work ?

<p align="center">
  <img src="" width="500" alt="Initialize Repository Screenshot">
</p>


- Docker creates a managed directory on the host<br>
- This directory is mounted into the container<br>
- Container reads/writes data → data goes to the volume, not the container layer

---

## Types of Docker Storage

### 1️⃣ Temporary Docker Volumes

Temporary volumes store data only for the lifetime of the container.

🔹 tmpfs Volume

A tmpfs volume stores data in memory (RAM), not on disk.

Key Points:

- Data is lost when container stops<br>
- Very fast<br>
- No disk usage<br>
- Used for sensitive or temporary data<br>

```
docker run -d \ --tmpfs /app/cache \ nginx
```
➡️ Ideal for cache, session data, secrets

>Example – Temporary Volume:

>This is an example of a temporary volume. When we install an application inside a container, the data exists only until the container is running. Once the container is removed, the installed application and its data are also removed.

---

### 2️⃣ Permanent Docker Volumes

Permanent volumes are used when data must persist even after the container is stopped or removed.<br>
They are commonly used for databases, application data, logs, and backups.

➡️ Data is stored outside the container layer<br>
➡️ Safe from container deletion<br>
➡️ Can be reused and shared across containers

---

## Permanent Volumes have 3 types:

### 🔹 1. Bind Mount

A Bind Mount directly maps a specific directory from the host system into the container.

Key Points:

- Host path is explicitly mentioned<br>
- Not managed by Docker<br>
- Useful for development<br>
- Changes reflect immediately between host and container


➡️ Host directory: `/home/user/app`<br>
➡️ Container directory: `/usr/share/nginx/html`

⚠️ Risky in production if host path changes or is deleted.

---

### 🔹 2. Named Volume

A Named Volume is a Docker-managed volume with a user-defined name.


Key Points:

- Managed fully by Docker<br>
- Can be reused by multiple containers<br>
- Easy to backup, inspect, and remove<br>
- Most recommended volume type

➡️ `mydata` is the named volume<br>
➡️ Data remains even if container is deleted

---

### 🔹 3. Anonymous Volume

An Anonymous Volume is created automatically by Docker without a name.

Key Points:

- Docker assigns a random ID
- Hard to track and manage
- Exists until manually removed
- Mostly used internally or unintentionally

➡️ Volume is created automatically<br>
➡️ No user-defined name

---

## Example's

### ex 1. Bind Volume

> We have already completed the Docker setup. You can refer to the previous information directory.

1. Login to the EC2 instance (host/server) using the terminal.<br>
2. If any old containers are running, stop and remove them.<br>
3. Next, move to the host machine’s directory and create a new folder. This directory will be used to store website files on the host.<br>

  > You can also go inside the Docker container and create a content in there as well.
    `docker exec -it 123 /bin/bash`<br>
    `cd /usr/share/nginx/html`<br>
    `apt update`<br>
    `apt install nano`<br>
    `nano login.html`<br>


```
mkdir /home/ec2-user/
```

> The `-v` option is used to mount a host directory into the container so the container can access host files.
 
 Run the Nginx container with volume mapping:

```
docker run -d -p 80:80 --name mynginx -v /home/ec2-user/nikii/:/usr/share/nginx/html/ nginx
```
<p align="center">
  <img src="" width="500" alt="Initialize Repository Screenshot">
</p>

The container is now running and ready.

4. Access the application using the EC2 `< public IP >` in the browser.<br>
   You may see an error (403 or blank page) because no files are present in the mounted directory.<br>
   This example shows how Docker volumes work and how host directories are shared with containers.

<p align="center">
  <img src="" width="500" alt="Initialize Repository Screenshot">
</p>

- Now, we will add some code or content to the host directory and run it to test the website. 
- You can also check inside the container; the same code will be available there as well.


| **Terminal**    | **Output**          | **Output**          |
|--------------------------------|------------------------------------|------------------------------------|
| ![VS]() | ![AWS]() | ![AWS]) |
















---
---



























































