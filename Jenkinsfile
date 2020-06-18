
def docker_host = 'unix:///var/run/docker.sock'
docker_host = 'tcp://192.168.1.18:2375'


//remote docker 호스트가 될 곳의 docker 설정을 다음 참고하여 설정
//https://gist.github.com/styblope/dc55e0ad2a9848f2cc3307d4819d819f
def remote_docker_host   = 'tcp://192.168.1.18:2375'
//def sns_info_channel_id  = '1'
//def sns_error_channel_id = '1'
def registry_url = "192.168.1.18:5000"
def git_branch = "master"

pipeline {
  agent any
  stages {
    stage('source') {
      steps {
        //git(url: 'git@github.com:tellamon/spring-vue-test.git', branch: 'master')
        checkout changelog: true, poll: true, scm: [
            $class: 'GitSCM',
            branches: [[name: "origin/${git_branch}"]],
            doGenerateSubmoduleConfigurations: false,
            submoduleCfg: [],
            userRemoteConfigs: [[
                name: 'origin',
                url: "git@github.com:tellamon/spring-vue-test.git"
            ]]
        ]
        println("branch_name : origin/${git_branch}")        
      }
    }

    stage('build & publish') {
      steps {
        sh "docker-compose build"
      }
    }

    stage('restart in staging') {
      steps {
        sh """
          docker-compose down
          docker-compose up -d
        """
      }
    }

  }
}