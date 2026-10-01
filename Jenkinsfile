pipeline {
    agent { label 'host1-sidki' }
    environment {
        TOKEN_SONAR = credentials('token-sonar')
        HOST_SONAR  = credentials('host-sonar')
    }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/sidki91/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url={HOST_SONAR}\
                -Dsonar.token={TOKEN_SONAR}
                '''
            }
        }
        
        stage('Deliver') {
            steps{
                input message: 'Apakah anda yakin ingin untuk deploy ke production ?', ok: 'Deploy Sekarang'
            }
            
        }
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        
    }
}