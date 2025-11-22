Name: Kshitija Sandip Khilari
Date: 22 November 2025

This repository contains the final DevOps workflow assessment, demonstrating Git, Linux scripting, Docker, CI/CD automation, Nomad workload deployment, and basic monitoring using Grafana Loki.

1. Git & GitHub Setup

A new public repository was created containing the project files.
The repository includes:

README.md

hello.py — sample Python script:

print("Hello, DevOps!")


A first commit was made to initialize the repository.

2. Linux Scripting

A folder named scripts/ was created containing:

scripts/sysinfo.sh
#!/bin/bash
echo "Current User:"
whoami

echo "Current Date:"
date

echo "Disk Usage:"
df -h


To make the script executable:

chmod +x scripts/sysinfo.sh


To run the script:

bash scripts/sysinfo.sh

3. Docker Setup

A Dockerfile was created in the project root:

FROM python:3.10-slim
WORKDIR /app
COPY hello.py .
CMD ["python", "hello.py"]

Build the Docker image:
docker build -t devops-hello .

Run the Docker container:
docker run devops-hello


This executes the Python script inside a container.

4. GitHub Actions — CI Pipeline

A workflow file was added under:

.github/workflows/ci.yml

ci.yml
name: CI Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  run-hello:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Run Python Script
      run: python hello.py

CI Status Badge

Add this to the top of the README:

![CI Status](https://github.com/YOUR_GITHUB_USERNAME/devops-intern-final/actions/workflows/ci.yml/badge.svg)


This pipeline executes the Python script automatically on every push.

5. Nomad Job Deployment

A folder named nomad/ was created containing the job file:

nomad/hello.nomad
job "hello-service" {
  datacenters = ["dc1"]
  type = "service"

  group "hello-group" {
    task "hello-task" {
      driver = "docker"

      config {
        image = "devops-hello"
      }

      resources {
        cpu    = 100
        memory = 128
      }
    }
  }
}

Run the Nomad Job

Start Nomad in dev mode:

nomad agent -dev


Deploy the job:

nomad job run nomad/hello.nomad

6. Monitoring with Grafana Loki

A folder named monitoring/ was created containing:

monitoring/loki_setup.txt
To start Loki locally:
docker run -d --name=loki -p 3100:3100 grafana/loki:2.9.0

To view logs from the Loki container:
docker logs loki

To forward application logs to Loki:
docker run --log-driver=loki \
  --log-opt loki-url="http://localhost:3100/loki/api/v1/push" \
  devops-hello


A screenshot of log output may be included as needed.

Project Structure
devops-intern-final/
│── hello.py
│── Dockerfile
│── README.md
│── scripts/
│     └── sysinfo.sh
│── nomad/
│     └── hello.nomad
│── monitoring/
      └── loki_setup.txt
│── .github/
      └── workflows/
             └── ci.yml

Conclusion

This project demonstrates an end-to-end DevOps workflow, covering:

Version control with Git

Linux shell scripting

Containerization with Docker

Automated CI using GitHub Actions

Application deployment with Nomad

Basic log monitoring using Loki

The repository fulfills all requirements of the DevOps Intern Final Assessment.
