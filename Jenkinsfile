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
            image: bitnami/kubectl
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
  options {
        skipStagesAfterUnstable()
  }
  stages {  
    stage('Build') {
      steps {
        container('maven') {
          sh 'mvn -B -DskipTests clean package'
        }
      }
    }
    stage('Test') {
       steps {
         container('maven') {
           sh 'mvn test'
         }
       }
       post {
          always {
              junit 'target/surefire-reports/*.xml' 
          }
      }
    }
    /**stage('SonarCloud analysis') {
        steps {       
            script {
                // def scannerHome = tool 'sonar scanner';             
                withSonarQubeEnv('SonarCloud') { 
                    container('maven') {
                        sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    }
                    // sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
    stage('Quality gate') {
        steps {
            script {
                if (waitForQualityGate() != 'OK') {
                    echo 'fail quality gate'
                }
            }
        }
    }*/
    stage('Deliver') {
      steps {
        container('docker') {
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
            //sh 'docker version'
            //sh 'docker build -t othom/e-commerce-backend-blue:latest .'
            sh 'docker login -u ${username} -p ${password}'
            //sh 'docker push othom/e-commerce-backend-blue:latest'
          }
        }
      }
    }
    stage('Deploy') {
      steps {
        container('docker') {
          //withKubeConfig([credentialsId: 'aws-cred']) {
            sh 'docker run --rm --name kubectl bitnami/kubectl:latest get pod'
            
          //}
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
