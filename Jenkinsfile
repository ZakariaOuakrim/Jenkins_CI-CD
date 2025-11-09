pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'ouakrimzakaria/tp4'
        DOCKER_TAG = 'latest'
        CONTAINER_NAME = 'tp4-container-v3'
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
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
