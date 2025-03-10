pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        timestamps()
    }

    environment {
        registry = 'quandvrobusto/house-price-prediction-api'
        registryCredential = 'dockerhub'
    }

    stages {
        stage('Test') {
            agent {
                docker {
                    image 'python:3.8'
                    args '-u root'  // Chạy container với quyền root để tránh lỗi permission
                }
            }
            steps {
                echo 'Testing model correctness..'
                sh 'pip install -r requirements.txt'
                sh 'pytest --maxfail=1 --disable-warnings'
            }
        }
        stage('Build') {
            steps {
                script {
                    echo 'Building image for deployment..'
                    def dockerImage = docker.build("${registry}:${BUILD_NUMBER}") // Fix lỗi Groovy
                    echo 'Pushing image to Docker Hub..'
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying models..'
                echo 'Running a script to trigger pull and start a docker container'
            }
        }
    }
}
