
def docker_host = 'unix:///var/run/docker.sock'
docker_host = 'tcp://192.168.1.18:2375'


//remote docker 호스트가 될 곳의 docker 설정을 다음 참고하여 설정
//https://gist.github.com/styblope/dc55e0ad2a9848f2cc3307d4819d819f
def remote_docker_host   = 'tcp://192.168.1.18:2375'
//def sns_info_channel_id  = '1'
//def sns_error_channel_id = '1'
def registry_url = "192.168.1.18:5000"
def git_branch = "master"
def github_access_key = "8c2086f34128811949d25d87ac85d69bfd2abb56"
def appName = "skns_rems"
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
                credentialsId: '002ef614-afb4-463d-a68f-5a0af0f729a3',
                name: 'origin',
                url: "git@github.com:tellamon/spring-vue-test.git"
            ]]
        ]
        println("branch_name : origin/${git_branch}")        
      }
    }

    stage('build & publish') {
      steps {
        sh """
          docker-compose -f docker-compose.was-a.yml build
          docker-compose -f docker-compose.was-a.yml push
        """
      }
    }

    stage('restart in staging') {
      steps {
          script{
            def CHECK_A_RUN = sh (
                script: "docker-compose -p ${appName} -f docker-compose.was-a.yml ps | grep Up",
                returnStatus: true
              ) == 0
            if(CHECK_A_RUN)
            {
              sh "docker-compose -p ${appName} -f docker-compose.was-b.yml up -d"
              sleep 10
              sh "docker-compose -p ${appName} -f docker-compose.was-a.yml down"
            }else {
              sh "docker-compose -p ${appName} -f docker-compose.was-a.yml up -d"
              sleep 10
              sh "docker-compose -p ${appName} -f docker-compose.was-b.yml down"              
            }
          }
      }
    }
  }
}