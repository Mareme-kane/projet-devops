pipeline { 
    agent any

    stages {
        stage('Cloner le code') {
            steps {
                git 'https://github.com/Mareme-kane/projet-devops.git'
            }
        }

        stage('Construire les images Docker') {
            steps {
                script {
                    bat 'docker-compose build'
                }
            }
        }

       



    stage('Push de l\'image Docker') {
            steps {
                script {
                    def registry = (ENV == 'dev' || ENV == 'staging') ? LOCAL_REGISTRY : REMOTE_REGISTRY
                    def imageTag = "${registry}/gestionEtablissement:${BUILD_NUMBER}"

                    bat "docker tag gestionEtablissement ${imageTag}"
                    bat "docker push ${imageTag}"
                    echo "✅ Image Docker poussée vers ${registry}"
                }
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline réussie et artéfact publié !'
        }
        failure {
            echo '⚠️ Erreur dans la pipeline, vérifiez les logs.'
        }
    }
}
