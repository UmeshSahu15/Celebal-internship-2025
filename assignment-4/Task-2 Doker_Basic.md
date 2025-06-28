
# 🐳 Week 4 – Docker Basics: Task 2

## 📌 Task: Docker Installation, Basic Container Operations & Building a Custom Image

## 🎯 Objective

This task focused on hands-on installation of Docker, running basic containers, and building a custom Docker image using a Dockerfile. The goal was to gain confidence working with Docker as a foundational DevOps skill.

---


## Step 1: Installing Docker on Ubuntu

I began with a clean Ubuntu 20.04 virtual machine. Since Docker runs natively on Linux, I figured this was the most realistic environment to practice.

Here’s how I installed Docker, step by step:

### 🔹 Update the System

```bash
sudo apt update -y
```

I ran this to make sure my package list was up to date. It ensures I’m downloading the latest versions of everything in the upcoming steps.

### 🔹 Install Dependencies

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
These packages are needed to:

* Handle HTTPS-based repositories
* Add external Docker sources securely
* Use tools like curl to fetch remote files

![Screenshot 2025-06-28 225351](https://github.com/user-attachments/assets/5d16bb2f-5df4-4637-8986-ca03e0741a21)


### 🔹 Add Docker’s GPG Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Docker signs its packages with a GPG key for security. This command downloads that key and stores it safely, so Ubuntu knows it's getting trusted software.

### 🔹 Add Docker’s Official Repository

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Here, I’m adding Docker’s official software repository to my system. This ensures I’m installing Docker straight from the source — not from Ubuntu’s default (and possibly outdated) repositories.

![Screenshot 2025-06-28 225400](https://github.com/user-attachments/assets/7181531c-d868-44b1-8fdd-5f00db98ca88)


### 🔹 Install Docker Engine

```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

I ran `apt update` again to refresh the package list now that Docker’s repo is added. Then I installed:

* `docker-ce`: the Docker engine itself
* `docker-ce-cli`: the command-line interface
* `containerd.io`: the runtime that actually runs containers under the hood

![Screenshot 2025-06-28 225410](https://github.com/user-attachments/assets/4a3952d9-5444-4dab-96bb-88fe86c96603)

### 🔹 Verify Docker Installation

```bash
docker --version
```
This command checks if Docker is installed correctly and prints out the version number.

![Screenshot 2025-06-28 225421](https://github.com/user-attachments/assets/265fd52c-2a64-4edf-ac55-ccf9266e3038)

It was great to see Docker installed and ready to go!

### 🔹 Start and Enable Docker

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

* The first command starts Docker right away.
* The second ensures Docker starts automatically every time the system boots up.

![Screenshot 2025-06-28 225430](https://github.com/user-attachments/assets/b3c48ce5-f3ef-4336-8de7-9389d11b10eb)

### 🔹 Run Docker Without sudo

```bash
sudo usermod -aG docker $USER
newgrp docker
```

By default, Docker needs root privileges. These commands add my user to the Docker group, allowing me to run Docker commands without `sudo`. I used `newgrp docker` to apply the group change immediately without needing to log out and log back in.

![Screenshot 2025-06-28 225442](https://github.com/user-attachments/assets/9041f44c-7f47-4088-a75e-8cda168373a8)

---

## Step 2: Running Basic Docker Commands

With Docker installed, I moved on to testing it by running a container.

### 🔹 Pull the NGINX Image

```bash
docker pull nginx
```

This command downloaded the official NGINX web server image from Docker Hub. It’s like getting a ready-to-use NGINX server in a box.

![Screenshot 2025-06-28 225451](https://github.com/user-attachments/assets/67b94a3d-e3e3-4f58-b104-1b6592510667)


### 🔹 Run the NGINX Container

```bash
docker run -d --name csi-nginx -p 80:80 nginx
```

Let me break this down:

* `-d`: Runs the container in the background (detached mode)
* `--name mynginx`: Names the container so it’s easier to manage
* `-p 80:80`: Maps port 80 inside the container to port 8080 on my local machine
* `nginx`: The image to run

![Screenshot 2025-06-28 225501](https://github.com/user-attachments/assets/b704cfda-c26d-49f9-8fbc-cbc9535351d1)


After running this, I opened my browser at [http://20.244.3.123:80/](http://20.244.3.123:80/) and saw the NGINX welcome page — success!

![Screenshot 2025-06-28 225511](https://github.com/user-attachments/assets/12627be7-4a29-40e3-bd38-375d3b17a6b8)


### 🔹 Interacting with the Container

```bash
docker ps
```

This listed the running containers. I used it to confirm that `csi-nginx` was active.

![Screenshot 2025-06-28 225521](https://github.com/user-attachments/assets/b2413898-d5dd-4603-856c-b22d7eb5dd04)


```bash
docker logs csi-nginx
```

This showed the logs from the NGINX container — super useful for checking what’s going on inside.

![Screenshot 2025-06-28 225531](https://github.com/user-attachments/assets/fbf2558f-c1fc-4017-8cb9-bfaaa89b2f67)



```bash
docker exec -it csi-nginx bash
```

This opened a shell inside the container. It felt like opening a tiny, isolated Linux machine — very cool!

![Screenshot 2025-06-28 225542](https://github.com/user-attachments/assets/75f73752-8d85-436b-a9d6-eb4253b75cbc)



```bash
docker stop csi-nginx
docker rm csi-nginx
```

These two commands stopped and removed the container once I was done experimenting with it.

![Screenshot 2025-06-28 225550](https://github.com/user-attachments/assets/391cb26a-f83c-4c43-a163-c8b88087a997)

---

## Step 3: Building a Custom Docker Image

Now that I was comfortable running containers, I wanted to create my own image, which is a core skill in DevOps workflows.

### 🔹 Create a Project Directory

```bash
mkdir csi-docker-app
cd csi-docker-app
```

This was my workspace for the app and Dockerfile.

![Screenshot 2025-06-28 225559](https://github.com/user-attachments/assets/6f75decb-c9f3-42b6-b09b-e163a8698241)


### 🔹 Write a Simple Node.js App

I created a file called `index.js` with this content:

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.end("Hello I am from custom Docker image!");
});

server.listen(3000, () => {
  console.log("Server running");
});
```
This is a basic Node.js web server. It responds with a message on every request.

![Screenshot 2025-06-28 225609](https://github.com/user-attachments/assets/68d08571-2aa4-46fe-9258-7741fb626ac3)

### 🔹 Create the Dockerfile

I created a file named `Dockerfile` in the same directory:

```Dockerfile
FROM node:18-alpine

WORKDIR /app
COPY . .
RUN npm install

EXPOSE 3000
CMD ["node", "index.js"]
```

Here’s what each line means:

* `FROM node:18-alpine`: Use a lightweight Node.js base image
* `WORKDIR /app`: Set the working directory inside the image
* `COPY . .`: Copy all files from my local directory into the image
* `RUN npm install`: Install dependencies (none for now, but this would cover future cases)
* `EXPOSE 3000`: Document that the app runs on port 3000
* `CMD ["node", "index.js"]`: The default command to run when a container starts

![Screenshot 2025-06-28 225618](https://github.com/user-attachments/assets/d95259a3-5f5a-4540-8e82-7ab27b220003)


### 🔹 Build and Run the Image

```bash
docker build -t csi-node-app:v1 .
```

This built the image and tagged it as `csi-node-app:v1`.

![Screenshot 2025-06-28 225627](https://github.com/user-attachments/assets/a1cbfe49-6f91-4cd4-8ea4-290d7afa5e6b)

```bash
docker run -d -p 3000:3000 csi-node-app:v1
```

Then I ran the container, mapping the internal port 3000 to my local port 3000.

![Screenshot 2025-06-28 225637](https://github.com/user-attachments/assets/dbe36df1-48f0-4797-860b-116ddfc71a7e)


Visiting [http://20.244.3.123:3000](http://20.244.3.123:3000/) in the browser showed the message from my Node.js app, that was a great!

![Screenshot 2025-06-28 225651](https://github.com/user-attachments/assets/cb466edb-a6f9-407a-9fa9-c99228e9b79f)


---

## Cleanup

To clean up my environment, I ran:

```bash
docker stop <container_id>
docker rm <container_id>
docker rmi my-node-app
```

This stops and removes both the container and the image I created. It's a good habit to avoid clutter and free up space.

![Screenshot 2025-06-28 225702](https://github.com/user-attachments/assets/e98cc627-b9a9-4e6f-a18c-98bf9e7ec1a4)


---

## Conclusion

Working through this task gave me a clear understanding of how Docker works from the ground up. Installing it manually, running containers, and building my own image made the whole concept feel much more practical and real. It was satisfying to see my own app running inside a container, and I now feel more comfortable using Docker in everyday development or deployment work. This was a solid and valuable hands-on experience.

---
