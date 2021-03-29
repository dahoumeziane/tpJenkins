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
      steps {
        withSonarQubeEnv('sonar') {
          sh './gradlew sonarqube'
        }

        waitForQualityGate true
      }
    }

  }
}