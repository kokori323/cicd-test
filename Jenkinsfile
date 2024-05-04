pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git 'https://github.com/your/repository.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image for Node.js project
                script {
                    docker.build('your-nodejs-image')
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests in Docker container
                script {
                    docker.image('your-nodejs-image').withRun('-p 3000:3000') { c ->
                        docker.inside("--workdir=/app --entrypoint='' ${c.id}") {
                            sh 'npm install'
                            sh 'npm test'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker resources
            script {
                docker.image('your-nodejs-image').clean()
            }
        }
    }
}
