pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'projet-devops'  // Nom de l'image Docker
        DOCKER_COMPOSE_PATH = './docker-compose.yml'  // Chemin vers ton fichier Docker Compose
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/Mareme-kane/projet-devops.git'  // Ton dépôt GitHub
            }
        }

        stage('Build Docker images') {
            steps {
                script {
                    // Construire les images Docker pour chaque microservice
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Exécuter des tests unitaires (exemple avec PHPUnit)
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH run --rm gestion-classes vendor/bin/phpunit'
                    // Répéter pour les autres microservices si nécessaire
                }
            }
        }

        stage('Push Docker images') {
            steps {
                script {
                    // Pousser les images vers le registre Docker (local ou distant)
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH push'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    // Déploiement dans un environnement de staging via Docker ou Kubernetes
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH up -d'  // Si tu utilises Docker Compose pour staging
                    // Ou une commande kubectl si tu utilises Kubernetes
                }
            }
        }
    }

    post {
        always {
            // Nettoyer les ressources après le build (supprimer les containers, etc.)
            sh 'docker-compose -f $DOCKER_COMPOSE_PATH down'
        }
    }
}
