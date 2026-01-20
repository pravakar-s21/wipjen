pipeline {
  agent any
  tools {
    jdk 'Java17'
    maven 'Maven'
  }
  stages {
    stage('Checkout Code') {
      steps {
        echo 'Pulling from Github'
        // Use your actual repo: https://github.com/pravakar-s21/wipjen.git
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
          // allowEmptyResults: true prevents the build from crashing if tests are missing
          junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
          echo 'Test stage completed.'
        }
      }
    }
    stage('Build Project') {
      steps {
        echo 'Building Java project'
        // Changed 'bat' to 'sh'
        sh 'mvn clean package -DskipTests'
      }
    }
    stage('Build the Docker Image') {
      steps {
        echo 'Building Docker Image'
        // Changed 'bat' to 'sh'
        sh 'docker build -t myjavaproj:1.0 .'
      }
    }
    stage('Run Docker Container') {
      steps {
        echo 'Running Java Application'
        // Changed 'bat' to 'sh'
        sh '''
        docker rm -f myjavaproj-container || true
        docker run -d --name myjavaproj-container myjavaproj:1.0
        '''               
      }
    }
  }
  post {
    success {
      echo 'Build and Run is SUCCESSFUL!'
    }
    failure {
      echo 'OOPS!!! Failure.'
    }
  }
}
