pipeline {
agent any

```
tools {
    maven 'maven'        // Nom EXACT de Maven dans Jenkins (Manage Jenkins > Tools)
    jdk   'JAVA_HOME'    // Nom EXACT du JDK dans Jenkins (ex: 'jdk17' si c'est ça chez toi)
}

environment {
    GIT_BRANCH = 'main'
    GIT_URL    = 'https://github.com/lindaismail2/student-management.git'
    IMAGE_NAME = 'student-management'
    IMAGE_TAG  = '1.0'
    // Credentials Jenkins -> DockerHub (Username/Password)
    DOCKER_CREDS = 'dockerhub-credentials'
}

stages {

    stage('Checkout') {
        steps {
            echo "📥 Clone du repository..."
            git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
            echo "✅ Clone terminé"
        }
    }

    stage('Clean') {
        steps {
            echo "🧹 Nettoyage du dossier target"
            sh "rm -rf target"
        }
    }

    stage('Build (Compile)') {
        steps {
            echo "🔨 Compilation Maven (sans tests)"
            sh "mvn clean compile -DskipTests"
            echo "✅ Compilation terminée"
        }
    }

    stage('Test') {
        steps {
            echo "🧪 Tests ignorés pour le moment"
            sh "echo Tests skipped"
        }
    }

    stage('Package JAR') {
        steps {
            echo "📦 Packaging final en JAR"
            sh "mvn clean package -DskipTests"
            echo "✅ JAR prêt"
        }
    }

    stage('Archive Artifact') {
        steps {
            echo "📁 Archivage du JAR"
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }

    stage('Build Docker Image') {
        steps {
            echo "🐳 Build de l'image Docker"
            sh """
               docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
            """
            echo "✅ Image Docker construite"
        }
    }

    stage('Push Docker Image') {
        steps {
            echo "🚀 Push vers Docker Hub"
            withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDS}",
                                             usernameVariable: 'DOCKERHUB_USER',
                                             passwordVariable: 'DOCKERHUB_PASS')]) {

                sh """
                   echo \$DOCKERHUB_PASS | docker login -u \$DOCKERHUB_USER --password-stdin
                   docker tag ${IMAGE_NAME}:${IMAGE_TAG} \$DOCKERHUB_USER/${IMAGE_NAME}:${IMAGE_TAG}
                   docker push \$DOCKERHUB_USER/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
            echo "✅ Push terminé"
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
    success {
        echo "🎉 Pipeline terminé avec succès"
    }
    failure {
        echo "❌ Le pipeline a échoué"
    }
    always {
        echo "✔️ Pipeline terminé!"
    }
}
```

}
