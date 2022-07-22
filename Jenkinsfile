pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
    //Mohamed Account
    registry=''
    repo=''
    DOCKERHUB_CREDENTIALS=credentials('DOCKER_AUTH_ID')
    DOCKERHUB_REPO='${registry}/${repo}'
    TAG="${BUILD_NUMBER}" 
  } // environment

   stages {

    stage ('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          sh "docker build -t ${DOCKERHUB_REPO} ." 
        }
      }
    }
    stage('Pushing to Docker Hub(Mohamed Account)') {
     steps{  
         script {
                sh "docker push ${DOCKERHUB_REPO}"
         }
     }
    }

   }
    
}
