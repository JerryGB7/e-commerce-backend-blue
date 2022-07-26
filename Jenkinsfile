pipeline {
  agent none
  
  stages {
        stage('Maven test') {
          agent {
            kubernetes {
                inheritFrom 'maven-1'
            }
          }
          stages {
               stage("build") {
                   steps {
                       sh 'echo "maven version $(mvn --version)"'
                   }
               }
               stage("test") {
                   steps {
                       sh 'echo "maven version $(mvn --version)"'
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
