pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh 'ls'
        sh './gradlew build'
      }
    }

    stage('mail notification') {
      steps {
        emailext(subject: 'test', body: 'test', to: 'dahoumeziane@gmail.com', attachLog: true)
      }
    }

    stage('Code analysis') {
      steps {
        withSonarQubeEnv('SonarQube Scanner') {
          sh 'sonar-scanner'
        }

        waitForQualityGate true
      }
    }

  }
}