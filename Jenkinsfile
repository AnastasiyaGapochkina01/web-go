def git_url = https://github.com/AnastasiyaGapochkina01/web-go.git
pipeline {
  agent {
    docker {
      image 'golang'
      label 'yandex'
      reuseNode true
      args '-u root --privileged'
    }
  }

  stages {
    stage('Checkout SCM') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: "${branch}"]], doGenerateSubmoduleConfigurations: false, extentions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jenkins-ssh-key', url: "$git_url"]]])
      }
    }
    stage('Build binary file for Linux and Windows') {
      parallel {
        stage('Windows'){
          steps {
            sh "CGO_ENABLED=1 GOOS=windows GOARCG=amd64 go build -o app.exe main.go"
            archiveArtifacts artifacts: 'app.exe', fingerprint: true, onlyIfSuccessful: true
          }
        }
        stage('Linux){
          steps {
            sh "CGO_ENABLED=1 GOOS=linux GOARCG=amd64 go build -o app main.go"
            archiveArtifacts artifacts: 'app', fingerprint: true, onlyIfSuccessful: true
          }
        }
      }
    }
  }
}
 
