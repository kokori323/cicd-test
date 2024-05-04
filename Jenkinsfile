pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                git branch: 'dev', url: 'https://github.com/kokori323/cicd-test.git'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests in Docker container
                script {
                    try {
                        sh 'npm install'
                        sh 'npm test'
                    } catch (err) {
                        // Handle test failures or errors
                        echo "Tests failed: ${err.message}"
                        currentBuild.result = 'FAILURE'
                        throw err
                    }
                }
            }
        }
    }
    
    post {
        always {
            // Clean up or perform actions after the build
            echo "Build completed"
        }
        success {
            // Actions to take if the build is successful
            echo "Tests passed successfully"
        }
        failure {
            // Actions to take if the build fails
            echo "Build failed"
        }
    }
}
