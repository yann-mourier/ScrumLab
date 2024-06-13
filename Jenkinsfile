pipeline {
    agent any
    
    environment {
        PATH = "${tool 'docker'}/bin:${env.PATH}"
        DOCKER_HOST = 'unix:///var/run/docker.sock'
        WEBAPP_PORT = '9090'
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
                    dockerImage.run('-d --name webapp -p ${WEBAPP_PORT}:80')
                }
            }
        }
    }

    stage('Unit Tests') {
            steps {
                script {
                    // Test 1: Vérifier si le conteneur est en cours d'exécution
                    def isRunning = sh(script: 'docker ps | grep webapp', returnStatus: true)
                    if (isRunning != 0) {
                        error 'Le conteneur webapp n\'est pas en cours d\'exécution'
                    }

                    // Test 2: Vérifier si l'application est accessible via HTTP
                    def response = sh(script: "curl -sSf http://localhost:${WEBAPP_PORT} >/dev/null && echo 'OK' || echo 'FAIL'", returnStatus: true)
                    if (response != 0) {
                        error 'Impossible d\'accéder à l\'application déployée'
                    }

                    // Test 3: Vérifier si l'application renvoie une réponse attendue
                    def appResponse = sh(script: "curl -sSf http://localhost:${WEBAPP_PORT} | grep 'wordpress'", returnStatus: true)
                    if (appResponse != 0) {
                        error 'L\'application ne renvoie pas la réponse attendue'
                    }

                    // Message si tous les tests unitaires réussissent
                    echo 'Tous les tests unitaires ont été exécutés avec succès !'
                }
            }
        }
    }

    post {
        success {
            mail bcc: '', body: 'The build was successful !', subject: 'Build Success !', to: 'yann.mourier26@gmail.com'
        }
        failure {
            mail bcc: '', body: 'The build failed. Please check the Jenkins console output for more details.', subject: 'Build Failed !', to: 'yann.mourier26@gmail.com'
        }
    }
    triggers {
        githubPush()
    }
}
