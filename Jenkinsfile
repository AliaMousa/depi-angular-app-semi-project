pipeline {
    agent {
             label 'master'
    }
    environment {
        DOCKER_IMAGE = 'depi-angular-app-jenkins'
        DOCKER_REGISTRY = 'alia98'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') { // Removed misplaced closing brace
            steps {
                sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop $DOCKER_IMAGE || true
                    docker rm $DOCKER_IMAGE || true
                    docker run -d --name $DOCKER_IMAGE -p 8000:80 $DOCKER_REGISTRY/$DOCKER_IMAGE:latest
                '''
            }
        }

        stage('Push') {
            steps { 
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh '''
                            echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin
                            docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
    }
}
