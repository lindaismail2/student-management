pipeline {
agent any
tools {
    maven 'maven'
}

environment {
    IMAGE_NAME = "student-management"
    IMAGE_TAG  = "1.0"
    DOCKER_CREDS = "dockerhub-credentials"   // ID des credentials dans Jenkins
}

stages {

    stage('Checkout') {
        steps {
            echo "🎉 Étape 1: Préparation de l'environnement"
            sh "echo Checkout OK"
        }
    }

    stage('Clean') {
        steps {
            echo "🧹 Nettoyage du dossier target"
            sh "rm -rf target"
        }
    }

    stage('Build') {
        steps {
            echo "🔨 Build du projet avec Maven"
            sh "mvn clean package -DskipTests=true"
        }
    }

    stage('Test') {
        steps {
            echo "🧪 Tests ignorés pour le moment"
            sh "echo Tests skipped"
        }
    }

    stage('Build Docker Image') {
        steps {
            echo "🐳 Construction de l'image Docker"
            sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            sh "docker images | grep ${IMAGE_NAME} || true"
        }
    }

    stage('Push DockerHub') {
        steps {
            echo "🚀 Push de l'image vers DockerHub"

            withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}",
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
            echo "🚀 Déploiement simulé"
            sh "echo Deploy OK"
        }
    }
}

post {
    always {
        echo "✔️ Pipeline terminé!"
    }
}
```

}
