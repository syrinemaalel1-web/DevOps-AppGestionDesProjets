pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devops-appgestion-backend'
    }

    stages {

        stage('Récupération du projet') {
            steps {
                echo 'Récupération du projet depuis GitHub...'
                checkout scm
            }
        }

        stage('Tests unitaires') {
            steps {
                echo 'Lancement des tests unitaires...'
                dir('backend') {
                    sh './mvnw test'
                }
            }
        }

        stage('Création du livrable') {
            steps {
                echo 'Création du livrable...'
                dir('backend') {
                    sh './mvnw package -DskipTests'
                }
            }
        }

        stage('Build Image Docker') {
            steps {
                echo 'Construction de l image Docker...'
                dir('backend') {
                    sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .'
                }
            }
        }
    }

    post {
        success {
            echo 'Construction terminée avec succès.'
        }

        failure {
            echo 'La construction a échoué.'
        }
    }
}
