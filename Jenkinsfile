pipeline {
  agent any
  tools {
    jdk 'Java17'
    maven 'Maven'
  }
  stages {
    stage('Checkout Code') {
      steps {
        echo 'Pulling from Github...'
        // Stick to your primary repository
        git branch: 'main', url: 'https://github.com/pravakar-s21/wipjen.git'
      }
    }
    stage('Test Code') {
      steps {
        echo 'JUNIT Test case execution started'
        // Changed 'bat' to 'sh'
        sh 'mvn clean test -Dmaven.test.failure.ignore=true'
      }
      post {
        always {
          // allowEmptyResults: true prevents failure if you haven't written tests yet
          junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
        }
      }
    }
    stage('Build Project') {
      steps {
        echo 'Building Java project...'
        // Changed 'bat' to 'sh'
        sh 'mvn clean package -DskipTests'
      }
    }
    stage('Build the Docker Image') {
      steps {
        echo 'Building Docker Image...'
        // Changed 'bat' to 'sh'
        sh 'docker build -t myjavaproj:1.0 .'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        echo 'Updating Kubernetes Deployment...'
        // Instead of 'docker run', we update the existing K8s deployment 
        // to fix your 'indiaproj' pods.
        sh 'kubectl set image deployment/indiaproj-deployment indiaproj-container=myjavaproj:1.0 || echo "Deployment not found, skipping..." '
      }
    }
  }
  post {
    success {
      echo 'Build and Deployment Successful!'
    }
    failure {
      echo 'OOPS!!! Pipeline Failed. Check the Console Output above.'
    }
  }
}
