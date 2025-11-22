**DevOps Intern Final Assessment**

Name: Kshitija Sandip Khilari
Date: 22 November 2025

This repository contains the complete DevOps Intern Final Assessment, demonstrating practical skills across Linux, Git, Docker, CI/CD, Nomad, and monitoring with Grafana Loki.

1. Git & GitHub Setup

A public GitHub repository was created and initialized with:

hello.py

README.md

Proper folder structure for all components

hello.py
print("Hello, DevOps!")

2. Linux Scripting

Created a scripts/ directory containing:

scripts/sysinfo.sh
#!/bin/bash
echo "Current User:"
whoami

echo "Current Date:"
date

echo "Disk Usage:"
df -h

Make executable
chmod +x scripts/sysinfo.sh

Run script
bash scripts/sysinfo.sh

3. Docker Setup

A Dockerfile was added to containerize the Python script.

Dockerfile
FROM python:3.10-slim
WORKDIR /app
COPY hello.py .
CMD ["python", "hello.py"]

Build image
docker build -t devops-hello .

Run container
docker run devops-hello


This executes the Python script inside a container.

4. GitHub Actions — CI Pipeline

A CI workflow was created under:

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


This pipeline automatically runs the Python script on every push to main.

A CI badge has been added at the top of this README.

5. Nomad Job Deployment

A nomad/ folder was added with a job file to deploy the Docker container using Nomad.

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

To run Nomad locally

Start Nomad in development mode:

nomad agent -dev


Run the job:

nomad job run nomad/hello.nomad

6. Monitoring with Grafana Loki

A folder named monitoring/ contains setup instructions.

monitoring/loki_setup.txt
To start Loki locally:
docker run -d --name=loki -p 3100:3100 grafana/loki:2.9.0

To view logs from the Loki container:
docker logs loki

To forward application logs to Loki:
docker run --log-driver=loki \
  --log-opt loki-url="http://localhost:3100/loki/api/v1/push" \
  devops-hello


Loki listens on port 3100 and can receive logs from Docker containers or Nomad tasks.

Optional screenshots may be added for documentation.

Project Structure
devops-intern-final/
├── hello.py
├── Dockerfile
├── README.md
├── scripts/
│   └── sysinfo.sh
├── nomad/
│   └── hello.nomad
├── monitoring/
│   └── loki_setup.txt
└── .github/
    └── workflows/
        └── ci.yml

Conclusion

This assessment demonstrates:

Git version control

Linux shell scripting

Docker containerization

CI Automation using GitHub Actions

Workload deployment using Nomad

Basic log monitoring using Loki

The project fulfills all requirements of the DevOps Intern Final Evaluation and represents a complete, functional DevOps workflow.
