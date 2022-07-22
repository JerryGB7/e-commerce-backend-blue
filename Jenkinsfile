pipeline {

  agent {
      kubernetes {
          inheritFrom 'maven'
      }
  }
  stages {
        stage('Environment Test') {
            steps{
                sh 'echo "eksctl version $(eksctl version)"'
                sh 'echo "docker version $(docker --version)"'
                sh 'echo "kubectl version $(kubectl version --short --client)"'
                
            }
        }
        stage('build image'){
          steps {
            sh 'echo " building blue app docker image"'
            sh "chmod +x -R ${env.WORKSPACE}"

          }
            

	      }
        stage('Push image to DockerHub'){
            steps {
                withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
                    sh 'echo " push blueapp image to dockerhub"'
                    
                }
            }

          
            
        }
        stage("Cleaning up") {
            steps{
                sh 'echo "Cleaning up..."'
                sh 'docker system prune --force'
            }
        }
        

  }
    
}
