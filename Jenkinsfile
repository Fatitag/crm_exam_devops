pipeline {
    agent any

    environment {
        // Inject your SonarQube token securely from Jenkins credentials (replace 'sonarqube-token' with your credential ID)
        SONAR_TOKEN = credentials('sonarqube-token')
    }

    stages {
        stage('Cloner le repo') {
            steps {
                git branch: 'main', url: 'https://github.com/fatitag/crm_exam_devops.git'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t crm_frontend .'
            }
        }

        stage('Analyse SonarQube') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    // Utiliser chemin absolu de sonar-scanner pour éviter erreur 'not found'
                    sh "/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=crm_exam -Dsonar.sources=./app -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Déploiement') {
            steps {
                sh 'docker run -d -p 8081:80 --name crm_app crm_frontend'
            }
        }
    
}
