pipeline { 
    agent any 
    environment { 
        APP_NAME = 'my-app' 
        DOCKER_USER = 'bsc-docker-username'
    }
    stages {
        stage('Login Docker') {
            steps {
                withCredentials([string(credentialsId: 'BSC_DOCKER_PASSWORD', variable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
    }
 
    stages { 
        stage('Build') { 
            steps { 
                script {
                    def buildVersion = "1.0.${env.BUILD_NUMBER}"
                    echo "Building ${APP_NAME} version ${buildVersion}"
                }
            } 
        } 
        stage('Test') { 
            steps { 
                echo "Testing ${APP_NAME}..."
            } 
        } 
        stage('Deploy') { 
            steps { 
                script {
                    def buildVersion = "1.0.${env.BUILD_NUMBER}"
                    echo "Deploying ${APP_NAME} version ${buildVersion}"
                }
            } 
        } 
    } 
}
