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
            echo "Checkout OK"
        }
    }

    stage('Clean') {
        steps {
            sh 'rm -rf target'
        }
    }

    stage('Build') {
        steps {
            sh 'mvn clean package -DskipTests=true'
        }
    }

    stage('Build Docker Image') {
        steps {
            sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
        }
    }

    stage('Push DockerHub') {
        steps {
            withCredentials([usernamePassword(credentialsId: DOCKER_CREDS,
                                              usernameVariable: 'DOCKERHUB_USER',
                                              passwordVariable: 'DOCKERHUB_PASS')]) {
                sh """
                    echo \$DOCKERHUB_PASS | docker login -u \$DOCKERHUB_USER --password-stdin
                    docker tag ${IMAGE_NAME}:${IMAGE_TAG} \$DOCKERHUB_USER/${IMAGE_NAME}:${IMAGE_TAG}
                    docker push \$DOCKERHUB_USER/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }
    }

    stage('Deploy') {
        steps {
            echo "Deploy OK"
        }
    }
}

post {
    always {
        echo "Pipeline terminé!"
    }
}

}
