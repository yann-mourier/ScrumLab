pipeline {
    agent any
    
    environment {
        // Indique à Jenkins d'utiliser le socket Docker partagé pour la communication Docker
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    
    stages {
        stage('Initialize') {
            steps {
                script {
                    // Vérification de la version de Docker
                    sh 'docker --version'
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                // Vérification du code source depuis le dépôt
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Utilisation du plugin Docker Pipeline pour lancer un conteneur
                    docker.image('dontrebootme/microbot:v1').run('-d -p 9090:80')
                }
            }
        }
    }

    post {
        always {
            // Nettoyage de l'espace de travail à la fin du pipeline
            cleanWs()
        }
        success {
            // Notification par email en cas de succès
            emailext (
                to: 'dipriceiquannu-6304@yopmail.com',
                subject: 'Build Success',
                body: 'Le build a réussi!'
            )
        }
        failure {
            // Notification par email en cas d'échec
            emailext (
                to: 'dipriceiquannu-6304@yopmail.com',
                subject: 'Build Failed',
                body: 'Le build a échoué. Veuillez vérifier la sortie de la console Jenkins pour plus de détails.'
            )
        }
    }
}
