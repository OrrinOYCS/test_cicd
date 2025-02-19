pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t your-image:${BUILD_NUMBER} .'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run your-image:${BUILD_NUMBER} npm test'
      }
    }
    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh "docker login -u $USER -p $PASS"
          sh "docker push your-image:${BUILD_NUMBER}"
        }
      }
    }
    stage('Deploy') {
      steps {
        sh "kubectl set image deployment/your-app your-app=your-image:${BUILD_NUMBER}"
      }
    }
  }
}