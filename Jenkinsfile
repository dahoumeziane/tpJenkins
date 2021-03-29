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
          post {
            failure {
              script {
                timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
              }
            }

          }

        }
        steps {
          withSonarQubeEnv('sonar') {
            sh './gradlew sonarqube'
          }

        }
      }

      stage('test reporting') {
        steps {
          cucumber 'reports/example-report.json'
        }
      }

    }
  }

  stage('Deployement') {
    steps {
      sh './gradlew publish'
    }
  }

  stage('Slack notification') {
    steps {
      slackSend(baseUrl: 'https://hooks.slack.com/services/', message: 'The app has been deployed successfully', token: 'T01MYCYURAQ/B01S7H1FUCF/FEdoYvax66P8MHS0wmPmj842')
    }
  }

}
}