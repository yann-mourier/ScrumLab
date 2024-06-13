pipeline {
    agent any
    
    environment {
        PATH = "${tool 'docker'}/bin:${env.PATH}"
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    
    tools {
        // Définir l'outil Docker (assurez-vous que l'outil est configuré dans Jenkins)
        dockerTool name: 'docker', installationName: 'docker'
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
                    // Utilisation du plugin Docker pour déployer une version spécifique de l'image
                    def dockerImage = docker.image('wordpress')
                    dockerImage.pull()  // Optionnel : télécharge l'image explicitement
                    dockerImage.run('-d --name webapp -p 9090:80')
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
