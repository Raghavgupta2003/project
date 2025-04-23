pipeline {
    agent any

    stages {
        stage('Docker Cleanup') {
            steps {
                bat '''
                docker stop food || echo Container not running
                docker rm food || echo No container to remove
                docker rmi -f food || echo No image to remove
                '''
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t food .'
            }
        }
        stage('Run Container') {
            steps {
                bat 'docker run -d -p 3000:80 --name food food'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                        docker tag food %DOCKER_USER%/food
                        docker push %DOCKER_USER%/food
                    '''
                }
            }
        }        
    }
}
