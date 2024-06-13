pipeline {
    agent any
    
    tools {
        docker 'docker'
    }

    environment {
        PATH = "${tool 'docker'}/bin:${env.PATH}"
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    
    stages {
        stage('Initialize') {
            steps {
                script {
                    sh 'docker --version'
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Stop and Remove Old Container') {
            steps {
                script {
                    sh 'docker stop webapp || true'
                    sh 'docker rm webapp || true'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def dockerImage = docker.image('wordpress')
                    dockerImage.pull()
                    dockerImage.run('-d -p 9090:80 --name webapp')
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            emailext body: 'The build was successful!',
                     subject: 'Build Success',
                     to: 'yann.mourier26@gmail.com' // Remplacez par votre propre adresse e-mail ou une adresse valide
        }
        failure {
            emailext body: 'The build failed. Please check the Jenkins console output for more details.',
                     subject: 'Build Failed',
                     to: 'yann.mourier26@gmail.com' // Remplacez par votre propre adresse e-mail ou une adresse valide
        }
    }
    
    triggers {
        githubPush()
    }
}
