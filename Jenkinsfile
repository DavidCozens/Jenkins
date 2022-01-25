pipeline {
  environment {
    registry = "davidcozens/jenkins"
    registryCredential = 'jenkins-dockerhub'
    dockerImage = ''
  }

  agent any

  stages {
    stage('Building image') {
      steps{
        bitbucketStatusNotify(buildState: 'INPROGRESS')

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
}