# DevOps Task: System Provisioning - Hello GUVI GEEK

## 1. Project Overview

This repository demonstrates a complete Continuous Integration/Continuous Delivery (CI/CD) pipeline implemented using **GitHub**, **Jenkins**, and **Docker Hub** on an **AWS EC2 (Ubuntu)** instance.

The pipeline automates the following steps on every code commit:
1.  **Checkout:** Pulls code from this GitHub repository.
2.  **Build:** Creates a Docker Image of the Node.js "Hello GUVI GEEK" web application.
3.  **Test:** Runs the container briefly to verify startup.
4.  **Push:** Pushes the final image to Docker Hub.

---

## 2. Final Pipeline Screenshot

<img src="https://github.com/Hishamaliec/Devops-Task---Hello-GUVI-GEEK/blob/main/assets/screenshot-2025-10-08_07-41-44.png" alt="Banner" />


---

## 3. Docker Image Link

The final container image is available on Docker Hub.

* **Docker Hub Repository URL:** [https://hub.docker.com/repository/docker/hishamali/hello-guvi-geek/general](https://hub.docker.com/repository/docker/hishamali/hello-guvi-geek/general)
* **Latest Image Tag:** `hishamali/hello-guvi-geek:latest`

### How to Run the Application:

To run the final application from Docker Hub, execute the following commands:

```bash
# 1. Pull the image
docker pull hishamali/hello-guvi-geek:latest

# 2. Run the container (mapping port 8080)
docker run -d -p 8080:8080 hishamali/hello-guvi-geek:latest

# 3. Access the application in your browser:
# http://localhost:8080
````

-----

## 4\. Work Files Summary

  * `Jenkinsfile`: Pipeline as Code (Groovy script).
  * `Dockerfile`: Defines the container image build process.
  * `app.js`: Node.js application displaying "Hello GUVI GEEK."
  * `package.json`: Node.js dependency file.
