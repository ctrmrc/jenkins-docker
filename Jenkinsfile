pipeline {
  agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker-jenkins-sony_vaio')
    Password = credentials('12345')
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          sudo usermod -aG docker ${USER}
          su - ${USER}
          echo $Password
        '''
        sh 'docker build -t ctmrc/docker-jenkins:latest .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push ctmrc/docker-jenkins:latest'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
