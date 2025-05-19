pipeline {
    agent any

    stages {
        stage('Cloner le repo') {
            steps {
                git 'https://github.com/fatitag/crm_exam_devops.git'
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
                    sh 'sonar-scanner -Dsonar.projectKey=crm_exam_devops -Dsonar.sources=./app'
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
