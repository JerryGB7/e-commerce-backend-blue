pipeline {

  tools {
    jdk 'Java'
  }
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

          }
            

	      }
        stage('Push image to DockerHub'){
          steps {

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
