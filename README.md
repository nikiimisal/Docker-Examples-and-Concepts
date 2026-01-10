# Docker-Example's & some Concept's

- [examples](#example-0)

<table>
  <thead>
    <tr>
      <th>Example </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <a href="#example-1">Example 1 –Deploying a Static Portfolio with Docker and Nginx</a><br>
         <a href="#example-2">Example 2 –Share container (push-pull) Docker hub Also create doc-hub account</a><br>
      </td>
    </tr>
  </tbody>
</table>

- [Docker Volume](#example-3)
    - [Temporary](#example-4)
    - [Permanent](#example-5)

<table>
  <thead>
    <tr>
      <th>Example </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <a href="#example-6">Example 1 – Bind Volume</a><br>
        <a href="#example-7">Example 2 – Named Volume</a><br>
        <a href="#example-8">Example 3 – Anonymous Volume</a><br>
         </td>
    </tr>
  </tbody>
</table>


- [Dockerfile & How To Use](#example-9)
- [Docker Optimization](#example-10)
- [Database Container](#example-11)







<a id="example-0"></a>
<a id="example-1"></a>
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
<a id="example-2"></a>

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

<a id="example-3"></a>

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
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Docker%20volume.jpg?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>


- Docker creates a managed directory on the host<br>
- This directory is mounted into the container<br>
- Container reads/writes data → data goes to the volume, not the container layer

---

## Types of Docker Storage

> The functionality of all three volumes is similar; the only difference is the use case—when and where you need to use them.

<a id="example-4"></a>

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

<a id="example-5"></a>

### 2️⃣ Permanent Docker Volumes

Permanent volumes are used when data must persist even after the container is stopped or removed.<br>
They are commonly used for databases, application data, logs, and backups.

➡️ Data is stored outside the container layer<br>
➡️ Safe from container deletion<br>
➡️ Can be reused and shared across containers

---

## Permanent Volumes have 3 types:

### 🔹 1. Bind Mount  

- [Example](#example-6)

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

- [Example](#example-7)
  
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

- [Example](#example-8)

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

<a id="example-6"></a>

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
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20091918.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

The container is now running and ready.

4. Access the application using the EC2 `< public IP >` in the browser.<br>
   You may see an error (403 or blank page) because no files are present in the mounted directory.<br>
   This example shows how Docker volumes work and how host directories are shared with containers.

<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20091331.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

- Now, we will add some code or content to the host directory and run it to test the website. 
- You can also check inside the container; the same code will be available there as well.


| **Terminal**    | **Output**          | **Output**          |
|--------------------------------|------------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20091918.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20091530.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20091736.png?raw=true) |


---

<a id="example-7"></a>

### ex 2. Named Volume

> We have already completed the Docker setup. You can refer to the previous information directory.


A Named Volume is a Docker-managed persistent storage.<br>
Unlike bind mounts, we do not specify a host path. Docker automatically creates and manages the storage location.

1. Login to the EC2 instance (host/server) using the terminal.<br>
2. If any old containers are running, stop and remove them.<br>
3. Create a Docker named volume. Docker will manage the storage location automatically.<br>

```
docker volume create vol1
```

>The `-v` option is used to mount a Docker-managed named volume into the container.
No host path is required.

Run the Nginx container with named volume mapping:

```
docker run -d -p 80:80 --name mynginx -v vol1:/usr/share/nginx/html nginx
```
The container is now running and ready.

4. Access the application using the EC2 `< public IP >` in the browser.<br>
   You may see the default Nginx page.<br>
   This example shows how Docker named volumes persist data outside the container.


- Now, we will modify the content stored inside the named volume.
- You can also check inside the container; the same content will be available there.


```
cd /var/lib/docker/volumes/vol1/_data
ls
nano index.html
```

> You can also edit files from inside the container if required.

```
docker exec -it mynginx /bin/bash
cd /usr/share/nginx/html
ls
```
✔ Data is stored in a Docker-managed location<br>
✔ Data remains even if the container is removed


Key Points (Named Volume)

- No host path required<br>
- Fully managed by Docker<br>
- Data persists after container deletion<br>
- Recommended for production workloads

#### One-Line Difference (Bind vs Named)

> Bind Volume uses a host path, while Named Volume is fully managed by Docker and is preferred for production use.

```
Bind Volume  → Host path is manually specified
Named Volume → Docker manages the storage location
```

<p align="center">
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20095552.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

 <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20094343.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>
---

<a id="example-8"></a>

### ex 3. Anonymous Volume

An Anonymous Volume is a Docker-managed volume without a user-defined name.<br>
Docker automatically creates it and assigns a random ID.

Run Nginx container with Anonymous Volume

```
docker run -d -p 80:80 -v /usr/share/nginx/html nginx
```
Explanation:

- No volume name is provided<br>
- Docker automatically creates an anonymous volume<br>
- Volume is mounted to `/usr/share/nginx/html`

  Key Points (Anonymous Volume)

- No name is provided by the user<br>
- Docker assigns a random volume ID<br>
- Managed by Docker<br>
- Hard to track and manage<br>
- Not recommended for production

  <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/aaaaa/Screenshot%202026-01-06%20095725.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

---

<a id="example-9"></a>

#  Dockerfile 🐳

A Dockerfile is a plain text configuration file that contains a set of instructions used to build a Docker image.<br>
It tells Docker how to create an image step by step, starting from a base environment and adding everything an application needs to run.

#### Core Idea

> Dockerfile = Blueprint of a Docker Image

Just like:

- A recipe tells how to cook a dish 🍳<br>
- A Dockerfile tells Docker how to build an image 🐳

### In simple words

👉 Dockerfile = recipe<br>
👉 Docker image = cooked food<br>
👉 Docker container = serving the food

---

### Why do we need a Dockerfile?

- To automate image creation
- To make applications portable
- To ensure same environment everywhere
- To avoid manual setup

---

### Common instructions in a Dockerfile

| Instruction | Purpose                   |
| ----------- | ------------------------- |
| `FROM`      | Base image (OS / runtime) |
| `WORKDIR`   | Set working directory     |
| `COPY`      | Copy files into image     |
| `RUN`       | Execute commands          |
| `EXPOSE`    | Open a port               |
| `CMD`       | Start application         |
|  `COPY`     | copy files from host -> container |
|  `ADD`      | Copy files from internet -> container |
|  `ENTRYPOINT` | contains all the commands  |


| ENTRYPOINT vs CMD |  | COPY vs ADD |  |
|------------------|--|------------|--|
| **ENTRYPOINT**   | **CMD** | **COPY** | **ADD** |
| Defines the main command (executable) of the container | Default arguments or default command | Copy file from host to container | Copy file from host or internet to container |
| Commands are written in ENTRYPOINT (WHAT to run) | Arguments usually written in CMD (HOW to run) | Simpler, more predictable | Can do more complex tasks (e.g., unpack archives) |
| Cannot be overridden easily | CMD can be overridden while running the container | Does NOT unpack archives automatically | Can unpack local .tar, .tar.gz, .tar.xz automatically |
| Runs first and always executes when container starts | Runs after ENTRYPOINT (if ENTRYPOINT exists) | Cannot fetch files from URLs | Can fetch remote files from internet |
| Only ONE ENTRYPOINT should exist | Multiple CMD can exist but only the LAST CMD works | Preferred for most use-cases | Use only when you need archive extraction or remote download |
| ENTRYPOINT is NOT compulsory (optional) | CMD is IMPORTANT (without CMD container may stop immediately) | Simpler syntax, faster build | Slightly slower due to extra features |
| ENTRYPOINT gives fixed behavior | CMD gives flexible behavior | Example: `COPY ./app /usr/src/app` | Example: `ADD app.tar.gz /usr/src/app` |
| Example: `ENTRYPOINT ["nginx"]` | Example: `CMD ["-g","daemon off;"]` | Example: `COPY ./index.html /usr/share/nginx/html/` | Example: `ADD https://example.com/file.txt /usr/src/file.txt` |
| `docker run myimage` → nginx -g daemon off | `docker run myimage /bin/sh` → CMD overridden | - | - |



---

### Simple Dockerfile example


```
FROM ubuntu
RUN apt update -y
RUN apt install nginx -y
WORKDIR /var/www/html/
RUN touch index.html
RUN echo "<h1>This is my first Dockerfile</h1>" > index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

Running Command of this file (Img Building Process)
```
docker build -t nginx-ubuntu .   # . for present path ( dir)
```
After creating the Dockerfile, to check which image was used for the container and how much size it consumes, we use this command.
```
docker history nginx-ubuntu
```



🔹 This Dockerfile:

- Uses Alpine Linux as a lightweight base image<br>
- Installs nginx using Alpine package manager `(apk)`<br>
- Creates required nginx runtime directory<br>
- Sets working directory to nginx website root<br>
- Creates a simple HTML file with custom content<br>
- Exposes port 80 for web access<br>
- Runs nginx in foreground to keep container alive

---

### Dockerfile workflow

```
Dockerfile → docker build → Docker Image → docker run → Container
```



| **Terminal**    | ****          | 
|--------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20220634.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20220647.png?raw=true) |



| **Terminal**    | **Output ( Browser )**          | 
|--------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20220723.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20190636.png?raw=true) |


---

### Difference between Dockerfile, Docker Image, and Docker Container

| Aspect             | Dockerfile                                            | Docker Image                                   | Docker Container                     |
| ------------------ | ----------------------------------------------------- | ---------------------------------------------- | ------------------------------------ |
| Definition         | A text file containing instructions to build an image | A read-only template created from a Dockerfile | A running instance of a Docker image |
| Nature             | Configuration / Blueprint                             | Static                                         | Dynamic (running)                    |
| Purpose            | Tells Docker **how to build** an image                | Provides everything needed to run an app       | Actually **runs** the application    |
| State              | Not executable                                        | Not running                                    | Running or stopped                   |
| Mutability         | Editable text file                                    | Immutable (cannot be changed)                  | Mutable during runtime               |
| Created By         | Written by developer                                  | Built using `docker build`                     | Created using `docker run`           |
| Used For           | Image creation                                        | Container creation                             | Application execution                |
| Dependency         | Independent                                           | Depends on Dockerfile                          | Depends on Image                     |
| Storage            | Stored as `.Dockerfile` / `Dockerfile`                | Stored in Docker local repo / registry         | Stored in Docker runtime memory      |
| Lifecycle          | Until modified by developer                           | Exists until deleted                           | Starts, stops, restarts, removed     |
| Versioning         | Versioned via Git                                     | Versioned via tags                             | Not versioned                        |
| Example            | Instructions like `FROM`, `RUN`, `CMD`                | `nginx:latest`                                 | Running Nginx server                 |
| Real-world Analogy | Recipe 📄                                             | Dish ingredients + prep 🍱                     | Served dish 🍽️                      |


- Dockerfile → How to build<br>
- Docker Image → What to run<br>
- Docker Container → Running application
---


### 🔹 Scenario-Based Example

#### Scenario:

We have created a Dockerfile that shows this output in the browser:
```
this is my first docker file
```

Now, we want to change this content.

---

### 🔹 Two Possible Ways to Change the Content


❌ Way 1: Change content inside the container<br>
We can go inside the running container and modify the file manually.

But this is not recommended because:

- The change is temporary
- If the container is deleted or recreated, the content is lost
- This breaks the concept of immutability
- It will also cause problems during rollback.
- If changes are made inside a container, it is not practical to go into 100 containers and update each one manually.

---

✅ Way 2: Change content from the Dockerfile (Recommended)<br>
Correct Approach:<br>
We should change the content inside the Dockerfile and rebuild the image.

Why this is correct:

-Dockerfile is the source of truth
- Image will be reproducible
- Changes are permanent
- Follows Docker best practices





| **Terminal**    | ****          | 
|--------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20220519.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20220537.png?raw=true) |



 <p align="center">
  <img src="https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-07%20192408.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

---
---

<a id="example-10"></a>

# Dockerfile Optimization 

- Dockerfile optimization is the process of writing a Dockerfile in a clean and efficient way so that the Docker image is lightweight, fast, and secure.  
- It focuses on reducing unnecessary layers, removing unused packages, and using minimal base images.  
- An optimized Dockerfile contains only what is required to run the application.  
- This helps Docker containers run efficiently in development as well as production environments.  
- Dockerfile optimization is an important concept in DevOps and container-based applications.

---

## Why should we minimize a Dockerfile?

- Docker image size becomes smaller<br>
- Image build time becomes faster<br>
- Security improves<br>
- Less storage and bandwidth usage<br>
- Follows Docker best practices for production

## Why Alpine Linux is commonly used for Dockerfile Optimization?

- Alpine Linux is a very lightweight Linux distribution.
- Its base image size is very small (around 5–7 MB).
- It reduces the overall Docker image size significantly.
- Faster image pull and build time compared to other OS images.
- Uses `apk` package manager which is simple and efficient.
- Ideal for microservices, containers, and cloud-native applications.

---

### Advantages of Dockerfile Optimization

- **Smaller Image Size:** Optimized images are lightweight, which saves storage and bandwidth.  
- **Faster Builds & Pulls:** Less data to download and build means faster container startup.  
- **Better Security:** Fewer packages and layers reduce the attack surface.  
- **Easier Maintenance:** Clean Dockerfiles are easier to read and maintain.  
- **Production Ready:** Optimized Dockerfiles perform well in production environments.

---

### Disadvantages / Limitations

- **Extra Effort Required:** Writing optimized Dockerfiles takes more thought and planning.  
- **Less Flexibility:** Minimal images may lack utilities or debugging tools by default.  
- **Manual Installation Needed:** Extra tools or software are not pre-installed, so you need to install them manually if required.  
- **Learning Curve:** Beginners may find optimization techniques (multi-stage builds, caching, etc.) slightly complex.  
- **Not Always Necessary:** For small projects or quick prototypes, optimization may not provide noticeable benefits.

---

## Ways to Minimize a Dockerfile

---

### 1. Use a Small Base Image

Always prefer lightweight base images instead of full OS images.

❌ Bad Practice

```
FROM ubuntu
```

✅ Good Practice
```
FROM alpine
```


---

### 2. Install Only Required Packages

Do not install unnecessary packages inside the container.

❌ Bad Practice

```
RUN apt update && apt install -y vim curl git
```

✅ Good Practice

```
RUN apk add --no-cache curl
```


---

### 3. Combine Multiple RUN Commands

Each RUN command creates a new layer.  <br>
Combine commands to reduce layers.

❌ Bad Practice

```
RUN apk update
RUN apk add nginx
RUN mkdir -p /run/nginx/
```


✅ Good Practice
```
RUN apk update && apk add nginx && mkdir -p /run/nginx/
```





---

### 4. Avoid Unnecessary Files and Tools

Do not install editors or debug tools inside production images.

- No vim<br>
- No nano<br>
- No extra utilities

This keeps the image lightweight and secure.

---

### 5. Set Correct WORKDIR

Using WORKDIR avoids unnecessary `cd` commands and keeps Dockerfile clean.

✅ Example

```
WORKDIR /usr/share/nginx/html/
```


---

### 6. Prefer Simple CMD or ENTRYPOINT

Run only the required service in foreground mode.

✅ Example

```
CMD ["nginx", "-g", "daemon off;"]
```


---

## Lightweight Nginx Dockerfile (Alpine Based)

Below is an optimized and lightweight Dockerfile using Alpine and Nginx:

```
FROM alpine
RUN apk update && apk add nginx && mkdir -p /run/nginx/
WORKDIR /usr/share/nginx/html/
RUN echo "<h1>This is my lightweight nginx</h1>" > index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```

---

## Explanation of the Above Dockerfile

- `FROM alpine`  <br>
  Uses a very small base image to reduce size.

- `RUN apk update && apk add nginx` <br> 
  Installs only nginx, no extra packages.

- `mkdir -p /run/nginx/`<br>  
  Creates required runtime directory for nginx.

- `WORKDIR /usr/share/nginx/html/` <br> 
  Sets default directory for website files.

- `RUN echo "<h1>...</h1>" > index.html`  <br>
  Creates a sample HTML page directly inside the image.

- `EXPOSE 80`  <br>
  Informs Docker that the container listens on port 80.

- `CMD ["nginx", "-g", "daemon off;"]`  <br>
  Runs nginx in foreground so the container stays alive.

---

## Result After Optimization

- Very small image size<br>
- Fast build time<br>
- Simple and clean Dockerfile<br>
- Lightweight nginx container<br>
- Suitable for learning and demos

---

## Conclusion

This Dockerfile demonstrates how to:<br>
- Use Alpine Linux<br>
- Reduce image size<br>
- Avoid unnecessary layers<br>
- Create a lightweight web server container

This approach is widely used in real DevOps and production environments.

---





## Some More Reason's









### 7. Use Multi-Stage Builds (Very Important)

Build tools should not be part of the final image.

❌ Single Stage Build

```
FROM node
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "app.js"]
```
✅ Multi-Stage Build

```
FROM node AS build
WORKDIR /app
COPY . .
RUN npm install

FROM node:alpine
WORKDIR /app
COPY --from=build /app /app
CMD ["node", "app.js"]
```


---

### 8. Clean Cache and Temporary Files

Remove cache files to reduce image size.

❌ Bad Practice

```
RUN apt install -y nginx
```

✅ Good Practice

```
RUN apt install -y nginx && rm -rf /var/lib/apt/lists/*
```


---

### 9. Copy Files in Correct Order (Docker Cache Optimization)

Copy files that change less frequently first.

❌ Bad Practice

```
COPY . .
RUN npm install
```

✅ Good Practice

```
COPY package.json .
RUN npm install
COPY . .
```


---
---

<a id="example-11"></a>

# Database Container 

>– Important Information & Best Practices

A database container is a Docker container that runs a database service such as MySQL, PostgreSQL, or MongoDB.  <br>
Databases are critical components, and special care must be taken while containerizing them.

---

## Very Important Rule: Do NOT Store Database Credentials in Dockerfile

While writing a Dockerfile for a database, **it is NOT mandatory and NOT recommended** to write the database username and password inside the Dockerfile.

### Why is this a very bad practice?

- Dockerfile is part of source code and can be pushed to GitHub or shared. <br>
- Anyone who gets access to the Dockerfile can see the database ID and password. <br>
- If credentials are exposed, anyone can enter the database. <br>
- This can lead to data theft, data loss, or security breaches. <br>
- In real production environments, this is considered a serious security issue.

---

## Correct Approach: Pass Credentials at Runtime Using `-e`

Instead of hardcoding credentials in the Dockerfile,   <br>
we pass database credentials **at container runtime** using environment variables (`-e`).

This way: <br>
- Credentials are not stored in the image <br>
- Dockerfile remains clean and secure <br>
- Different environments (dev, test, prod) can use different credentials

---

## Example: MySQL Database Container (Best Practice)

### Dockerfile (No ID / Password Stored)

```
FROM mysql:8.0
EXPOSE 3306
CMD ["mysqld"]
```

👉 Note:  
No username or password is written inside the Dockerfile.

---

### Running the Database Container (Passing Credentials Securely)

```
docker run -d -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=myntra -p 3306:3306 --name mysql-db mysql
```


---

## Explanation of the Runtime Command

- `-e MYSQL_ROOT_PASSWORD=root`<br>  
  Sets the MySQL root password at runtime.

- `-e MYSQL_DATABASE=myntra`  <br>
  Automatically creates a database named `myntra`.

- `-p 3306:3306`  <br>
  Exposes MySQL port to the host machine.

- `mysql:8.0`  <br>
  Uses the official MySQL image.

---

## Additional Best Practices for Database Containers

- Always use **official database images** from Docker Hub.<br>
- Never hardcode passwords in Dockerfile or source code.<br>
- Use environment variables or secret managers.<br>
- Use Docker volumes to persist database data.<br>
- Restrict database port access in production.

---

## Conclusion

When creating database containers:<br>
- Writing ID and password in Dockerfile is **not mandatory**<br>
- It is a **bad and insecure practice**<br>
- Always pass credentials at runtime using `-e`<br>
- This approach is secure, flexible, and industry standard<br>

This method is widely used in real-world DevOps and production environments.

---

>Note:
Here, we are storing database credentials only to understand how it works and how containers behave.<br>
This is done purely for study and learning purposes.<br>
Do not use this approach in real-world projects or production environments, as storing credentials like this is a bad security practice.


```
FROM mysql
ENV MYSQL_ROOT_PASSWORD=root
ENV MYSQL_DATABASE=myntra
EXPOSE 3306
CMD ["mysqld"]
```

| **Terminal**    | ****          | 
|--------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-11%20004527.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-11%20004545.png?raw=true) |


| ****    | ****          | 
|--------------------------------|------------------------------------|
| ![VS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-11%20004608.png?raw=true) | ![AWS](https://github.com/nikiimisal/Docker-Examples-and-Concepts/blob/main/img/Screenshot%202026-01-11%20004629.png?raw=true) |


---































































