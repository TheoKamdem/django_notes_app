pipeline {
    agent any

    stages {
        stage('Clone Code from GitHub') {
            steps {
                echo 'Cloning the code'
                git url: "https://github.com/TheoKamdem/django_notes_app.git", branch: "master"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                sh "docker build -t notes-app ."
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing to Docker Hub'
                withCredentials([usernamePassword(credentialsId: "dockerhub-login", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh "docker tag notes-app ${env.dockerhubuser}/notes-app:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/notes-app:latest"
                }
            }
        }
        
        stage('Deployment') {
            steps {
                echo 'Deploying container'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
