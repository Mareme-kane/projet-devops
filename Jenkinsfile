pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'projet-devops'
        DOCKER_COMPOSE_PATH = './docker-compose.yml'
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_SCANNER = 'SonarScanner'
    }

    stages {
        stage('Clone repository') {
            steps {
                git 'https://github.com/Mareme-kane/projet-devops.git'
            }
        }

        stage('Build Docker images') {
            steps {
                script {
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH build'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH run --rm gestion-classes vendor/bin/phpunit'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh '''
                            sonar-scanner \
                              -Dsonar.projectKey=projet-devops \
                              -Dsonar.sources=. \
                              -Dsonar.host.url=$SONAR_HOST_URL \
                              -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH run --rm terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH run --rm terraform apply -auto-approve'
                }
            }
        }

        stage('Push Docker images') {
            steps {
                script {
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH push'
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    sh 'docker-compose -f $DOCKER_COMPOSE_PATH up -d'
                }
            }
        }
    }

    post {
        always {
            sh 'docker-compose -f $DOCKER_COMPOSE_PATH down'
        }
    }
}
