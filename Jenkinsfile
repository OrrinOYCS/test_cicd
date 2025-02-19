pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t test_cicd:${BUILD_NUMBER} .'
      }
    }
    stage('Test') {
      steps {
        sh 'docker run test_cicd:${BUILD_NUMBER} pytest'
      }
    }
    stage('Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
          sh "docker login -u $USER -p $PASS"
          sh "docker push test_cicd:${BUILD_NUMBER}"
        }
      }
    }
  }
}