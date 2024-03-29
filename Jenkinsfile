 pipeline {
    agent any

    stages {
        stage('Clone Code from github') {
            steps {
                echo 'Cloning the code'
                git url: "https://github.com/TheoKamdem/django_notes_app.git", branch: "master"
            }
        }
        stage('Build') {
            steps {
                echo 'This is Build Stage'
                sh "docker build -t notes-app ."
            }
        }
        stage('Push to Docker hub') {
            steps {
                echo 'Pushing to Docker Hub'
                withCredentials([usernamePassword(credentialsId:"dockerhub-login",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser" )]){
                    sh "docker tag notes-app ${env.dockerhubuser}/notes-app:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerhubuser}/notes-app:latest"
                }
            }
        }
        stage('Deployment') {
            steps {
                echo 'Deploying container'
                // Assurez-vous que Docker est installé et que l'utilisateur Jenkins a les autorisations nécessaires pour exécuter des commandes Docker
                sh "docker-compose --version"
                // Assurez-vous que docker-compose.yml est présent dans le répertoire de travail de Jenkins
                sh "ls -l"
                // Exécutez docker-compose down pour arrêter les conteneurs existants
                sh "docker-compose down"
                // Exécutez docker-compose up pour démarrer les nouveaux conteneurs en mode détaché
                sh "docker-compose up -d"
                // Vous pouvez également inclure des commandes supplémentaires pour vérifier le statut du déploiement, par exemple :
                sh "docker ps -a"
            }
        }
    }
}
