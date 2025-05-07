# 🚀 Bookstore API Test Automation with CI/CD Integration

End-to-end API test automation framework using **Java**, **Cucumber BDD**, and **TestNG**, integrated with **Allure Reports** and CI/CD pipelines via **Jenkins**.

---

## 🧰 Tech Stack

| Component         | Details                                                                 |
|------------------|-------------------------------------------------------------------------|
| 🧠 IDE            | IntelliJ IDEA                                                           |
| ☕ Language       | Java 11+                                                                |
| 🔍 Framework     | Cucumber BDD + Rest Assured for clean, readable API tests               |
| 🛠 Build Tool     | Maven for dependency management and build automation                    |
| ✅ Test Runner    | TestNG for flexible test execution, retries, and parallel runs          |
| 📊 Reporting     | Allure for insightful, interactive test reports                          |

---

## 💡 Why This Stack?

### ✅ TestNG Over JUnit
- Excellent for non-Spring Boot architectures
- Built-in retry logic, parallel execution, and listener support
- CI-friendly configuration

### 📈 Allure Reporting Benefits
- Interactive test history and failure trends
- Visual breakdown of test vs product failures
- Easily integrated into Jenkins/GitHub Actions pipelines

---

## ⚙️ Project Setup

### 🔧 Prerequisites
- Java 11+
- Maven installed and configured

### 🛠 Getting Started

1. Create a new Maven project or fork this repo.
2. Clone the Dev repository and follow its setup instructions.
3. API Endpoints covered in test scenarios:
   - `POST /signup` – Register a new user  
   - `POST /login` – Authenticate and retrieve a token  
   - `POST /books` – Add a new book  
   - `PUT /books/{id}` – Update book details  
   - `GET /books/{id}` – Retrieve book by ID  
   - `GET /books` – List all books  
   - `DELETE /books/{id}` – Remove a book  

4. Run tests using the **Cucumber TestNG Runner**.
5. After execution, view reports at `target/allure-results`.

---

## 🔄 CI/CD Pipeline Integration

### 🧰 Required Tools

- Jenkins installed locally
- Required Jenkins plugins:
  - Git
  - GitHub
  - Pipeline
  - Maven Integration
  - Allure Reporting
- **Ngrok** for local tunneling (development/test environments)

---

## 🔃 CI/CD Flow: Development to QA

This project supports CI/CD integration through Jenkins to automate the testing pipeline from Dev commits to QA execution and reporting.

---

### 🧩 1. Jenkins Pipeline – Dev Repository (Triggers QA Automation)

```groovy```
pipeline {
    agent any
    stages {
        stage('Build Dev') {
            steps {
                echo 'Building dev code...'
            }
        }
        stage('Trigger QA Automation') {
            steps {
                build job: 'QA-Repo'
            }
        }
    }
}
pipeline {
    agent any
    tools {
        maven 'Maven 3.6.3'
        allure 'Allure'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: '<GIT_REPO_URL>', branch: '<BRANCH_NAME>'
            }
        }
        stage('Build and Test') {
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Generate Allure Report') {
            steps {
                sh 'mvn allure:report'
            }
        }
    }
    post {
        always {
            allure([
                includeProperties: false,
                jdk: '',
                results: [[path: 'target/allure-results']]
            ])
        }
    }
}
## 🌐 Webhook Setup for Auto-Triggering Builds

Integrate GitHub with Jenkins using webhooks to trigger builds automatically on each push.

---

### 🧱 Step-by-Step Setup

#### 🔹 1. Launch Jenkins Locally

Start Jenkins on your local machine:


Ensure your Jenkins server is accessible and the necessary plugins (Git, GitHub, Pipeline, Maven, Allure) are installed.

---

#### 🔹 2. Expose Jenkins Publicly Using Ngrok

Jenkins is local, so use [Ngrok](https://ngrok.com/) to make it publicly accessible:

```bash
```
ngrok http http://localhost:8080
### 🔹 3. Create Jenkins Pipeline Jobs

Set up two pipeline jobs in Jenkins:

- **Dev Job**: Builds the development code and triggers the QA pipeline.
- **QA Job**: Executes automated tests and generates Allure reports.

✅ *Tip: Make sure both jobs run successfully at least once manually before integrating with GitHub.*

---

### 🔹 4. Add GitHub Webhook

Integrate GitHub with Jenkins to trigger builds on every push.

#### 🚀 Steps:

1. Open your **Dev GitHub Repository**.
2. Go to:  
   `Settings` → `Webhooks` → `Add webhook`
3. In the **Payload URL**, use the format:


**Replace:**
- `<username>` → Your Jenkins username
- `<token>` → Jenkins API token  
  *(Jenkins → User → Configure → API Token)*
- `<ngrok-id>` → Your public Ngrok domain (e.g., `abcd1234`)
- `<DevJobName>` → Exact Jenkins job name for Dev pipeline

4. **Content Type**:  

5. **Event Trigger**:  
Select:

6. Click **Add Webhook**.

---

### ✅ What Happens Now?

Every time a commit is pushed to the **Dev GitHub repo**:

1. GitHub triggers a POST request to Jenkins via Ngrok.
2. Jenkins runs the **Dev pipeline job**.
3. On successful execution, Jenkins automatically starts the **QA job**.
4. QA job runs all test cases and generates a detailed **Allure Report**.

> This ensures a seamless, automated CI/CD pipeline from development to quality assurance — with full test visibility.


