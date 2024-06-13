pipeline {
    agent any
    environment {
        NODE_ENV = 'production'
    }
    stages {
        stage('Deploy Docker Image') {
            steps {
                script {
                    // Exécuter le conteneur Docker
                    sh 'docker run -d -p 9090:80 dontrebootme/microbot'
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
                to: 'nijunnanupra-6905@yopmail.com',
                subject: 'Build Success',
                body: 'The build was successful!'
            )
        }
        failure {
            // Notifications en cas d'échec
            emailext (
                to: 'nijunnanupra-6905@yopmail.com',
                subject: 'Build Failed',
                body: 'The build failed. Please check the Jenkins console output for more details.'
            )
        }
    }
}
