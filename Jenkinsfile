
def docker_host = 'unix:///var/run/docker.sock'
docker_host = 'tcp://192.168.1.18:2375'
def remote_docker_host   = 'tcp://192.168.1.18:2375'
def sns_info_channel_id  = '1'
def sns_error_channel_id = '1'
def registry_url = "192.168.1.18"
def docker_tag = "master"

pipeline {
  agent any
  stages {
    stage('source') {
      steps {
        //git(url: 'git@github.com:tellamon/spring-vue-test.git', branch: 'master')
        checkout changelog: true, poll: true, scm: [
            $class: 'GitSCM',
            branches: [[name: "origin/${docker_tag}"]],
            doGenerateSubmoduleConfigurations: false,
            submoduleCfg: [],
            userRemoteConfigs: [[
                name: 'origin',
                url: "git@github.com:tellamon/spring-vue-test.git"
            ]]
        ]
        println("branch_name : origin/${docker_tag}")        
      }
    }

    stage('build') {
      steps {
        sh 'docker build'
      }
    }

    stage('deploy') {
      steps {
        sh 'echo "done"'
      }
    }

  }
}