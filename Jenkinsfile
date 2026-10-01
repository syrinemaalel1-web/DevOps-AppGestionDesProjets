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

        stage('Push Docker Hub') {
            steps {
                echo 'Envoi de l image vers Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_TOKEN')]) {
                    sh '''
                        echo "$DH_TOKEN" | docker login -u "$DH_USER" --password-stdin
                        docker tag ${IMAGE_NAME}:${BUILD_NUMBER} $DH_USER/${IMAGE_NAME}:${BUILD_NUMBER}
                        docker tag ${IMAGE_NAME}:latest $DH_USER/${IMAGE_NAME}:latest
                        docker push $DH_USER/${IMAGE_NAME}:${BUILD_NUMBER}
                        docker push $DH_USER/${IMAGE_NAME}:latest
                        docker logout
                    '''
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
