pipeline {
    agent any

    //ต้องไปเพิ่ม หรือ install nodeJs ที่ jenkins UI ก่อน 
    //ถ้าโปรเจ็คเรามีการติดตั้งส่วนอื่น ให้เพิ่มเข้าไปด้วยเลยในส่วนชอง NodeJS installations >> Global npm packages to install
    tools {
        nodejs '22.1.0'
    }
    environment {
        DOCKER_IMAGE_NAME = 'kokori323/automation-test-101'
        DOCKERHUB_CREDENTIALS=credentials('docker-hub-kokori323')
        // GCP_CREDENTIALS = credentials('gcp-service-account-key')
        // GCP_PROJECT = 'test001-422905'
        // GCP_ZONE = 'us-central1-c'
        // GCP_INSTANCE = 'instance-20240519-125406'
    }

    //ใช้การอ่านโค้ดโปรเจ็คที่ github "https://github.com/kokori323/cicd-test.git" branch dev 
    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/kokori323/cicd-test.git'
                echo 'clone repository success'
            }
        }
        
        stage('Run Tests') {
            steps {
                //ในนี้จะต้องทำงานหรือใช้คำสั่งเหมือนกันในไลฟ์ package.json เช่น 
                //"scripts": {
                //     "start": "node index.js",
                //     "test": "jest --coverage"
                //   },
                //ก็ต้องใช้ sh เพื่อให้ jenkins ทำงานได้เหมือนกับการรันโปรเจ็คผ่าน local
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
        stage('Build Image Docker'){
            steps{
                sh "docker build -t ${DOCKER_IMAGE_NAME} . "
                echo 'build image success'
            }
        }

        stage('Login Dockerhub'){
            steps{
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                echo 'login dockerhub success'
            }
        }

        stage('Push Image'){
            steps{
                sh "docker push ${DOCKER_IMAGE_NAME}"
                echo 'push image success'
            }
        }

        // stage('Deploy to VM') {
        //     steps {
        //         withCredentials([file(credentialsId: 'gcp-service-account-key', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
        //             sh '''
        //             # Authenticate with GCP
        //             gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
        //             gcloud config set project $GCP_PROJECT

        //             # Copy docker-compose file to VM
        //             gcloud compute scp --zone $GCP_ZONE docker-compose.yml $GCP_INSTANCE:~/docker-compose.yml

        //             # SSH into VM and run docker-compose
        //             gcloud compute ssh $GCP_INSTANCE --zone $GCP_ZONE --command 'docker-compose -f ~/docker-compose.yml up -d'
        //             '''
        //             echo 'Deploy to VM success'
        //         }
        //     }
        // }
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
