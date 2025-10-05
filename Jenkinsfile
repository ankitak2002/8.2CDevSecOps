pipeline {
  agent any

  tools {
    nodejs 'Node18'   // Use the NodeJS version you configured in Jenkins Tools
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/ankitak2002/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        echo 'Installing npm dependencies...'
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running unit tests...'
        sh 'npm test 2>&1 | tee test-output.txt || true'
        archiveArtifacts artifacts: 'test-output.txt', onlyIfSuccessful: false
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Running npm audit security scan...'
        sh 'npm audit --json > audit.json || true'
        archiveArtifacts artifacts: 'audit.json', onlyIfSuccessful: false
        sh 'cat audit.json || true'
      }
    }
  }

  post {
    always {
      echo "Pipeline finished with result: ${currentBuild.currentResult}"
    }
  }
}
