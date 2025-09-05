# Hands-On DevOps 2025: Node.js App with GitHub Actions, Docker & Kubernetes

## 📑 Table of Contents
- [📋 Prerequisites - Download These First!](#-prerequisites---download-these-first)
- [Step 1: Set Up Git for Version Control](#step-1-set-up-git-for-version-control)
- [Step 2: Build a Node.js Web App](#step-2-build-a-nodejs-web-app)
- [Step 3: Create Proper Tests](#step-3-create-proper-tests)
- [Step 4: GitHub Actions CI/CD Pipeline](#step-4-github-actions-cicd-pipeline)
- [Step 5: Dockerfile](#step-5-dockerfile)
- [Step 6: Essential Config Files](#step-6-essential-config-files)
- [Step 7: Docker Compose](#step-7-docker-compose)
- [Step 8: Test Everything Locally](#step-8-test-everything-locally)
- [Step 9: Deploy to GitHub](#step-9-deploy-to-github)
- [Step 10: Kubernetes Deployment](#step-10-kubernetes-deployment)
- [Step 11: Complete Deployment Workflow](#step-11-complete-deployment-workflow)
- [🚨 Troubleshooting Guide](#-troubleshooting-guide)

---

## 📋 Prerequisites - Download These First!


Before starting, install these tools on your machine:

### Required Downloads:
1. **Node.js (v18 or higher)** - Current LTS: v20.x  
   - [Download here](https://nodejs.org/) (choose LTS 20.x)  
   - Verify:  
     ```bash
     node --version
     npm --version
     ```

2. **Git - Latest stable version**  
   - [Download here](https://git-scm.com/downloads)  
   - Verify:
   
   bash 
     ```
     git --version
     ```

3. **Docker Desktop - Latest version**  
   - [Download here](https://www.docker.com/products/docker-desktop/)  
   - Verify:
  
     bash 
     ```
     docker --version
     docker-compose --version
     ```

4. **GitHub Account**  
   - [Sign up here](https://github.com)  

5. **Code Editor (Optional but recommended)**  
   - [VS Code](https://code.visualstudio.com/)  
   - Or Sublime Text, Atom, etc.

---

### Verify Everything is Installed

bash
```
node --version    # v18.x+ or v20.x+
npm --version     # 9.x+ or 10.x+
git --version     # 2.34+
docker --version  # 24.x+
```

### Step 1: Set Up Git for Version Control

Configure Git (one-time setup):

bash
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```
Create and initialize project:

bash
```
mkdir my-devops-project
cd my-devops-project
git init
```

## Step 2: Build a Node.js Web App

Initialize Node.js Project:

bash
```
npm init -y
```
The above creates a Package.json file where all the depencies for the app resides.

## Update package.json

Copy and paste the content below into the package.json file.

json

```
{
  "name": "my-devops-project",
  "version": "1.0.0",
  "description": "DevOps learning project with Node.js",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "jest",
    "dev": "node app.js",
    "lint": "eslint ."
  },
  "keywords": ["devops", "nodejs", "docker"],
  "author": "Your Name",
  "license": "MIT",
  "engines": {
    "node": ">=18.0.0"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "eslint": "^8.57.0",
    "supertest": "^7.1.4"
  }
}
```

Create Application file 

```
# Create app.js file which will serve as the entry point of the app.

tocuh app.js

```
Copy and past the content of [app.js](https://github.com/ibrahimbio/SkillSchuleDevOps2/blob/main/app.js)
```
// core modules
const http = require("http");
const url = require("url");

const PORT = process.env.PORT || 3000;
const ENVIRONMENT = process.env.NODE_ENV || "development";
let requestCount = 0;

// helpers for responses...
// [full code from your content]
```

## Install Dependencies

```bash
npm install --save-dev jest eslint supertest
npm install
```

## Step 3: Create Proper Tests


Create tests

```bash
mkdir tests
touch tests/app.test.js
```
Add tests/app.test.js:

js
```
const request = require('supertest');
const server = require('../app');

describe('App Endpoints', () => {
  afterAll(() => server.close());

  test('GET / should return welcome page', async () => {
    const response = await request(server).get('/');
    expect(response.status).toBe(200);
    expect(response.text).toContain('DevOps Lab 2025');
  });

  // other tests...
});
```

## Add Jest config (jest.config.js)

js
```
module.exports = {
  testEnvironment: 'node',
  collectCoverage: true,
  coverageDirectory: 'coverage',
  testMatch: ['**/tests/**/*.test.js'],
  verbose: true
};
```

## Step 4: GitHub Actions CI/CD Pipeline


Create workflow:
```
mkdir -p .github/workflows
```

Add .github/workflows/ci.yml:

yaml
```
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
    tags: [ 'v*' ]
  pull_request:
    branches: [ main ]

# [full YAML code from your content]
```

## Step 5: Dockerfile

Create Dockerfile:

dockerfile
```
FROM node:20-alpine AS dependencies
RUN apk update && apk upgrade --no-cache
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force

FROM node:20-alpine AS production
RUN apk update && apk upgrade --no-cache && \
    apk add --no-cache curl dumb-init && \
    rm -rf /var/cache/apk/*
RUN addgroup -g 1001 -S nodejs && adduser -S nodeuser -u 1001 -G nodejs
WORKDIR /app
COPY --from=dependencies --chown=nodeuser:nodejs /app/node_modules ./node_modules
COPY --chown=nodeuser:nodejs package*.json ./
COPY --chown=nodeuser:nodejs app.js ./
USER nodeuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=10s --start-period=15s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
ENTRYPOINT ["dumb-init", "--"]
CMD ["npm", "start"]
```

## Step 6: Essential Config Files


- .dockerignore (use ``` touch .dockerignore ``` to create file).
Copy and paste content below in created file

.dockerignore

```
node_modules 
npm-debug.log* 
.git 
.github 
.env 
.env.local 
.env.*.local 
logs 
*.log 
coverage 
.nyc_output 
.vscode 
.idea 
*.swp 
*.swo 
.DS_Store 
Thumbs.db 
README.md 
tests/ 
jest.config.js 
.eslintrc*
```
- .gitignore (use ``` touch .gitignore ``` to create file).
Copy and paste content below in created file:

.gitignore

```
# Dependencies
node_modules/
npm-debug.log*

# Runtime data pids
*.pid
*.seed
*.pid.lock

# Coverage
coverage/
.nyc_output

# Environment variables
.env
.env.local
.env.*.local

# Logs
logs
*.log

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

```
- .env.example (use ``` touch env.example ``` to create file).
Copy and paste content below in created file:

```
# Server Configuration
PORT=3000
NODE_ENV=production

# Logging
LOG_LEVEL=info

```
- .eslintrc.js  (use ``` touch .eslintrc.js ``` to create file).
Copy and paste content below in created file:

```
module.exports = {
  env: {
    node: true,
    es2021: true,
    jest: true
  },
  extends: ['eslint:recommended'],
  parserOptions: {
    ecmaVersion: 12,
    sourceType: 'module'
  },
  rules: {
    'no-console': 'off',
    'no-unused-vars': ['error', { 'argsIgnorePattern': '^_' }]
  }
};

```

(Use the configs provided in your content.)


## Step 7: Docker Compose

Create docker-compose.yml:

yaml

```
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - PORT=3000
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s
```

### Step 8: Test Everything Locally

bash

```
npm install
npm test
npm start
```

Test endpoints:

bash

```
curl http://localhost:3000/
curl http://localhost:3000/health
curl http://localhost:3000/info
curl http://localhost:3000/metrics
```

Docker commands:
bash
```
docker build -t my-devops-app:latest .
docker run -d -p 3000:3000 --name my-devops-container my-devops-app:latest
docker ps
docker logs my-devops-container
```

### Step 9: Deploy to GitHub

bash

```
git add .
git commit -m "Initial commit: Complete DevOps setup with working CI/CD"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/my-devops-project.git
git push -u origin main
```

### Step 10: Kubernetes Deployment

Create directories:

bash
```
mkdir -p k8s/staging k8s/production
```

Add staging and production deployments (deployment.yml in each).
(Use the full YAML from your content.)

### Step 11: Complete Deployment Workflow

- develop → deploys to staging
- main → deploys to production

Deploy to staging:

bash
```
git checkout -b develop
git add .
git commit -m "Add new feature"
git push origin develo
```
Deploy to production:

bash

```
git checkout main
git merge develop
git push origin main
```

### Troubleshooting Guide

## Common Issues

- npm test fails with module not found → Run npm install
- Docker build fails - permission denied → Ensure Docker Desktop is running
- GitHub Actions failing on push → Update YOUR_GITHUB_USERNAME
- Port 3000 already in use → Kill process or change port in .env
- eslint command not found → Run npm install --save-dev or npx eslint .
- Tests timing out → Ensure module.exports = server; in app.js
