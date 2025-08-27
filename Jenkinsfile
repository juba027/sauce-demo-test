pipeline {
  agent {
    docker {
      image 'maven:3.9.9-eclipse-temurin-17'
      args '-u 0:0'              // run as root pour apt-get
    }
  }

  environment {
    CHROME_BIN = '/usr/bin/chromium'
  }

  stages {
    stage('Install Chromium') {
      steps {
        sh '''
          set -eux
          apt-get update
          apt-get install -y chromium || apt-get install -y chromium-browser
        '''
        script {
          def bin = sh(script: "command -v chromium || command -v chromium-browser || true", returnStdout: true).trim()
          if (bin) { env.CHROME_BIN = bin }
          sh "${env.CHROME_BIN} --version || true"
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
