pipeline { 
    agent any 
    environment { 
        APP_NAME = 'my-app' 
        DOCKER_USER = 'bschroeder1978'
        ARTIFACT_NAME = 'app.tar.gz'
    }
    stages {
        stage('Login Docker') {
            steps {
                withCredentials([string(credentialsId: 'BSC_DOCKER_PASSWORD', variable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        stage('Build') { 
            steps { 
                script {
                    def buildVersion = "1.0.${env.BUILD_NUMBER}"
                    echo "Building ${APP_NAME} version ${buildVersion}"
                    sh 'echo "Contenu de l\'application" > app.txt'
                    sh 'tar -czf ${ARTIFACT_NAME} app.txt'
                    archiveArtifacts artifacts: ARTIFACT_NAME, fingerprint: true
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
                    echo "Déploiement de ${ARTIFACT_NAME}"
                }
            } 
        } 
    } 
}
