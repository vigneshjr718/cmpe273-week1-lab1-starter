# CMPE 273 – Week 1 Lab 1: Your First Distributed System

## Overview
This lab implements a tiny, locally distributed system consisting of two independent services that communicate over HTTP. The goal is to demonstrate basic service-to-service communication, logging, timeouts, and handling partial failure.

---

## Services

### Service A (Echo API)
- Runs on `localhost:8080`
- Endpoints:
  - `GET /health` → `{"status":"ok"}`
  - `GET /echo?msg=hello` → `{"echo":"hello"}`

### Service B (Client)
- Runs on `localhost:8081`
- Endpoints:
  - `GET /health` → `{"status":"ok"}`
  - `GET /call-echo?msg=hello`
    - Calls Service A `/echo`
    - Uses a timeout
    - Returns HTTP 503 if Service A is unavailable

---

## How to Run Locally (Mac)

### Terminal 1 – Service A
```bash
cd python-http/service-a
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 app.py
Service A will start on:

http://127.0.0.1:8080

Terminal 2 – Service B
cd python-http/service-b
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python3 app.py


Service B will start on:

http://127.0.0.1:8081

Testing
Success Case

<img width="842" height="495" alt="sucessimage" src="https://github.com/user-attachments/assets/8ad4d358-24f5-4d90-8ded-fe5edaff196f" />

With both services running:

curl "http://127.0.0.1:8081/call-echo?msg=hello"


Expected result:

HTTP 200 response

JSON containing the echoed message

Logs printed in both Service A and Service B showing endpoint, status, and latency

Failure Case

<img width="848" height="453" alt="failureimage" src="https://github.com/user-attachments/assets/956e5a19-c9fc-4988-beee-dfa8a43cca10" />

Stop Service A by pressing Ctrl + C in Terminal 1

Run the same request again:

curl -i "http://127.0.0.1:8081/call-echo?msg=hello"


Expected result:

HTTP 503 Service Unavailable

Service B logs an error indicating Service A is unavailable

What Makes This Distributed?

This system is distributed because it consists of two independent services running as separate processes on different ports that communicate over HTTP. Service B depends on Service A via a network call and must handle partial failure when Service A is unavailable, returning HTTP 503. Even though both services run on localhost, they exhibit key distributed system characteristics such as network communication, independent failure, and fault handling.

Notes

Each service runs in its own process and terminal

Service B uses a timeout when calling Service A

Basic request logging includes service name, endpoint, status, and latency
