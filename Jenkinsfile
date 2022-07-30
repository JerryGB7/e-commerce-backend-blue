pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: maven
            image: maven:alpine
            command:
            - cat
            tty: true
          - name: kubectl
            image: gcr.io/cloud-builders/kubectl
            command:
            - cat
            tty: true
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock
        '''
    }  
  }
  stages {  
    stage('Build') {
      steps {
        container('maven') {
          sh 'mvn install'
        }
      }
    }
    stage('Test') {
      steps {
        container('maven') {
          sh 'mvn test'
        }
      }
    }
    stage('SonarCloud analysis') {
        steps {       
            script {
                nodejs(nodeJSInstallationName: 'nodejs'){ 
                  def scannerHome = tool 'sonar scanner';             
                  withSonarQubeEnv('SonarCloud') { 
                    sh "${scannerHome}/bin/sonar-scanner"
                  }
                }
            }
        }
    }
    stage('Quality gate') {
        steps {
            script {
                timeout(time: 5, unit: 'MINUTES') {
                  waitForQualityGate abortPipeline: true
                }
            }
        }
    }
    stage('Deliver') {
       steps {
         container('docker') {
           withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
             //sh 'docker version'
             //sh 'docker build -t othom/e-commerce-backend-blue:latest .'
             sh 'ls'
             dir("target") {
               sh "ls"
            }
             sh 'docker login -u ${username} -p ${password}'
             //sh 'docker push othom/e-commerce-backend-blue:latest'
             sh 'docker logout'
          }
        }
      }
    }    
    stage('Deploy') {
      steps {
         container('kubectl') {
             sh 'kubectl get pods --all-namespaces'          
         }
        
      }
    }
    
  }
  post {
      always {
        container('docker') {
          sh 'docker logout'
        }
      }
  }
    
}
