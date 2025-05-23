pipeline {
  agent any

  stages {
    stage('1. Checkout') {
      steps {
        checkout scm
      }
    }

    stage('2. Install Dependencies') {
      steps {
        echo '⬇Installing dependencies…'
        bat 'npm install'
      }
    }

    stage('3. Run Tests') {
      steps {
        echo ' Running tests…'
        // Capture both stdout and stderr into test.log
        bat 'npm test > test.log 2>&1'
      }
      post {
        always {
          // Email the test.log to the dev team
          emailext(
            to: 'dev-team@example.com',
            subject: "Tests ${currentBuild.fullDisplayName}: ${currentBuild.currentResult}",
            body: """
              <p>Test stage finished with status: <b>${currentBuild.currentResult}</b></p>
              <p>See attached <b>test.log</b> for full output.</p>
            """,
            attachmentsPattern: 'test.log',
            mimeType: 'text/html'
          )
        }
      }
    }

    stage('4. Generate Coverage') {
      steps {
        echo 'Generating coverage report…'
        // Continue on error so coverage failures don’t abort the build
        bat 'npm run coverage > coverage.log 2>&1 || exit 0'
      }
    }

    stage('5. Security Scan') {
      steps {
        echo 'Running npm audit…'
        // Capture audit output into audit.log
        bat 'npm audit --audit-level=high > audit.log 2>&1 || exit 0'
      }
      post {
        always {
          // Email the audit.log to the security team
          emailext(
            to: 'security-team@example.com',
            subject: "Security Scan ${currentBuild.fullDisplayName}: ${currentBuild.currentResult}",
            body: """
              <p>Security scan finished with status: <b>${currentBuild.currentResult}</b></p>
              <p>See attached <b>audit.log</b> for vulnerability details.</p>
            """,
            attachmentsPattern: 'audit.log',
            mimeType: 'text/html'
          )
        }
      }
    }

    stage('6. Deploy to Staging') {
      steps {
        echo 'Deploying to staging…'
        // Placeholder
        bat 'echo Deploying app to staging environment'
      }
    }

    stage('7. Staging Integration Tests') {
      steps {
        echo 'Running integration tests on staging…'
        // Placeholder
        bat 'echo Running staging integration tests'
      }
    }
  }

  post {
    success {
      echo '✅ Pipeline completed successfully!'
    }
  }
}
