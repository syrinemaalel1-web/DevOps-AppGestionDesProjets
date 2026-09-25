```groovy
pipeline {
    agent any

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
```

