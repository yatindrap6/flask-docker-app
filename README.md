# 🚀 Automate Docker Image Creation and Deployment using Jenkins Pipeline

This DevOps project demonstrates how to automate the process of building Docker images and pushing them to a **self-hosted Docker registry** using a **Jenkins pipeline** — all set up manually from scratch (no prebuilt Jenkins images).

## 🧩 Tech Stack
- Jenkins (manual install, no image pulling)
- Docker (image build & push)
- Self-Hosted Docker Registry (`localhost:5000`)
- Python Flask App
- GitHub (source code management)

---

## 🔁 CI/CD Pipeline Workflow

1. Developer pushes code to GitHub.
2. Jenkins pipeline is triggered.
3. Jenkins:
   - Clones the GitHub repo
   - Builds a Docker image from `Dockerfile`
   - Tags the image with `latest` and build number
   - Pushes image to `localhost:5000`
4. Image is ready to be pulled/deployed from the internal registry.

---

## 🛠️ Jenkins Pipeline: `Jenkinsfile`

```groovy
pipeline {
  agent any

  environment {
    REGISTRY = "localhost:5000"
    IMAGE = "flaskapp"
    TAG = "${env.BUILD_NUMBER}"
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
        docker build -t ${REGISTRY}/${IMAGE}:${TAG} .
        docker tag ${REGISTRY}/${IMAGE}:${TAG} ${REGISTRY}/${IMAGE}:latest
        '''
      }
    }

    stage('Push to Registry') {
      steps {
        sh '''
        docker push ${REGISTRY}/${IMAGE}:${TAG}
        docker push ${REGISTRY}/${IMAGE}:latest
        '''
      }
    }
  }
}
````

---

## 📂 Project Structure

```
flask-docker-app/
├── app.py
├── requirements.txt
├── Dockerfile
└── Jenkinsfile
```

---

## 🔍 Example Output

```bash
curl http://localhost:5000/v2/_catalog
# Output: {"repositories":["flaskapp"]}

curl http://localhost:5000/v2/flaskapp/tags/list
# Output: {"name":"flaskapp","tags":["1","latest"]}
```

---

## ✨ Highlights

* 🔧 Jenkins installed and configured manually (no Docker images used)
* 🐳 Docker integrated with Jenkins user
* 📦 Secure, internal Docker registry setup
* ⚙️ Full pipeline automation of build & push
* 💬 Well-documented and production-ready starter project

---

## 👤 Author

**Yatindra Panchal**

