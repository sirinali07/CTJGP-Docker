

# Multi Stage Build Lab

## Overview

In this lab, you will create a simple **Node.js Express application**, containerize it using **Docker**, and run it as a container.

The application displays a welcome message along with the current **date and time** and provides **health check endpoints**.

---


# Step 1: Create Project Directory

```bash
mkdir node-docker-lab
cd node-docker-lab
```

---

# Step 2: Create `index.js`

Create the application file.

```bash
vi index.js
```

Add the given content, by pressing `INSERT`

```javascript
const port = process.env.PORT || 8080;
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  const currentDate = new Date();
  const dateTime = currentDate.toLocaleString();

  res.send(`
    <div style="font-family: Arial, sans-serif; text-align: center; padding: 50px;">
      <h1 style="color: dodgerblue; font-size: 50px;">
        Hello-World-777!
      </h1>
      <p style="color: gold; font-size: 20px;">
        Current Date and Time: ${dateTime}
      </p>
    </div>
  `);
});

app.get('/readiness', (req, res) => res.send('Ready !!'));

app.get('/liveness', (req, res) => res.send('Live !!'));

app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```
save the file using `ESCAPE + :wq!`
---

# Step 3: Create `package.json`

Create the package file.

```bash
vi package.json
```


Add the given content, by pressing `INSERT`

```json
{
  "name": "example-app",
  "version": "1.0.0",
  "description": "A simple Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
save the file using `ESCAPE + :wq!`
---

# Step 4: Create `Dockerfile`

Create the Dockerfile.

```bash
vi Dockerfile
```

Add the given content, by pressing `INSERT`

```dockerfile
# Stage 1: Build
FROM node:18 AS builder
WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker cache
COPY package.json ./

# Install dependencies
RUN npm install --only=production

# Copy the entire source code
COPY . .

# Stage 2: Run (Alpine)
FROM alpine:3
WORKDIR /app

# Install Node.js in Alpine
RUN apk add --no-cache nodejs

# Copy built files from the builder stage
COPY --from=builder /app .

# Expose the application port
EXPOSE 8080

# Run the Node.js application
CMD ["node", "index.js"]
```
save the file using `ESCAPE + :wq!`
---

# Step 5: Build Docker Image

Build the Docker image.

```bash
docker build -t node-docker-lab .
```

---

# Step 6: Run the Container

Run the container and expose port **8080**.

```bash
docker run -d -p 8080:8080 --name node-app node-docker-lab
```

---

# Step 7: Verify the Application

Open the browser:

```
http://localhostIP:8080
```

You should see:

```
Hello-World-777!
Current Date and Time
```

---



# Step 8: View Logs

```bash
docker logs node-app
```

---

# Step 9: Stop and Remove Container

```bash
docker stop node-app
docker rm node-app
```

---

# Learning Objectives

After completing this lab, you will understand:

* How to create a **Node.js** application
* How to write a **Dockerfile**
* How to build **Docker images**
* How to run containers
