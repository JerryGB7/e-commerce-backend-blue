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
          sh 'ls'
        }
      }
    }
    stage('Build') {
      steps {
        container('maven') {
          sh 'mvn --version'
          sh 'ls'
        }
      }
    }
    stage('Docker Build & Push') {
      steps {
        container('docker') {
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'password', usernameVariable: 'username')]) {
            sh 'docker version'
            sh 'docker build -t mshmsudd/e-commerce-backend-blue:latest .'
            // sh 'docker push -v /var/run/docker.sock:/var/run/docker.sock --privileged mshmsudd/e-commerce-backend-blue:latest'
          }
        }
      }
    }
    /**stage('Deploy Image to AWS EKS cluster') {
      steps {
        container('kubectl') {
          // withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cred', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
            sh 'kubectl version'
            sh 'ls'
            
          // }
        }
      }
    }*/
    
  }
  post {
        success {
            discordSend description: "CI/CD Pipeline", footer: "Footer Text", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "https://discord.com/api/webhooks/1001401336706895874/lkLUzH5kD1jEcBRIzNXw8gM2w97gtzxePk3OOqXCdLtejGzrCNNMOTUSCbgPf6fWpbVu"
        }
  }
    
}
