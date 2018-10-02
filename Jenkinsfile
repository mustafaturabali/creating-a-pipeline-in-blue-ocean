pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
    stage('SonarQube analysis') {
      steps {
        // requires SonarQube Scanner 2.8+
        withSonarQubeEnv('My SonarQube Server') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }
  }
  environment {
    CI = 'true'
    scannerHome = tool 'SonarQube Scanner 3.2.0.1227'
  }
}
