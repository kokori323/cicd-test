pipeline {
    agent any

    tools {
        nodejs 'Name_of_Your_NodeJS_Installation'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/kokori323/cicd-test.git'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh 'npm install'
                        sh 'npx jest --coverage'
                    } catch (err) {
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
            echo "Build completed"
        }
        success {
            echo "Tests passed successfully"
        }
        failure {
            echo "Build failed"
        }
    }
}
