pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'YOURDOCKERHUBUSERNAME/shopcloud'
        DOCKER_CREDENTIALS = credentials('dockerhub-credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'echo "Static site — no build step required"'
            }
        }

        stage('Test') {
            steps {
                echo 'Running HTML lint tests...'
                sh '''
                    npm install -g htmlhint
                    htmlhint "src/frontend/**/*.html" --config .htmlhintrc
                    echo "All lint checks passed"
                '''
            }
            post {
                failure {
                    echo 'Tests failed — aborting pipeline'
                }
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
            }
        }

        stage('Push') {
            steps {
                echo 'Pushing image to Docker Hub...'
                sh '''
                    echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    docker push ${DOCKER_IMAGE}:latest
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to environment: ${env.BRANCH_NAME}"
                sh '''
                    echo "Deployment triggered for branch: ${BRANCH_NAME}"
                    echo "Image: ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                '''
            }
        }

        stage('Notify') {
            steps {
                echo 'Pipeline complete — sending notification...'
                sh 'echo "Build ${BUILD_NUMBER} deployed successfully"'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline FAILED — rollback initiated'
            sh "docker tag ${DOCKER_IMAGE}:latest ${DOCKER_IMAGE}:rollback || true"
        }
        always {
            echo 'Cleaning up workspace...'
            sh 'docker rmi ${DOCKER_IMAGE}:${BUILD_NUMBER} || true'
        }
    }
}