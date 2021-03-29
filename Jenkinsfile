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
        sh '/usr/local/Cellar/sonarqube/8.6.1.40680_1/libexec/bin/macosx-universal-64/sonar.sh start -Dproject.settings=sonar-project.properties'
      }
    }

  }
}