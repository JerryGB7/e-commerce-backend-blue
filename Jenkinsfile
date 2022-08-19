pipeline {
  environment {
        registry = "jerrygb7/e-commerce"
        registryCredential = 'Dockerhub'
        dockerImage = ''
  }
  agent any
  stages {
    stage('cloning from git'){
      steps{
        git 'https://github.com/JerryGB7/e-commerce-backend-blue.git'
      } 
    }
    stage('stop docker containers'){
      steps{
        catchError(buildResult: 'SUCCESS'){
          sh 'docker stop e-commerce-app'
        }
        catchError(buildResult: 'SUCCESS'){
          sh 'docker rm -f e-commerce-app'
        }
      }
    }
    stage('Build the image'){
      steps{
        script{
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy our image'){
      steps{
        script{
          docker.withRegistry('', registryCredential){
            dockerImage.push()
          }
        }
      }
    }
    stage('Run image'){
      steps{
        script{
          docker.withRegistry('', registryCredential){
            sh "docker run -d -p 5001:5000 --name e-commerce-app $registry:$BUILD_NUMBER"
          }
        }
      }
    }
  }
}
