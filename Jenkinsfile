pipeline {
    agent {
        docker {
            image 'docker:stable'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Test Docker') {
            steps {
                sh 'docker --version'
                sh 'docker run hello-world'
            }
        }
    }
}
