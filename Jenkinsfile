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
      environment {
        SONAR_SCANNER_OPTS = '-Xmx2g'
      }
      steps {
        sh 'pwd'
        sh '/opt/sonar-scanner/bin/sonar-scanner -Dproject.settings=sonar-project.properties'
      }
    }

  }
}