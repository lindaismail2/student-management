pipeline {
    agent any

    tools {
        // Le nom doit correspondre à la configuration de Maven dans Jenkins
        maven 'maven' 
    }

    stages {

        stage('Checkout') {
            steps {
                echo "🎉 Étape 1: Préparation de l'environnement"
                // Commande d'affichage simple - bat remplacé par sh
                sh "echo Checkout OK" 
            }
        }

        stage('Clean') {
            steps {
                echo "🧹 Nettoyage du dossier target"
                // Commande Windows 'rmdir /s /q target' remplacée par la commande Linux 'rm -rf target'
                sh "rm -rf target" 
            }
        }

        stage('Build') {
            steps {
                echo "🔨 Build du projet avec Maven"
                // La commande Maven reste la même, mais elle est exécutée via sh
                sh "mvn clean package -DskipTests=true" 
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Tests ignorés pour le moment"
                // Commande d'affichage simple - bat remplacé par sh
                sh "echo Tests skipped"
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Déploiement simulé"
                // Commande d'affichage simple - bat remplacé par sh
                sh "echo Deploy OK"
            }
        }
    }

    post {
        always {
            echo "✔️ Pipeline terminé!"
        }
    }
}
