pipeline {

  agent {
      kubernetes {
          inheritFrom 'default'
      }
  }
  stages {
        stage('Environment Test') {
            steps {
              sh 'echo "Test the env"'
                
            }
        }
        stage('build image'){
          steps {
            sh 'echo " building blue app docker image"'
            

          }
            

	      }
        stage('Push image to DockerHub'){
            steps {
                
                sh 'echo " push blueapp image to dockerhub"'
                    
                
            }

          
            
        }
        stage("Cleaning up") {
            steps{
                sh 'echo "Cleaning up..."'
                
            }
        }
        

  }
    
}
