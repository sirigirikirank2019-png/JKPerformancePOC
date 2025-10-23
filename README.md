# HTTPBin Post API Performance Test Suite (JMeter + Docker Compose)

## Overview
HTTPBin is a simple HTTP request and response service, commonly used for testing and learning. 
Performance testing it helps simulate real-world API behavior, validate performance metrics, and understand system limits before testing actual APIs.

# Objective
The objective of HTTPBin API performance testing is to evaluate how well the API behaves under different conditions and loads. 
HTTPBin is often used as a test endpoint for experiments because it simulates various HTTP methods, status codes, and responses. Here’s a detailed breakdown:

# Goal
The goal is to ensure the API performs reliably, quickly, and consistently under expected and extreme conditions, and to identify bottlenecks before deploying similar real-world APIs.

# Functional Requirements
Endpoints to Test: Identify which HTTPBin endpoints will be part of testing, e.g.:
/get – Simple GET requests
/post – POST requests with payload
/put, /patch, /delete – Other HTTP methods
/status/:code – Simulate various response codes
/delay/:seconds – Test delayed responses
HTTP Methods: GET, POST, PUT, PATCH, DELETE.
Payload Requirements: JSON or form-data for POST, PUT, PATCH requests

# Workload modelling
Workload modeling is the process of defining how users interact with a system or API during performance testing. 
It helps simulate realistic user behavior, ensuring that the test reflects production-like conditions.
It includes aspects like:
    Number of users
    Frequency of requests
    Mix of operations
    Think time between requests
    Ramp-up patterns

# Scenario Design:
Load Testing
    Objective: Measure API performance under expected normal and peak load conditions.
    Purpose: Validate response time, throughput, and error rates under expected traffic.
    Key Metrics: Response time, throughput, error rate, CPU/memory utilization (if self-hosted).
Stress Testing
    Objective: Push the API beyond its expected limits to identify breaking points.
    Purpose: Find maximum capacity, observe failure behavior, and determine recovery capability.
    Key Metrics: Point of failure, error rate spikes, response time degradation.
Endurance (Soak) Testing
    Objective: Test API over a long duration to detect performance degradation over time.
    Purpose: Identify memory leaks, resource exhaustion, or throughput decline.
    Key Metrics: Long-term CPU/memory usage, increasing response times, growing error rates.
Spike Testing
    Objective: Test API with sudden bursts of traffic to observe behavior under extreme but short-term load.
    Purpose: Check API stability, error handling, and recovery after sudden spikes.
    Key Metrics: Response time spikes, error rate, recovery time.
# Tool Choosen
 - Jmeter - Open Source Tool

## Setup
### 1️⃣ Docker Setup
Docker allows you to run HTTPBin in an isolated container, ensuring consistent API behavior regardless of your host environment.
Steps:
Install Docker – Download Docker Desktop (Windows/macOS) or Docker Engine (Linux) and verify installation.
Run HTTPBin Container – 
docker run -d --name=httpbin -p 91:80 kennethreitz/httpbin
Pull the official kennethreitz/httpbin image and run it on a local port (e.g., 9091).
Verify API – Access endpoints like **http://localhost:91/get** to confirm HTTPBin is responding correctly.

### 2️⃣ JMeter 
wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-5.6.3.zip
unzip apache-jmeter-5.6.3.zip -d /opt/
ln -s /opt/apache-jmeter-5.6.3/bin/jmeter /usr/local/bin/jmeter
- Open `PostBin_Test.jmx` in JMeter GUI
- Adjust Thread Group for user count & duration
- Run test and view results

## Results & Reports
- Raw results: `results/`
- HTML report: `results`

## Analyzation
Analyze the resource utilization http://localhost:9091/containers/
    1. CPU usage
    2. Memory usage
    3. Throughput
    4. Error

🏁 Next Steps Integrate with CI/CD (e.g., Jenkins) for automated performance runs Add custom assertions & SLAs in the JMeter test plan Extend coverage to more endpoints or advanced scenarios
    
