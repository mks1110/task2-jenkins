pipeline {
    agent any
    environment {
        YOUR_NAME = credentials("YOUR_NAME")
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t mks1011/task2-db db
                docker build -t mks1011/task2-app flask-app
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push mks1011/task2-db
                docker push mks1011/task2-app
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f .
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}