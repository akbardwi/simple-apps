pipeline {
    agent { label 'host1-akbar' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/akbardwi/simple-apps.git'
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
                -Dsonar.host.url=http://172.23.4.116:9000 \
                -Dsonar.token=sqp_07def1d64712b18f6d1f6a37e8e51b859bcbf62e
                '''
            }
        }
        
        stage('Delivery') {
            steps {
                input message: 'Apakah sudah siap untuk di deploy ke production?', ok: 'Deploy Sekarang!'
            }
        }

        stage('Deploy') {
            steps {
                sh'''
                cd app
                docker compose up --build -d
                '''
            }
        }
    }
}