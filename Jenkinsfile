pipeline {
    agent any

    //ต้องไปเพิ่ม หรือ install nodeJs ที่ jenkins UI ก่อน 
    //ถ้าโปรเจ็คเรามีการติดตั้งส่วนอื่น ให้เพิ่มเข้าไปด้วยเลยในส่วนชอง NodeJS installations >> Global npm packages to install
    tools {
        nodejs '22.1.0'
    }

    //ใช้การอ่านโค้ดโปรเจ็คที่ github "https://github.com/kokori323/cicd-test.git" branch dev 
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
