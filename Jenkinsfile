pipeline {
    agent any
    
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                // Compiles the Spring Boot app
                sh './mvnw clean package -DskipTests' 
            }
        }
        
        stage('Containerize') {
            steps {
                // Builds the Docker image based on your Dockerfile
                sh 'docker build -t petclinic-app:latest .'
            }
        }
        
        stage('Deploy') {
            steps {
                // Removes old containers to prevent port conflicts
                sh 'docker stop petclinic-container || true'
                sh 'docker rm petclinic-container || true'
                // Runs the new container, mapping server port 8081 to app port 8080
                sh 'docker run -d -p 8081:8080 --name petclinic-container petclinic-app:latest'
            }
        }
    }
}
