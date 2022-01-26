+++
title = "Sonarqube for Quality Code and Code Security"
categories = [
    "productivity-tools",
]
tags = [
    "sonarqube"
]
date = "2022-01-26"
draft = false
+++


## Installation

### Install Sonarqube

Install menggunakan docker.

```bash
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest
```

Setelah itu, untuk melakukan scanning di local diperlukan sonar-scan. 

### Install Sonar Scan

#### Windows

1. Command Prompt

   - [Download file sonar-scanner](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/)
   - Extract File
   - Setting sistem environment variable
     - Edit pada variable PATH
     - Arahkan PATH folder `/bin` pada didalam folder hasil extract sebelumnya

2. Powershell

    ```bash
    $env:Path += ';C:\sonar-scanner\sonar-scanner-4.6.2.2472-windows\bin'
    ```


## Integration

### Local

- Buat manual project
- Analisa code di local
- Buat token
    
    ```   
        zeihanaulia: 76098068a0a3f90db6cf2ba759129690176eedf4
    ```
- Sonar Scan
    
    ```bash
        sonar-scanner 
            -Dsonar.projectKey=project-name 
            -Dsonar.sources=. 
            -Dsonar.host.url=http://localhost:9000 
            -Dsonar.login=76098068a0a3f90db6cf2ba759129690176eedf4
    ```
### Github

### Gitlab

### Bitbucket