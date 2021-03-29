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
        withGradle() {
          publishChecks()
        }

      }
    }

    stage('Slack notification') {
      steps {
        slackSend(baseUrl: 'https://hooks.slack.com/services/', message: 'The app has been deployed successfully', token: 'T01MYCYURAQ/B01S7H1FUCF/FEdoYvax66P8MHS0wmPmj842')
      }
    }

  }
}