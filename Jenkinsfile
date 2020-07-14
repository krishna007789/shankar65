pipeline {
  agent any
  tools { 
        maven 'Maven3'
        jdk 'Java'
  }
  stages {
    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace... */
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
        sh 'echo $USER'
        sh 'echo whoami'
      }
    }
    stage('Docker Build') {
      steps {
        sh 'docker build -t address-service .'
      }
    }
   
    stage('push image to ECR'){
      steps {
       withDockerRegistry(credentialsId: 'ecr:us-east-1:aws-credentials', url: 'http://453767435700.dkr.ecr.us-east-1.amazonaws.com/address-service') {
          sh 'docker tag address-service:latest 453767435700.dkr.ecr.us-east-1.amazonaws.com/address-service:latest'
          sh 'docker push 453767435700.dkr.ecr.us-east-1.amazonaws.com/address-service:latest'
        } 
      }
    }
  }
}
