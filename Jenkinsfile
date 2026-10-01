pipeline {
    agent { label 'host1-sidki' }

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
                -Dsonar.host.url=http://172.23.4.111:9000 \
                -Dsonar.token=sqp_c8f7d54febb1a424339d58b37c50b81163251203
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