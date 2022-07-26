pipeline {
  agent none
  
  stages {
        stage('java test') {
          agent {
            kubernetes {
                inheritFrom 'docker'
            }

          }
          stages {
               stage("build") {
                   steps {
                       sh 'echo "java version $(java --version)"'
                   }
               }
               stage("test") {
                   steps {
                       sh 'echo "java version $(java --version)"'
                   }
               }
          }
            
        }

        stage('Docker test') {
          agent {
            kubernetes {
                inheritFrom 'docker'
            }

          }
          stages {
               stage("build") {
                   steps {
                       sh 'echo "docker version $(docker --version)"'
                   }
               }
               stage("test") {
                   steps {
                       sh 'echo "docker version $(docker --version)"'
                   }
               }
          }
            
        }

        stage('kubectl test') {
          agent {
            kubernetes {
                inheritFrom 'kubectl'
            }

          }
          stages {
               stage("build") {
                   steps {
                       sh 'echo "kubectl version $(kubectl version --short --client)"'
                   }
               }
               stage("test") {
                   steps {
                       sh 'echo "kubectl version $(kubectl version --short --client)"'
                   }
               }
          }
            
        }
      
  }
    
}
