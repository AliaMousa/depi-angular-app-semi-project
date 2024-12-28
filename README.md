# AngularBasic

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 17.3.7.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.
*************************************************************************************************************************************************

# Best Practices for Angular Project

## Git Best Practices
1. **Private Repository**:
   - Use a private repository to protect the code and ensure secure collaboration among team members.

2. **Branching Strategy**:
   - Implement a branching strategy to streamline development and collaboration. Examples include:
     - **main**: Stable production-ready code.
     - **develop**: Active development branch.
     - **feature/\***: New features.
     - **bugfix/\***: Bug fixes.

3. **Pull Requests (PRs)**:
   - Enable pull requests for all branches to enforce code review before merging.
   - Add automated checks (e.g., linting, testing) as part of the PR process.

4. **.gitignore**:
   - Maintain a `.gitignore` file to exclude unnecessary files (e.g., `node_modules`, build artifacts) from the repository.

## Docker Best Practices
1. **Optimized Docker Practices**:
   - **Multi-Stage Build**:
     ```Dockerfile
     # Build Stage
     FROM node:18 AS build

     WORKDIR /usr/src/app
     COPY package*.json ./
     RUN npm install
     COPY . .
     RUN npm run build --prod

     # Serve Stage
     FROM nginx:1.25-alpine
     COPY --from=build /usr/src/app/dist/angular-basic/browser /usr/share/nginx/html

     EXPOSE 80
     CMD ["nginx", "-g", "daemon off;"]
     ```
     - The first stage (`node:18`) builds the Angular app.
     - The second stage (`nginx:1.25-alpine`) serves the static content.
     - This minimizes the final image size and optimizes performance.

   - **Using Alpine-based Images**:
     - Reduces the image size and improves deployment efficiency.

   - **Exposing Ports**:
     - Declare `EXPOSE 80` to clarify the port used by the application.

## Jenkins CI Pipeline Best Practices
1. **Pipeline as Code**:
   - Use a `Jenkinsfile` to define the CI/CD pipeline. Example:
     ```
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
     ```
<authorizationStrategy class="hudson.security.FullControlOnceLoggedInAuthorizationStrategy">
 <securityRealm class="hudson.security.HudsonPrivateSecurityRealm">
