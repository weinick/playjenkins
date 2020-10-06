pipeline {

  environment {
    registry = "weiyunick/myweb"
	registryCredential = 'dockerhub'
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/weinick/playjenkins.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = sudo docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          sudo docker.withRegistry( "",registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
