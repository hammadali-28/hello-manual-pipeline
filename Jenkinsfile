pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Yacer514/hello-manual.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('hello-manual-image')
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    docker.image('hello-manual-image').run('-p 3090:80')
                }
            }
        }
    }
}