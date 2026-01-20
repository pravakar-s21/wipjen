pipeline {
    agent any
    stages {
        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Docker Build') {
            steps {
                // Building the image locally in Minikube's Docker env
                sh 'docker build -t myjavaproj:1.0 .'
            }
        }
        stage('Deploy to K8s') {
            steps {
                // Applying the manifests we created above
                sh 'kubectl apply -f db-k8s.yaml'
                sh 'kubectl apply -f app-k8s.yaml'
                // Force K8s to restart pods with the new image
                sh 'kubectl rollout restart deployment indiaproj-deployment'
            }
        }
    }
}
