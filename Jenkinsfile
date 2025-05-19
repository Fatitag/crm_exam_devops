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
                    // Pass the token via -Dsonar.login
                    sh "sonar-scanner -Dsonar.projectKey=crm_exam_devops -Dsonar.sources=./app -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Déploiement') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}

