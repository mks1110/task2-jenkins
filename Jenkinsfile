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
                docker build -t mks1011/task2-nginx nginx
                docker build -t mks1011/task2-app flask-app
                '''
            }

        }
        stage('Push') {
            steps {
                sh '''
                docker push mks1011/task2-db
                docker push mks1011/task2-nginx
                docker push mks1011/task2-app
                '''
            }

        }
        stage('Deploy') {
            steps {
                sh '''
                ssh jenkins@michaela2-deploy <<EOF
                export YOUR_NAME=${YOUR_NAME}
                docker network rm task2-net && echo "removed network" || echo "network already removed"
                docker network create task2-net
                docker stop nginx && echo "stopped nginx" || echo "nginx is not running"
                docker rm nginx && echo "removed nginx" || echo "nginx does not exist"
                docker stop flask-app && echo "stopped flask-app" || echo "flask-app is not running"
                docker rm flask-app && echo "removed flask-app" || echo "flask-app does not exist"
                docker stop db && echo "stopped db" || echo "db is not running"
                docker rm db && echo "removed db" || echo "db does not exist"
                docker run -d --name db --network task2-net -e MYSQL_ROOT_PASSWORD=supersecret-password mks1011/task2-db
                docker run -d --name flask-app --network task2-net -e MYSQL_ROOT_PASSWORD=supersecret-password -e YOUR_NAME=${YOUR_NAME} mks1011/task2-app
                docker run -d --name nginx --network task2-net -p 80:80 mks1011/task2-nginx
                '''
            }

        }

    }

}