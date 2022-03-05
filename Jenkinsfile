pipeline {
  environment {
    registry = "davidcozens/jenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }

  agent {
    docker {
      args '-v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -v /usr/bin/com.docker.cli:/usr/bin/com.docker.cli'
      image 'ubuntu'
      registryCredentialsId 'dockerhub'
    }
  }

  stages {
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}