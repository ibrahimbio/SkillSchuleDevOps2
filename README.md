# 🚀 DevOps Lab 2025 – Complete Setup Guide

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
⏱️ **Time Required: 15-30 minutes**

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
     ```bash
     git --version
     ```

3. **Docker Desktop - Latest version**  
   - [Download here](https://www.docker.com/products/docker-desktop/)  
   - Verify:  
     ```bash
     docker --version
     docker-compose --version
     ```

4. **GitHub Account**  
   - [Sign up here](https://github.com)  

5. **Code Editor (Optional but recommended)**  
   - [VS Code](https://code.visualstudio.com/)  
   - Or Sublime Text, Atom, etc.

---

### ✅ Verify Everything is Installed
```bash
node --version    # v18.x+ or v20.x+
npm --version     # 9.x+ or 10.x+
git --version     # 2.34+
docker --version  # 24.x+
```

### Step 1: Set Up Git for Version Control
⏱️ Time Required: 5 minutes
Configure Git (one-time setup):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```
Create and initialize project:
```bash
mkdir my-devops-project
cd my-devops-project
git init
```

Step 2: Build a Node.js Web App
⏱️ Time Required: 10-15 minutes
Initialize Node.js Project
```bash
npm init -y
```

## Update package.json

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
