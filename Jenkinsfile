pipeline {
    agent any

    tools {
        maven 'maven'
    }

    environment {
        IMAGE_NAME   = 'student-management'
        IMAGE_TAG    = '1.0'
        DOCKER_CREDS = 'dockerhub-credentials'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t student-management:1.0 .'
            }
        }

        stage('Push DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: DOCKER_CREDS,
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh '''
                        echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin
                        docker tag student-management:1.0 $DOCKERHUB_USER/student-management:1.0
                        docker push $DOCKERHUB_USER/student-management:1.0
                    '''
                }
            }
        }
    }
}
