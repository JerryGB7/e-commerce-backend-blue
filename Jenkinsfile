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
            steps {
              sh 'echo "Test the env"'
                
            }
        }
        stage('Dependencies') {
            steps {
              // Download the dependencies and plugins before we attempt to do any further actions
              sh(script: './mvnw --batch-mode dependency:resolve-plugins dependency:go-offline')
              // Save the dependencies that went into this build into an artifact. This allows you to review any builds for vulnerabilities later on.
              sh(script: './mvnw --batch-mode dependency:tree > dependencies.txt')
              archiveArtifacts(artifacts: 'dependencies.txt', fingerprint: true)
              // List any dependency updates.
              sh(script: './mvnw --batch-mode versions:display-dependency-updates > dependencieupdates.txt')
              archiveArtifacts(artifacts: 'dependencieupdates.txt', fingerprint: true)
            }
        }
        stage('build image'){
          steps {
            sh 'echo " building blue app docker image"'
            

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
                
            }
        }
        

  }
    
}
