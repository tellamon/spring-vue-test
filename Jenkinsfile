pipeline {
  agent any
  stages {
    stage('source') {
      steps {
        git(url: 'git@github.com:tellamon/spring-boot-vuejs.git', branch: 'master')
      }
    }

    stage('build') {
      steps {
        sh './mvnw clean install -DskipTests'
      }
    }

    stage('deploy') {
      steps {
        sh 'echo "done"'
      }
    }

  }
}