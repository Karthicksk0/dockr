pipeline {
    agent any

    environment {
        IMAGE_NAME = "demo-app"
        CONTAINER_NAME = "demo-app-container"
        PORT = "3000"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'pipelinegit',
                    url: 'https://github.com/Karthicksk0/dockr.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Run Tests') {
            steps {
                sh "docker run --rm $IMAGE_NAME node test.js"
            }
        }

        stage('Run Container') {
            steps {
                sh "docker rm -f $CONTAINER_NAME || true"
                sh "docker run -d --name $CONTAINER_NAME -p $PORT:3000 $IMAGE_NAME"
            }
        }
    }

    post {
        success {
            echo "Build and deployment successful! App running at http://localhost:$PORT/"
        }
        failure {
            echo "Build failed!"
        }
    }
}
