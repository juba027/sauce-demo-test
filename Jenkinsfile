pipeline {
  agent {
    docker { image 'maven:3.9.9-eclipse-temurin-17' }
  }

  environment {
    CHROME_BIN = '/usr/bin/chromium'
  }

  stages {
    stage('Install Chromium') {
      steps {
        sh '''
          apt-get update
          apt-get install -y chromium || apt-get install -y chromium-browser || true
          (command -v chromium || command -v chromium-browser) && echo "Chromium OK"
        '''
        script {
          def bin = sh(script: "command -v chromium || command -v chromium-browser || true", returnStdout: true).trim()
          if (bin) { env.CHROME_BIN = bin }
        }
      }
    }

    stage('Clean and compile') {
      steps { sh 'mvn -B clean compile' }
    }

    stage('Run tests') {
      steps { sh 'mvn -B test' }
    }
  }

  post {
    always {
      junit 'target/surefire-reports/**/*.xml'
    }
  }
}
