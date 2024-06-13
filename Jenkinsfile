pipeline {
    agent any
    
    environment {
        PATH = "${tool 'docker'}/bin:${env.PATH}"
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    
    tools {
        dockerTool name: 'docker', installationName: 'docker'
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
        always {
            node {
                cleanWs()
            }
        }
        success {
            emailext body: 'The build was successful!',
                     subject: 'Build Success',
                     to: 'yann.mourier26@gmail.com'
        }
        failure {
            emailext body: 'The build failed. Please check the Jenkins console output for more details.',
                     subject: 'Build Failed',
                     to: 'yann.mourier26@gmail.com'
        }
    }
    
    triggers {
        githubPush()
    }
}
