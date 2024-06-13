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

        stage('Build') {
            steps {
                script {
                    // Construire l'image Docker (ajustez le Dockerfile et le contexte selon vos besoins)
                    sh 'docker build -t myapp:latest .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Exécuter des tests (ajustez la commande de test selon vos besoins)
                    sh 'docker run --rm myapp:latest ./run_tests.sh'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Déployer l'image Docker (ajustez la commande de déploiement selon vos besoins)
                    sh 'docker push myapp:latest'
                }
            }
        }
    }

    post {
        always {
            // Nettoyer les ressources après l'exécution du pipeline
            script {
                sh 'docker system prune -f'
            }
        }
        success {
            // Actions à effectuer en cas de succès
            echo 'Pipeline completed successfully!'
        }
        failure {
            // Actions à effectuer en cas d'échec
            echo 'Pipeline failed!'
        }
    }
}
