pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        sh './gradlew build'
        sh './gradlew javadoc'
        archiveArtifacts 'build/libs/*.jar'
        archiveArtifacts 'build/docs/javadoc/**/*.*'
        archiveArtifacts 'build/reports/tests/test/**/*.*'
      }
    }

    stage('mail notification') {
      steps {
        emailext(subject: 'test', body: 'test', to: 'dahoumeziane@gmail.com', attachLog: true)
      }
    }

    stage('Code analysis') {
      parallel {
        stage('Code analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              sh './gradlew sonarqube'
            }

            script {
              timeout(time: 4, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
              def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
              if (qg.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
            }
          }

        }
      }

      stage('Test reporting') {
        steps {
          cucumber 'reports/example-report.json'
        }
      }

    }
  }

}
}