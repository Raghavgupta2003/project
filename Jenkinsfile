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

        stage('Build Image') {
            steps {
                bat 'docker build -t food .'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 3000:80 --name food food'
            }
        }
    }
}
