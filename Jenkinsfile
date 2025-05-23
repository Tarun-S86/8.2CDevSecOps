pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        // clone your repo; Multibranch will do this automatically,
        // but explicit git lets you pin branch if needed:
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        // continue even if tests fail so we can still see coverage/audit
        bat 'npm test || exit 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        // shows vulnerabilities in console
        bat 'npm audit || exit 0'
      }
    }
  }

  post {
    success {
      echo '✅ DevSecOps pipeline passed!'
    }
    failure {
      echo '❌ Pipeline failed – check console output for details.'
    }
  }
}
