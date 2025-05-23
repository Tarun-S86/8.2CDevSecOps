pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Install Dependencies') {
      steps {
        echo '⬇Installing…'
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running tests…'
        sh 'npm test 2>&1 | tee test.log'
      }
      post {
        always {
          emailext(
            to:   'dev-team@example.com',
            subject: "Tests ${currentBuild.fullDisplayName}: ${currentBuild.currentResult}",
            body: "<p>Test status: <b>${currentBuild.currentResult}</b></p>",
            attachmentsPattern: 'test.log'
          )
        }
      }
    }

    stage('Generate Coverage') {
      steps {
        echo 'Coverage…'
        sh 'npm run coverage || true'
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Auditing…'
        sh 'npm audit --audit-level=high 2>&1 | tee audit.log'
      }
      post {
        always {
          emailext(
            to:   'security-team@example.com',
            subject: "Security Scan ${currentBuild.fullDisplayName}: ${currentBuild.currentResult}",
            body: "<p>Scan status: <b>${currentBuild.currentResult}</b></p>",
            attachmentsPattern: 'audit.log'
          )
        }
      }
    }

    stage('Deploy to Staging') {
      steps { echo '🚀 Deploying…' }
    }

    stage('Staging Integration Tests') {
      steps { echo 'Test on staging…' }
    }
  }

  post {
    success { echo 'Pipeline succeeded!' }
  }
}
