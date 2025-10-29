pipeline {
    agent any

    environment {
        REGISTRY = "harbor.mycompany.com"
        PROJECT = "my-app"
        IMAGE_NAME = "backend"
        HARBOR_CRED = credentials('harbor-cred')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/GodNowoon/jenkinstest.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                    docker build -t $REGISTRY/$PROJECT/$IMAGE_NAME:latest .
                    """
                }
            }
        }

        stage('Login & Push to Harbor') {
            steps {
                script {
                    sh """
                    echo $HARBOR_CRED_PSW | docker login $REGISTRY -u $HARBOR_CRED_USR --password-stdin
                    docker push $REGISTRY/$PROJECT/$IMAGE_NAME:latest
                    """
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker rmi $REGISTRY/$PROJECT/$IMAGE_NAME:latest || true"
            }
        }
    }
}
