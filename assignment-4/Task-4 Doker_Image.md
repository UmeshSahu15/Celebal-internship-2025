# Week 4 – Docker Basics: Task 4

## Task: Create a Docker Image Using Multiple Methods

## Objective

The goal of this task was to learn how to create Docker images using more than one approach, primarily using a `Dockerfile`, and also by committing from a running container. I wanted to understand both the declarative and manual ways of image creation, and how they fit into real-world workflows.

---

## Step 1: Create a Docker Image Using a Dockerfile

This method is the most common and automated way to build Docker images. I followed a similar pattern as the previous task, but the idea was to get more comfortable with customizing `Dockerfiles` and understanding what’s really happening at each layer.

### 🔹 Set Up Project Directory

```bash
mkdir task3-dockerfile-image
cd task3-dockerfile-image
```

![Screenshot 2025-06-28 230440](https://github.com/user-attachments/assets/6e6b4841-20a8-4544-a29a-f0b1d027345b)


This is where I placed my app files and the Dockerfile. I like keeping things organized in separate folders for each image or container.

### 🔹 Create a Basic Node.js App

I created a simple server again to keep things familiar. Here's the `index.js` file:

```bash
const http = require('http');

const server = http.createServer((req, res) => {
  res.end("Image created using Dockerfile method.");
});

server.listen(8080, () => {
  console.log("Server is running on port 8080");
});
```

The only change here is the port and response message — just to make it clear which image was built.

![Screenshot 2025-06-28 230454](https://github.com/user-attachments/assets/6f7be3f4-248c-43f1-b76f-5474db0bb960)


### 🔹 Write the Dockerfile

Created a Dockerfile for Sample Nodejs App

```Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 8080

CMD ["node", "index.js"]
```

Here’s a quick breakdown:

* `FROM node:18-alpine`: Lightweight and efficient Node.js base image.
* `WORKDIR /app`: All operations from here onward happen in `/app`.
* `COPY . .`: Copies all files into the container.
* `RUN npm install`: Installs dependencies (though none for now).
* `EXPOSE 4000`: This is just for documentation — it doesn’t actually publish the port.
* `CMD`: This tells Docker what command to run when the container starts.

![Screenshot 2025-06-28 230502](https://github.com/user-attachments/assets/6c67e364-6aed-49be-8dd0-5f4fadf09a79)


### 🔹 Build and Run the Docker Image

```bash
docker build -t csi-dockerfile-method:v1 .
docker run --name csi-dockerfile-container -d -p 8080:8080 csi-dockerfile-method:v1
```

![Screenshot 2025-06-28 230509](https://github.com/user-attachments/assets/5f4a2136-eda9-4885-bb5f-1288d8179790)

This built the image and ran it successfully. I opened my browser at `http://<vm-ip>:8080` and saw the custom message from the server a great sign that the image works exactly as expected.

![Screenshot 2025-06-28 230518](https://github.com/user-attachments/assets/28cbdd5a-1497-4970-aeba-00baea39f996)


---

## Step 2: Create a Docker Image from a Running Container

This method is more manual, but it's useful when we want to experiment interactively, then save work as an image.

### 🔹 Start a Base Container

```bash
docker run -it node:18-alpine sh
```

This opened a terminal inside a container running the base Node image. From here, I manually created and ran a mini Node.js app.

### 🔹 Inside the Container

I manually did the following:

```bash
mkdir /csi-app
cd /csi-app
```

![Screenshot 2025-06-28 230525](https://github.com/user-attachments/assets/55c90c3b-3e09-4781-8083-9c98eca220cd)


Then I used `vi` to create `index.js` (or used `echo` for simpler editing):

```bash
const http = require("http");

const server = http.createServer((req, res) => {
  res.end("Image from running container method.");
});

server.listen(3000, () => { 
  console.log("Server running on port 3000"); 
});
```

![Screenshot 2025-06-28 230534](https://github.com/user-attachments/assets/935631e5-4f4d-499e-86fe-4c744af173e4)


Then initialized the app and installed Node.js dependencies:

```bash
npm init -y
```
And finally ran the app:

```bash
node index.js
```

![Screenshot 2025-06-28 230541](https://github.com/user-attachments/assets/8237ad3b-2bd7-4180-bb29-a6eedf776c98)


Once I confirmed it worked inside the container, I exited:

```bash
ctrl p + ctrl q
```

### 🔹 Commit the Container as an Image

First, I found the container ID:

```bash
docker ps
```

Then committed it:

```bash
docker commit  --change='WORKDIR /csi-app' --change='CMD ["node", "index.js"]' 479ff06c0ed5 csi-manual-image:v1
```

- `--change='WORKDIR /csi-app':` Sets the working directory inside the new image to /csi-app, which is where I had placed my index.js file inside the container.
- `--change='CMD ["node", "index.js"]':` Sets the default command to run when a container is started from this image — in this case, it will launch the Node.js app.

This way, I make sure that the image starts correctly without having to manually change directories or specify the run command every time.

![Screenshot 2025-06-28 230548](https://github.com/user-attachments/assets/33db04ea-6a93-4985-8731-880fdd99bfc4)



Now I had a new image built entirely from a running container, without writing a Dockerfile.

### 🔹 Run the Committed Image

```bash
docker run --name csi-manual-container -d -p 3000:3000 csi-manual-image:v1
```

![Screenshot 2025-06-28 230556](https://github.com/user-attachments/assets/9be6196b-66c0-46bd-a822-34cd3a44347d)


I accessed it via `http://98.70.42.63:3000/` and saw the custom message — success again!

![Screenshot 2025-06-28 230604](https://github.com/user-attachments/assets/e01e7444-8ee9-42d2-a901-b6b8614d8888)


---

## Conclusion

This task gave me a solid understanding of two ways to create Docker images — using a Dockerfile and committing from a running container. The Dockerfile method felt clean and ideal for repeatable setups, while the manual method was useful for quick, interactive builds. Both approaches were straightforward once I understood the flow. It was great to see my app running successfully from images built in completely different ways.

---
