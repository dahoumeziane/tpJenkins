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
        withSonarQubeEnv('Sonar') {
          sh './gradlew sonarqube'
        }

        script {
          def qualitygate = waitForQualityGate()
          if (qualitygate.status != "OK") {
            error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
          }
        }

      }
    }

  }
}