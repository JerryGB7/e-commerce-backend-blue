pipeline {

  agent {
      kubernetes {
          inheritFrom 'alpine'
      }
  }
  stages {
        stage('Environment Test') {
            steps {
              sh 'echo "Java version $(java --version)"'
              sh 'echo "docker version $(docker --version)"'
              // sh 'echo "eksctl version $(eksctl version)"'
              // sh 'echo "kubectl version $(kubectl version --short --client)"'
              // sh 'echo "Hadolint version $(hadolint --version)"'
                
            }
        }
        stage('build image'){
          steps {
            sh 'echo " building docker image"'
            sh "chmod +x -R ${env.WORKSPACE}"
            // sh "docker build -t mshmsudd/e-commerce ."
          }
            

	      }
        stage('Push image to DockerHub'){
            steps {
                
                sh 'echo " push image to dockerhub"'
            }

          
            
        }
        stage("Cleaning up") {
            steps{
                sh 'echo "Cleaning up..."'
                
            }
        }
      
  }
  post {
        success {
            discordSend description: "CI/CD Pipeline", footer: "Footer Text", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "https://discord.com/api/webhooks/993653688251977870/jBKI7wwzebBdfEymLf0hLoR3H3yYhPXuM56ZBrNEvydLeP8vzrhC2_-x2r4iHehACRmf"
        }
  }
    
}
