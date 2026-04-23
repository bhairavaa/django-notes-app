pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-note-app'
        IMAGE_TAG  = 'microdegree'
    }

    stages {

        stage('Code Cloning') {
            steps {
                echo 'Cloning the code'
                git branch: 'main',
                    url: 'https://github.com/bhairavaa/django-notes-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                echo 'Pushing image to DockerHub'

                withCredentials([usernamePassword(
                    credentialsId: 'dockerHub',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {

                    sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                    sh 'docker tag $IMAGE_NAME $dockerHubUser/$IMAGE_NAME:$IMAGE_TAG'
                    sh 'docker push $dockerHubUser/$IMAGE_NAME:$IMAGE_TAG'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully 🎉'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}