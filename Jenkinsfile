def remote = [:]
pipeline {
  agent any
  environment {
    HOST = "89.169.168.67"
    REPO = "zabella/go_server"
  }
  stages {
    stage('Configure credentials') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'jenkins_ssh_key', keyFileVariable: 'private_key', usernameVariable: 'username')]) {
          script {
            remote.name = "${env.HOST}"
            remote.host = "${env.HOST}"
            remote.user = "$username"
            remote.identity = readFile("$private_key")
            remote.allowAnyHosts = true
          }
        }
      }
    }
    stage('build') {
      steps {
      def Image = docker.build("${env.REPO}:${env.BUILD_ID}") 
        
      docker.withRegistry('https://registry-1.docker.io', 'hub_token') {
              Image.push()
        }
      }
    }
  }
}

