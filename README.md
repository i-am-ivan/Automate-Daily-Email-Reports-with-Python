
# Daily Email Report Automation with Python, Docker, and Kubernetes

## Overview

This project demonstrates how to automate sending daily email reports using Python. The script is designed to run seamlessly on a local machine, in containers using Docker, and in orchestration environments such as Kubernetes. The project provides a scalable, portable, and automated solution to streamline email reporting.

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Features](#features)
4. [Installation and Setup](#installation-and-setup)
   1. [Local Machine](#local-machine)
   2. [Running on Linux](#running-on-linux)
5. [Running the Script](#running-the-script)
6. [Testing the Script](#testing-the-script)
7. [Docker Containerization](#docker-containerization)
8. [Deploying on Kubernetes](#deploying-on-kubernetes)
9. [Screenshots](#screenshots)
10. [Conclusion](#conclusion)

## Introduction

This documentation provides a step-by-step guide to implementing and deploying a Python-based automated email report system. The core Python script, using the `schedule` library, allows for sending daily reports at specified times. In addition to local execution, the script is designed to run on a Docker container and in Kubernetes, making it suitable for both local and cloud environments.

### Use Cases:
- Automated daily email reporting for monitoring systems.
- Daily logs or updates delivered via email.
- Periodic reminders sent programmatically.

## Prerequisites

Before setting up and running the script, ensure that the following tools and packages are available on your system.

### Requirements:
1. **Python 3.x** installed on your machine.
2. **pip**: Python package installer.
3. **Docker** for containerization.
4. **Kubernetes** for deployment in cloud environments.

### Python Libraries:
- `smtplib`: Used to send emails using the Simple Mail Transfer Protocol.
- `schedule`: A Python library for scheduling tasks.
  
To install Python dependencies:
```bash
pip install schedule
```
## Features

- **Automated Scheduling**: The script sends daily email reports at a predefined time using the `schedule` library.
- **Docker Integration**: The application can be containerized using Docker, making it portable across different platforms.
- **Kubernetes Support**: Supports deployment in Kubernetes, enabling scalable, fault-tolerant deployment.
- **Email Sending**: Uses SMTP to send emails, including attachment support for more complex reports.
- **Scalable**: Can be easily scaled using Docker or Kubernetes in enterprise environments.

## Installation and Setup

### 1. Local Machine

Follow these steps to set up and run the script on a local machine.

#### Step 1: Clone the Repository

```bash
git clone https://github.com/your-repo/email-automation.git
cd email-automation
```

#### Step 2: Install Dependencies

Ensure Python 3.x is installed and then install the required libraries:

```bash
pip install schedule
```

#### Step 3: Configuration

Update the SMTP configuration inside the script for email credentials:

```python
SMTP_SERVER = "smtp.example.com"
SMTP_PORT = 587
EMAIL_ADDRESS = "your-email@example.com"
EMAIL_PASSWORD = "your-password"
```

You can also modify the report content inside the `create_report()` function in the `email_report.py` script.

### 2. Running on Linux

The script can be run on a Linux system by following these steps:

#### Step 1: Ensure Python is Installed

```bash
sudo apt update
sudo apt install python3 python3-pip
```

#### Step 2: Run the Script

You can run the script using `nohup` to keep it running after you log out of the session:

```bash
nohup python3 email_report.py &
```

#### Step 3: Set Up Cron (Optional)

Instead of using the `schedule` library, you can set up a cron job to run the Python script daily:

```bash
crontab -e
```

Add the following cron job to run the script daily at 9:00 AM:

```bash
0 9 * * * /usr/bin/python3 /path/to/email_report.py
```

## Running the Script

Once the configuration is set, you can start the script using:

```bash
python email_report.py
```

This will start sending the daily reports based on the scheduled time set in the script.

## Testing the Script

To test the functionality of the email automation script, you can use the following test script.

### Test Script (`test_email_report.py`):

```python
import unittest
from email_report import send_email, create_report

class TestEmailReport(unittest.TestCase):
    def test_create_report(self):
        report = create_report()
        self.assertIsNotNone(report)
        self.assertIsInstance(report, str)

    def test_send_email(self):
        result = send_email("test@example.com", "Daily Report", "This is a test report")
        self.assertTrue(result)

if __name__ == "__main__":
    unittest.main()
```

### Running Tests

```bash
python -m unittest test_email_report.py
```

This will validate the creation of reports and ensure that emails are sent correctly.

## Docker Containerization

To run the Python script inside a Docker container, follow these steps.

### Step 1: Create a `Dockerfile`

```Dockerfile
FROM python:3.9-slim

WORKDIR /usr/src/app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

CMD [ "python", "./email_report.py" ]
```

### Step 2: Build the Docker Image

```bash
docker build -t email-reporter .
```

### Step 3: Run the Docker Container

```bash
docker run -d --name email-reporter email-reporter
```

## Deploying on Kubernetes

This section demonstrates how to deploy the Dockerized Python email automation script on Kubernetes for high availability and scalability.

### Step 1: Create Kubernetes Deployment YAML

Create a file named `email-reporter-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: email-reporter-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: email-reporter
  template:
    metadata:
      labels:
        app: email-reporter
    spec:
      containers:
      - name: email-reporter
        image: email-reporter:latest
        ports:
        - containerPort: 80
```

### Step 2: Apply the Kubernetes Deployment

```bash
kubectl apply -f email-reporter-deployment.yaml
```

### Step 3: Check Kubernetes Pods

To check the status of your deployment, use:

```bash
kubectl get pods
```

### Step 4: Scaling the Deployment

You can scale the deployment to multiple instances for load balancing:

```bash
kubectl scale deployment email-reporter-deployment --replicas=3
```

## Screenshots

Below are the screenshots of the program running and the test results:

- **Program Running**  
  ![Program Running](assets/program_running.png)

- **Test Script Running**  
  ![Test Script](assets/test_script.png)

- **Test Results**  
  ![Test Results](assets/test_results.png)

## Conclusion

This Python-based email automation system, combined with Docker and Kubernetes, provides a scalable, portable solution for sending scheduled email reports. It is ideal for use in monitoring systems, daily reporting, and enterprise-level deployments where automated email notifications are essential.

For further improvements, you can add logging, error handling, and more customizable report generation mechanisms.

### References

- Python `smtplib`: https://docs.python.org/3/library/smtplib.html
- Docker: https://www.docker.com/
- Kubernetes: https://kubernetes.io/
```
