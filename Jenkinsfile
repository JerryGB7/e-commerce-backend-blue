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
  stages {  
    stage('Test') {
      steps {
        container('maven') {
          sh 'mvn --version'
        }
      }
    }
    stage('Build') {
      steps {
        container('maven') {
          //sh 'mvn package'
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
            sh 'docker logout'
          }
        }
      }
    }
    stage('Deploy Image to AWS EKS cluster') {
      steps {
        container('kubectl') {
          //withKubeConfig([credentialsId: 'aws-cred']) {
            sh 'docker run --rm --name kubectl bitnami/kubectl:latest version'
            
          //}
        }
        
      }
    }
    
  }
    
}
