pipeline {
    agent any
    
    environment {
        PATH = "${tool 'docker'}/bin:${env.PATH}"
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    
    tools {
        dockerTool 'docker' // Assurez-vous que 'docker' correspond à l'installation dans Jenkins
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
                    dockerImage.run('-d --name webapp -p 9090:80')
                }
            }
        }
    }

    post {
        success {
            emailext (
                subject: 'Build Success',
                body: 'The build was successful!',
                to: 'yann.mourier26@gmail.com'
            )
        }
        failure {
            emailext (
                subject: 'Build Failed',
                body: 'The build failed. Please check the Jenkins console output for more details.',
                to: 'yann.mourier26@gmail.com'
            )
        }
    }
    triggers {
        githubPush()
    }
}
