pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'ls'
        sh './gradlew --version'
      }
    }

    stage('mail notification') {
      steps {
        emailext(subject: 'test', body: 'test', to: 'dahoumeziane@gmail.com', attachLog: true)
      }
    }

    stage('Code analysis') {
      environment {
        SONAR_SCANNER_OPTS = '-Xmx2g'
      }
      steps {
        withSonarQubeEnv('SonarQube Scanner') {
          sh './gradlew sonarqube'
        }

      }
    }

  }
}