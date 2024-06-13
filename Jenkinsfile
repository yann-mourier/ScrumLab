pipeline {
    agent {
        docker {
            image 'node:14'  // Utilisez l'image Docker avec les outils nécessaires
            args '-v /var/run/docker.sock:/var/run/docker.sock'  // Montez le socket Docker dans le conteneur agent
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Exécuter le conteneur Docker depuis le conteneur Jenkins
                    sh 'docker run -d -p 9090:80 dontrebootme/microbot'
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            emailext (
                to: 'nijunnanupra-6905@yopmail.com',
                subject: 'Build Success',
                body: 'The build was successful!'
            )
        }
        failure {
            emailext (
                to: 'nijunnanupra-6905@yopmail.com',
                subject: 'Build Failed',
                body: 'The build failed. Please check the Jenkins console output for more details.'
            )
        }
    }
}
