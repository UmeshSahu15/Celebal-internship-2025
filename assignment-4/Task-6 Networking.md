# 🐳 Week 4 – Docker: Task 6

## 📌 Task: Create a Custom Docker Bridge Network (Advanced Real-World Simulation)

## 🎯 Objective

This task wasn’t just about creating a basic Docker bridge network. I wanted to simulate a **real-world microservices environment** with multiple containers (an API and a MongoDB database) communicating over a custom network. This mirrors how services are structured in real enterprise setups.

---

## Step 1: Create a Custom Bridge Network

I started by creating a custom bridge network. Instead of just letting Docker manage everything, I defined the subnet and gateway myself. This gives more control over IP ranges and can be useful in larger setups.

```bash
docker network create \
  --driver bridge \
  --subnet 172.1.0.0/16 \
  --gateway 172.1.0.1 \
  csi-microservices-net
```

![Screenshot 2025-06-28 232934](https://github.com/user-attachments/assets/3c0db4c0-8ac0-4f2b-96bb-80d04152d04b)


I verified the network was created properly with:

```bash
docker network inspect csi-microservices-net
```

![Screenshot 2025-06-28 232944](https://github.com/user-attachments/assets/7cb47fc0-ef4d-4588-8ee9-3774872e0d13)


Everything looked good. The custom network was in place.

---

## Step 2: Launch MongoDB on the Custom Network

Next, I deployed a MongoDB container that would act as the database. I attached it directly to the custom network:

```bash
docker run -d \
  --name csi-mongo \
  --network csi-microservices-net \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=csi@123 \
  mongo:6
```

Using the `--network` flag ensured this container would only be reachable from other containers on the same network which is perfect for internal service communication.

![Screenshot 2025-06-28 233001](https://github.com/user-attachments/assets/a783927f-40ac-4ec5-bb2f-9a48c71af678)


I confirmed it with inspect container to make sure it was connected to the custom network.

```bash
docker inspect csi-mongo
```

![Screenshot 2025-06-28 233035](https://github.com/user-attachments/assets/fca89cf2-2b03-49e9-b45c-df8517c199f4)


---

## Step 3: Build and Run a Node.js API That Talks to MongoDB

To test service-to-service communication, I wrote a simple Node.js Express API that connects to the MongoDB instance and fetches data.

Before writing the API code, I initialized the Node.js project and installed the required packages:

```bash
npm init -y
npm install express mongodb
```

![Screenshot 2025-06-28 233044](https://github.com/user-attachments/assets/cd82e396-ae08-4864-9be1-078152fa4aa1)


Then, I created the main API file:

**index.js:**

```bash
const express = require('express');
const { MongoClient } = require('mongodb');
const app = express();

const client = new MongoClient('mongodb://admin:csi%40123@csi-mongo:27017/?authSource=admin');

app.get('/', async (req, res) => {
  try {
    await client.connect();
    const db = client.db('csidb');
    const employees = await db.collection('employees').find({}).toArray();
    res.send(employees);
  } catch (err) {
    res.status(500).send(err);
  }
});

app.listen(8080, () => console.log('API running on port 8080'));
```

**Dockerfile:**

```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD ["node", "index.js"]
```

![Screenshot 2025-06-28 233052](https://github.com/user-attachments/assets/84dc922d-0fc4-4e7b-ada9-1960683ac0db)

Then I built the Docker image:

```bash
docker build -t csi-node-api .
```
The build completed successfully, and the image was ready to run.

![Screenshot 2025-06-28 233059](https://github.com/user-attachments/assets/922d6896-48ff-4076-bf63-1050c7ab91ca)

---

## Step 4: Deploy the API Connected to the Same Network

I launched the API container and attached it to the same network as MongoDB:

```bash
docker run -d --name csi-api --network csi-microservices-net -p 8080:8080 csi-node-api
```

This way, the API could connect to MongoDB using its container name csi-mongo as the hostname using Docker internal DNS

![Screenshot 2025-06-28 233107](https://github.com/user-attachments/assets/9dcb44a3-c0ef-4e14-a3e3-f587b733b3c7)

Inspect Api Container to verify network

```bash
docker inspect csi-api
```

![Screenshot 2025-06-28 233116](https://github.com/user-attachments/assets/2a85f578-e3ee-4a38-86bd-0389d44eb28a)

---

## Step 5: Verified Internal Container DNS & Connectivity

I jumped into the API container to test the connection manually:

```bash
docker exec -it csi-api sh
```

The ping worked! That confirmed the containers could talk to each other inside the custom network using just container names no IPs needed.

```bash
ping csi-mongo
```

![Screenshot 2025-06-28 233124](https://github.com/user-attachments/assets/d97b8f44-23cd-4e3d-8308-5a1c71d29a3d)


Success! The API also fetched data from MongoDB using internal DNS.

---

## Step 6: API Data Test From Web UI

Once both MongoDB and the Node.js API were running smoothly on the custom Docker network, it was time to test the actual functionality: could the API successfully fetch data from MongoDB and serve it to users via a browser?

### Adding Sample Data to MongoDB

First, I logged into the MongoDB container to manually insert some test data. This simulates what might happen in a real app — having a backend database with employee records.

```bash
docker exec -it csi-mongo mongosh -u admin -p csi@123
```

Once inside the shell, I switched to the database the API expects to connect to:

```bash
use csidb
```

![Screenshot 2025-06-28 233133](https://github.com/user-attachments/assets/d02ce236-903a-4a04-8300-1cacf73647f1)


Then I inserted a list of employee documents:

```bash
db.employees.insertMany([
  { name: "Vikas", csiintern: true, domain: "DevOps", location: "Kakinada, Visakhapatnam" },
  { name: "Lala's Hymavati", csiintern: true, domain: "Cloud Infrastructure", location: "Visakhapatnam" },
  { name: "Prasanthy", csiintern: true, domain: "DevOps", location: "Visakhapatnam" },
  { name: "Greeshma", csiintern: true, domain: "DevOps", location: "Kakinada" },
  { name: "Venkat", csiintern: true, domain: "Python Backend", location: "Tirupati" },
  { name: "Suresh", csiintern: true, domain: "Frontend - React", location: "Kurnool" }
]);
```

![Screenshot 2025-06-28 233143](https://github.com/user-attachments/assets/86accba8-9e5f-4299-9ef7-b01d872af054)

### Viewing the API Response in the Browser

Once the data was in place, I opened my browser and visited the API endpoint: `http://98.70.32.51:8080/`

>This IP points to the host machine running the container, and port 8080 is mapped to the Node.js app inside the container.

The moment the page loaded, it displayed all the employee data that was just added to MongoDB. This confirmed that:

![Screenshot 2025-06-28 233156](https://github.com/user-attachments/assets/83b7c49c-4ccc-4572-9b35-a31662ba207b)



## 🔒 Step 7: Enhanced Security by Removing Default Bridge

Tested by launching an Alpine container with default network, which couldn’t access anything with in custom network:

```bash
docker run -it alpine sh
```

![Screenshot 2025-06-28 233204](https://github.com/user-attachments/assets/5cc12681-4d32-4fa4-9075-92283bb295b6)


---

## conclusion

This task gave me hands-on experience with setting up a real-world microservices environment using Docker. By creating a custom bridge network, I was able to isolate and securely connect multiple containers, just like in production systems. From configuring MongoDB and building a Node.js API, to testing internal connectivity and API responses, every step reinforced how containerized services communicate and scale in a controlled network. It was a solid deep dive into Docker networking and service orchestration.

---
