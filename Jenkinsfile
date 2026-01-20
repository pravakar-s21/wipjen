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
                checkout scm
            }
        }
        stage('Test Code') {
            steps {
                echo 'JUNIT Test case execution started'
                // Use 'sh' for Linux. The '-Dmaven.test.failure.ignore=true' 
                // ensures the pipeline continues so JUnit can report the errors.
                sh 'mvn test -Dmaven.test.failure.ignore=true'
            }
            post {
                always {
                    // allowEmptyResults: true prevents the "Configuration error" crash
                    junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Build Project') {
            steps {
                echo 'Building the Maven Project...'
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build the Docker Image') {
            steps {
                echo 'Building Docker Image...'
                // This assumes you have a Dockerfile in your repo
                sh 'docker build -t indiaproj-app:${BUILD_NUMBER} .'
            }
        }
    }
    post {
        success {
            echo 'Build Successful!'
        }
        failure {
            echo 'OOPS!!! Failure.'
        }
    }
