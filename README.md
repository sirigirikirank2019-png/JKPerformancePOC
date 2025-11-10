# 🚀 JMeter CI/CD Pipeline with GitHub Actions

This repository automates **performance testing with Apache JMeter** using **GitHub Actions**.  
It provisions **HTTPBin** as a mock API service and **cAdvisor** for container metrics, executes JMeter tests, and uploads comprehensive performance reports as workflow artifacts.

---

## 🧩 Workflow Overview

### File: `.github/workflows/jmeter-ci.yml`
This GitHub Actions workflow performs the following steps automatically on every push or pull request to the `main` branch:

1. **Set up environment**
   - Runs on `ubuntu-22.04`
   - Installs JDK 17, JMeter, and required plugins

2. **Start dependent services**
   - 🧱 **HTTPBin** (on port `91`) — provides sample REST endpoints  
   - 📊 **cAdvisor** (on port `8080`) — exports container-level performance metrics

3. **Run JMeter test**
   - Executes the JMeter test plan:
     ```
     ./Scritps/PT01_LoadTest_HTTPMethods.jmx
     ```
   - Generates HTML and JTL reports in `/tmp/results/`

4. **Collect reports**
   - Copies JMeter HTML report and cAdvisor metrics into:
     ```
     /tmp/final_reports/
     ```

5. **Upload artifacts**
   - Uploads performance reports as GitHub workflow artifacts for download.

---

## ⚙️ Services Used

| Service   | Purpose                        | Port | Docker Image                              |
|------------|--------------------------------|------|-------------------------------------------|
| HTTPBin    | Mock API server for testing    | 91   | `kennethreitz/httpbin`                    |
| cAdvisor   | Container performance metrics  | 8080 | `gcr.io/cadvisor/cadvisor:latest`         |

---

## 🧰 Installed Components

| Component | Version / Source |
|------------|------------------|
| JMeter     | 5.6.3 (from Apache archive) |
| Java       | OpenJDK 17 |
| JMeter Plugins | Plugins Manager + Ultimate Thread Group |

---

## 📂 Output Artifacts

After the workflow runs, you can find the following artifacts under  
**GitHub Actions → Run → Artifacts:**

