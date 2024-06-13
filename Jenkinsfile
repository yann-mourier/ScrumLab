pipeline {
    agent any
    
    tools {
        // Définir l'outil Docker (assurez-vous que l'outil est configuré dans Jenkins)
        dockerTool 'docker'
    }

    environment {
        // Initialiser la variable d'environnement PATH pour inclure Docker
        PATH = "${tool('docker')}/bin:${env.PATH}"
    }
    
    stages {
        stage('Initialize') {
            steps {
                script {
                    // Afficher la version de Docker pour vérifier l'installation
                    sh 'docker --version'
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                // Vérifier le code source depuis le dépôt
                checkout scm
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déployer l'image Docker (ajustez la commande de déploiement selon vos besoins)
                    sh 'docker run -d -p 9090:80 dontrebootme/microbot:v1'
                }
            }
        }
    }

    post {
        always {
            // Actions à réaliser à la fin du pipeline, peu importe le résultat
            cleanWs()
        }
        success {
            // Notifications en cas de succès
            emailext (
                to: 'dipriceiquannu-6304@yopmail.com',
                subject: 'Build Success',
                body: 'The build was successful!'
            )
        }
        failure {
            // Notifications en cas d'échec
            emailext (
                to: 'dipriceiquannu-6304@yopmail.com',
                subject: 'Build Failed',
                body: 'The build failed. Please check the Jenkins console output for more details.'
            )
        }
    }
}
