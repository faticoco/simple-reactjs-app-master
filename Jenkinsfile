pipeline {
    agent any

    stages {
        stage('Check out') {
            steps {
                git 'https://github.com/faticoco/simple-reactjs-app-master.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("fatima71459/simple-reactjs-app:${env.BUILD_ID}")
                }
            }
        }

        stage('Run Docker Image') {
            steps {
                script {
                    dockerImage.run('-d -p 3000:3000 --name simple-reactjs-app')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '1008') {
                        dockerImage.push("${env.BUILD_ID}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker rm -f simple-reactjs-app || true'
            sh 'docker rmi fatima71459/simple-reactjs-app:${env.BUILD_ID} || true'
        }
    }
}
