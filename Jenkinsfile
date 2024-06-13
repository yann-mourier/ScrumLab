pipeline {
    agent any  // Use 'any' agent type to allow flexibility

    stages {
        stage('Deploy') {
            agent any  // Use 'any' agent type for deployment stage as well
            steps {
                script {
                    // Run Docker container for deployment
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