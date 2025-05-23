pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        echo 'Installing…'
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running tests…'
        // redirect both stdout+stderr into test.log
        bat 'npm test > test.log 2>&1'
      }
      post {
        always {
          emailext(
            to: 'dev-team@example.com',
            subject: "Tests ${currentBuild.fullDisplayName}: ${currentBuild.currentResult}",
            body: "<p>Test status: <b>${currentBuild.currentResult}</b></p>",
            attachmentsPattern: 'test.log'
          )
        }
      }
    }

    stage('Generate Coverage') {
      steps {
        echo 'Generating coverage…'
        bat 'npm run coverage > coverage.log 2>&1 || exit 0'
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Running security audit…'
        bat 'npm audit --audit-level=high > audit.log 2>&1'
      }
      post {
        always {
          emailext(
            to: 'security-team@example.com',
            subject: "Security Scan ${currentBuild.fullDisplayName}: ${currentBuild.currentResult}",
            body: "<p>Scan status: <b>${currentBuild.currentResult}</b></p>",
            attachmentsPattern: 'audit.log'
          )
        }
      }
    }

    stage('Deploy to Staging') {
      steps {
        echo 'Deploying to staging…'
        bat 'echo (deployment step here)'
      }
    }

    stage('Staging Integration Tests') {
      steps {
        echo 'Running staging integration tests…'
        bat 'echo (staging tests here)'
      }
    }
  }

  post {
    success {
      echo 'Pipeline completed successfully!'
    }
  }
}
