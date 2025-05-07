🚀 Bookstore API Test Automation with CI/CD Integration
End-to-end API test automation framework using Java, Cucumber BDD, and TestNG, integrated with Allure Reports and CI/CD pipelines via Jenkins.

🧰 Tech Stack
Component	Details
🧠 IDE	IntelliJ IDEA
☕ Language	Java 11+
🔍 Framework	Cucumber BDD + Rest Assured for clean, readable API tests
🛠 Build Tool	Maven for dependency management and build automation
✅ Test Runner	TestNG for flexible test execution, retries, and parallel runs
📊 Reporting	Allure for insightful, interactive test reports

💡 Why This Stack?
✅ TestNG Over JUnit
Excellent for non-Spring Boot architectures

Built-in retry logic, parallel execution, and listener support

CI-friendly configuration

📈 Allure Reporting Benefits
Interactive test history and failure trends

Visual breakdown of test vs product failures

Easily integrated into Jenkins/GitHub Actions pipelines

⚙️ Project Setup
🔧 Prerequisites
Java 11+

Maven installed and configured

🛠 Getting Started
Create a new Maven project or fork this repo.

Clone the Dev repository and follow its setup instructions.

API Endpoints covered in test scenarios:

POST /signup – Register a new user

POST /login – Authenticate and retrieve a token

POST /books – Add a new book

PUT /books/{id} – Update book details

GET /books/{id} – Retrieve book by ID

GET /books – List all books

DELETE /books/{id} – Remove a book

Run tests using the Cucumber TestNG Runner.

After execution, view reports at target/allure-results.

🔄 CI/CD Pipeline Integration
🧰 Required Tools
Jenkins installed locally

Required Jenkins plugins:

Git

GitHub

Pipeline

Maven Integration

Allure Reporting

Ngrok for local tunneling (development/test environments)

🔃 CI/CD Flow: Development to QA
1. Jenkins Pipeline – Dev Repo (Triggers QA Automation)
groovy
Copy
Edit
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
2. Jenkins Pipeline – QA Repo (Runs Tests + Reports)
groovy
Copy
Edit
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
🌐 Webhook Setup for Auto-Triggering Builds
Launch Jenkins locally (http://localhost:8080)

Set up 2 Jenkins pipeline jobs:

One for Dev repo

One for QA Automation repo

Configure GitHub webhook in Dev repo:

Use Ngrok to expose local Jenkins:

nginx
Copy
Edit
ngrok http http://localhost:8080
Use generated URL in GitHub Webhook:

php-template
Copy
Edit
https://<ngrok-url>/job/<DevJobName>/build?token=<your-token>
Push changes to Dev repo → Dev build triggers → QA job runs → Allure report generated.

📁 Reports & Artifacts
Reports generated at:
target/allure-results → Open in Allure Report Viewer

📌 Summary
This framework offers a modular and CI-ready approach for end-to-end API testing with:

Clean BDD structure

Parallel execution & retries

CI/CD with Jenkins

Interactive reporting
