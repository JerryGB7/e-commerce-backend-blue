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
    stage('Test') {
      steps {
        container('maven') {
          sh 'mvn --version'
        }
      }
    }
    // stage('Build') {
    //   steps {
    //     container('maven') {
    //       //sh 'mvn package'
    //     }
    //   }
    // }
    stage('SonarCloud analysis') {
        steps {       
            script {
                nodejs(nodeJSInstallationName: 'nodejs'){ 
                  def scannerHome = tool 'sonar scanner';             
                  withSonarQubeEnv('SonarCloud') { 
                    // container('maven') {
                    //     sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    // }
                      sh "${scannerHome}/bin/sonar-scanner"
                  }
                }
            }
        }
    }
    stage('Quality gate') {
        steps {
            script {
                timeout(time: 1, unit: 'HOURS') {
                  def qg = waitForQualityGate()
                  if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                  }
                }
            }
        }
    }
    stage('Docker Build & Push') {
      steps {
        container('docker') {
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
            //sh 'docker version'
            //sh 'docker build -t othom/e-commerce-backend-blue:latest .'
            sh 'docker login -u ${username} -p ${password}'
            //sh 'docker push othom/e-commerce-backend-blue:latest'
            //sh 'docker logout'
          }
        }
      }
    }    
    stage('Deploy Image to AWS EKS cluster') {
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
