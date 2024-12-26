pipeline {
    agent any

    triggers {
        pollSCM('H/5 * * * *') // Poll every 5 minutes for changes in SCM
    }

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')  // DockerHub credentials from Jenkins
        IMAGE_NAME_SERVER = '[username]/mern-server'  // Replace [username] with your Docker Hub username
        IMAGE_NAME_CLIENT = '[username]/mern-client'  // Replace [username] with your Docker Hub username
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:RaniaKedri1/CI_with_jenkins.git', credentialsId: 'Github_ssh'
            }
        }

        stage('Build Server Image') {
            steps {
                dir('server') {
                    script {
                        dockerImageServer = docker.build("${IMAGE_NAME_SERVER}")
                    }
                }
            }
        }

        stage('Build Client Image') {
            steps {
                dir('client') {
                    script {
                        dockerImageClient = docker.build("${IMAGE_NAME_CLIENT}")
                    }
                }
            }
        }

        stage('Push Server Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        dockerImageServer.push()
                    }
                }
            }
        }

        stage('Push Client Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        dockerImageClient.push()
                    }
                }
            }
        }
    }
}
