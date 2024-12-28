pipeline {
         agent any

         stages {
             stage('Checkout') {
                 steps {
                     checkout scm
                 }
             }

             stage('Install Dependencies') {
                 steps {
                     sh 'npm install'
                 }
             }

             stage('Build') {
                 steps {
                     sh 'npm run build --prod'
                 }
             }

             stage('Test') {
                 steps {
                     echo 'Testing the Angular application'
                     sh 'npm test'
                 }
             }

             stage('Docker Build & Push') {
                 environment {
                     DOCKER_IMAGE = 'my-angular-app'
                     DOCKER_REGISTRY = 'my-docker-registry'
                 }
                 steps {
                     sh 'docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE:latest .'
                     sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
                 }
             }

             stage('Deploy') {
                 steps {
                     sh 'docker run -d -p 80:80 $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
                 }
             }
         }

         post {
             always {
                 echo 'Pipeline completed!'
             }
         }
     }