pipeline {
    agent any

    stages {

        stage('git checkout') {
            steps {
                git url: 'https://github.com/sunandgmohan/nodedemobackend.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-app .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker stop node-app || true'
                sh 'docker rm node-app || true'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 98:3000 --name node-app node-app'
            }
        }
    }
}


