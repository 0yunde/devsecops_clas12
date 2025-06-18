pipeline {
  agent any
  tools { nodejs 'NodeJS_14' }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Install dependencies') {
      steps { sh 'npm install' }
    }
    stage('Run tests') {
      steps { sh 'npm test' }
    }
    stage('OWASP Dependency-Check') {
      steps {
        sh '''
          dependency-check \
            --project "SafeNotes" \
            --scan . \
            --format "HTML" \
            --out dependency-check-report.html
        '''
      }
    }
  }
  post {
    always {
      archiveArtifacts artifacts: 'dependency-check-report.html', fingerprint: true
      publishHTML([
        reportName: 'OWASP Dependency-Check',
        reportDir: '.',
        reportFiles: 'dependency-check-report.html',
        keepAll: true,
        alwaysLinkToLastBuild: true
      ])
    }
  }
}
