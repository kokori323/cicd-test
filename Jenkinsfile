pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git 'https://github.com/kokori323/cicd-test.git'
            }
        }
        

        stage('Run Tests') {
            steps {
                // Run tests in Docker container
                script {
                    
                    sh 'npm install'
                    sh 'npm test'
                            
                }
            }
        }
    }
    
}
