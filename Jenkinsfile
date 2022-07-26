pipeline {
  agent none
  
  stages {
        stage('java test') {
          agent { label 'alpine' }
          }
          stages {
               stage("build") {
                   steps {
                       sh 'echo "java version $(java --version)"'
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
          agent { label 'docker' }
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
          agent { label 'kubectl' }
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
