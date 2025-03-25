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
        stage('Compilation & Tests') {
            parallel {
                stage('Build') {
                    steps {
                        echo "Compilation en cours..."
                        sh 'sleep 3' // Simulation du build
                    }
                }
                stage('Tests Unitaires') {
                    steps {
                        echo "Exécution des tests unitaires..."
                        sh 'sleep 2' // Simulation des tests unitaires
                    }
                }
                stage('Quality Analysis') {
                    steps {
                        echo "Analyse statique du code avec SonarQube..."
                        sh 'sleep 4' // Simulation de l’analyse
                    }
                }
            }
        }
        stage('Failed Build') {
            steps {
                script {
                    try {
                        echo "Compil en cours..."
                        sh 'exit 1'
                    } catch(Exception e) {
                        echo "Exception caught"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        } 
        post {
            failure {
                echo "Pipeline Failed"
                sh 'echo "Pipeline failed" > error.log'
                archiveArtifacts artifacts: 'error.log', fingerprint: true
            }
            success {
                echo "Pipeline succeeded"
            }
        }
    } 
}
