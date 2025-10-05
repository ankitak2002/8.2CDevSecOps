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

  
 stage('SonarCloud Analysis') {
  environment { SONAR_TOKEN = credentials('SONAR_TOKEN') }
  steps {
    echo 'Running SonarCloud analysis...'
    bat '''
      curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip

      REM Extract the Sonar Scanner zip
      powershell -Command "Expand-Archive sonar-scanner.zip -DestinationPath sonar-scanner -Force"

      REM Find the extracted folder name dynamically
      for /d %%i in (sonar-scanner\\sonar-scanner-*) do set "SCANNER_DIR=%%i"

      REM Run Sonar Scanner using the resolved folder
      "%SCANNER_DIR%\\bin\\sonar-scanner.bat" -Dsonar.login=%SONAR_TOKEN%
    '''
  }
}
}

  post {
    always {
      echo "Pipeline finished with result: ${currentBuild.currentResult}"
    }
  }
}
