pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'ouakrimzakaria/tp4'
        DOCKER_TAG = 'latest'
        CONTAINER_NAME = 'tp4-container-final'
        CONTAINER_PORT = '8083'
    }
    
    stages {
        stage('Cloning Git') {
            steps {
                echo 'Cloning repository...'
                git branch: 'master', url: 'https://github.com/ZakariaOuakrim/Jenkins_CI-CD'
            }
        }
        
        stage('Building image') {
            steps {
                echo 'Building Docker image...'
                script {
                    bat "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }
        
        stage('Test image') {
            steps {
                echo 'Testing Docker image...'
                script {
                    bat """
                        docker run --rm ${DOCKER_IMAGE}:${DOCKER_TAG} nginx -t
                        echo Image test passed!
                    """
                }
            }
        }
        
        stage('Publish Image') {
            steps {
                echo 'Pushing to Docker Hub...'
                script {
                    bat "docker login -u ouakrimzakaria -p dckr_pat_6I9sWuBiQUFLS6z1E0uPvIXZ868"
                    bat "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
        
        stage('Deploy image') {
            steps {
                echo 'Deploying container...'
                script {
                    bat """
                        docker stop ${CONTAINER_NAME} 2>nul || echo Container not running
                        docker rm ${CONTAINER_NAME} 2>nul || echo Container not found
                        docker pull ${DOCKER_IMAGE}:${DOCKER_TAG}
                        docker run -d -p ${CONTAINER_PORT}:80 --name ${CONTAINER_NAME} ${DOCKER_IMAGE}:${DOCKER_TAG}
                        echo Deployment complete! Access at http://localhost:${CONTAINER_PORT}
                        docker ps | findstr ${CONTAINER_NAME}
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
            echo 'Application is running at http://localhost:8083'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
