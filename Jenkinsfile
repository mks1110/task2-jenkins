pipeline {
    agent any
    environment {
        MYSQL_ROOT_PASSWORD = credentials("MYSQL_ROOT_PASSWORD")
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
                sed -e 's,{{password}}, '${MYSQL_ROOT_PASSWORD}',g;' db-password.yaml | kubectl apply -f -
                kubectl apply -f task2.yaml
                kubectl apply -f nginx-config.yaml
                kubectl apply -f nginx-pod.yaml
                sleep 60
                kubectl get services
                '''
            }

        }

    }

}