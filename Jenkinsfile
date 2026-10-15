
pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        APP_NAME = 'fullstack-backend'
        IMAGE_NAME = 'fullstack-backend'
        CONTAINER_PORT = '8000'
        HOST_PORT = '8000'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code from GitHub...'
                checkout scm
            }
        }

        stage('Validate Backend') {
            steps {
                echo 'Validating backend files...'

                sh '''
                    test -f backend/Dockerfile
                    test -f backend/requirements.txt
                    test -f backend/main.py

                    echo "Backend files validated successfully."
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                echo 'Building the FastAPI Docker image...'

                sh '''
                    docker build \
                        --pull \
                        -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                        -t ${IMAGE_NAME}:latest \
                        ./backend
                '''
            }
        }

        stage('Test Backend Image') {
            steps {
                echo 'Checking the Docker image...'

                sh '''
                    docker image inspect ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy Backend') {
            steps {
                echo 'Deploying the backend container to EC2...'

                sh '''
                    docker rm -f ${APP_NAME} 2>/dev/null || true

                    docker run -d \
                        --name ${APP_NAME} \
                        --restart unless-stopped \
                        -p ${HOST_PORT}:${CONTAINER_PORT} \
                        ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking backend availability...'

                sh '''
                    sleep 5

                    docker ps --filter "name=${APP_NAME}"

                    curl --retry 5 \
                         --retry-delay 3 \
                         --retry-connrefused \
                         -fsS http://localhost:8000/docs \
                         -o /dev/null

                    echo "Backend deployment verification passed."
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Backend pipeline completed.'
        }

        failure {
            echo 'FAILED: Check the Jenkins console output.'
            sh 'docker logs fullstack-backend 2>/dev/null || true'
        }
    }
}
pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    environment {
        APP_NAME = 'fullstack-backend'
        IMAGE_NAME = 'fullstack-backend'
        CONTAINER_PORT = '8000'
        HOST_PORT = '8000'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code from GitHub...'
                checkout scm
            }
        }

        stage('Validate Backend') {
            steps {
                echo 'Validating backend files...'

                sh '''
                    test -f backend/Dockerfile
                    test -f backend/requirements.txt
                    test -f backend/main.py

                    echo "Backend files validated successfully."
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                echo 'Building the FastAPI Docker image...'

                sh '''
                    docker build \
                        --pull \
                        -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                        -t ${IMAGE_NAME}:latest \
                        ./backend
                '''
            }
        }

        stage('Test Backend Image') {
            steps {
                echo 'Checking the Docker image...'

                sh '''
                    docker image inspect ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy Backend') {
            steps {
                echo 'Deploying the backend container to EC2...'

                sh '''
                    docker rm -f ${APP_NAME} 2>/dev/null || true

                    docker run -d \
                        --name ${APP_NAME} \
                        --restart unless-stopped \
                        -p ${HOST_PORT}:${CONTAINER_PORT} \
                        ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking backend availability...'

                sh '''
                    sleep 5

                    docker ps --filter "name=${APP_NAME}"

                    curl --retry 5 \
                         --retry-delay 3 \
                         --retry-connrefused \
                         -fsS http://localhost:8000/docs \
                         -o /dev/null

                    echo "Backend deployment verification passed."
                '''
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Backend pipeline completed.'
        }

        failure {
            echo 'FAILED: Check the Jenkins console output.'
            sh 'docker logs fullstack-backend 2>/dev/null || true'
        }
    }
}

