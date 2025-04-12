
# Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)

## Overview

This project demonstrates how to automate the deployment of a Node.js application using a CI/CD pipeline with GitHub Actions and Docker. It is part of a DevOps internship task aimed at showcasing continuous integration and continuous deployment workflows.

---

## Project Structure

```
├── app.js               # Node.js application entry point
├── package.json         # Project dependencies and metadata
├── Dockerfile           # Docker configuration to build the image
└── .github
    └── workflows
        └── main.yml     # GitHub Actions workflow for CI/CD
```

---

## Technologies Used

- Node.js
- Docker
- GitHub Actions
- YAML (workflow configuration)

---

## Application Details

### app.js

A simple Node.js HTTP server:

```js
const http = require('http');
const PORT = process.env.PORT || 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.end('Hello from CI/CD Node App!');
});

server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

---

### Dockerfile

Defines steps to build and run the app inside a Docker container:

```dockerfile
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

---

### GitHub Actions Workflow

The `main.yml` workflow triggers on every push to the `main` branch, builds the Docker image, and runs it:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Build Docker image
        run: docker build -t node-ci-cd-app .

      - name: Run Docker container
        run: docker run -d -p 3000:3000 node-ci-cd-app
```

---

## Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```

2. **Ensure your app works locally using Docker:**
   ```bash
   docker build -t node-ci-cd-app .
   docker run -d -p 3000:3000 node-ci-cd-app
   ```

3. **Create GitHub Actions workflow file:**
   ```bash
   mkdir -p .github/workflows
   touch .github/workflows/main.yml
   ```

4. **Commit and push to GitHub:**
   ```bash
   git add .
   git commit -m "Set up CI/CD pipeline"
   git push origin main
   ```

5. **Monitor pipeline on GitHub:**  
   Navigate to the **Actions** tab in your repository.

---

## Optional: Push to Docker Hub

Add the following steps to your workflow if you want to push the image to Docker Hub:

```yaml
- name: Log in to Docker Hub
  uses: docker/login-action@v2
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}

- name: Build and Push to Docker Hub
  uses: docker/build-push-action@v4
  with:
    context: .
    push: true
    tags: yourdockerhubusername/node-ci-cd-app:latest
```

Store Docker credentials as secrets in your GitHub repository.

---

## Conclusion

This project showcases a streamlined and automated approach to application deployment using modern DevOps practices. It can serve as a foundation for more advanced CI/CD workflows including testing, security scanning, and multi-environment deployments.

---
## Author
 Srikanth Berla
