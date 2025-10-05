pipeline {
  agent any

  tools {
    nodejs 'Node18'
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
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        echo 'Running unit tests...'
        bat 'npm test > test-output.txt 2>&1 || exit 0'
        archiveArtifacts artifacts: 'test-output.txt', onlyIfSuccessful: false
      }
    }

    stage('Security Scan') {
      steps {
        echo 'Running npm audit security scan...'
        bat 'npm audit --json > audit.json || exit 0'
        archiveArtifacts artifacts: 'audit.json', onlyIfSuccessful: false
        bat 'type audit.json'
      }
    }
  }

  post {
    always {
      echo "Pipeline finished with result: ${currentBuild.currentResult}"
    }
  }
}
