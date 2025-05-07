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

### 🔃 CI/CD Flow: Development to QA

#### 1. Jenkins Pipeline – Dev Repo (Triggers QA Automation)
```groovy
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

To automatically trigger Jenkins builds on every commit to the Dev repository, follow these steps:

### 1. Launch Jenkins Locally

Start Jenkins on your local machine (usually runs at `http://localhost:8080`).

### 2. Create Jenkins Pipeline Jobs

- **Dev Job**: Builds the dev code and triggers QA automation.
- **QA Job**: Executes tests and generates Allure reports.

### 3. Expose Jenkins to the Internet (Using Ngrok)

Use Ngrok to make your local Jenkins accessible to GitHub:

```bash
ngrok http http://localhost:8080
### 4. Configure GitHub Webhook

To allow GitHub to trigger your local Jenkins job:

#### 🔧 Steps:

1. Navigate to your **Dev GitHub repository**.
2. Go to:  
   `Settings` → `Webhooks` → `Add webhook`
3. In the **Payload URL**, enter the following format:


Replace:
- `<username>` → Your Jenkins username
- `<token>` → Jenkins API token (generate from Jenkins user settings)
- `<ngrok-id>` → The public ID from Ngrok (e.g., `abcd1234`)
- `<DevJobName>` → Your actual Jenkins Dev job name

4. Set **Content type** to:


5. Choose **Just the push event**.
6. Click **Add webhook**.

#### ✅ Result:

Every time you push a commit to the Dev repo:
- GitHub sends a POST request to Jenkins
- The Dev pipeline is triggered
- On success, QA automation runs
- Allure reports are generated automatically
