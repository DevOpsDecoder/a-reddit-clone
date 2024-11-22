pineline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    enviornment {
        SCANNER_HOME = tools 'sonar-scanner'
        APP_NAME = "reddit-clone-app"
        RELEASE = "1.0.0"
        DOCKER_USER = "devopsdecoder"
        DOCKER_PASS = 'imran@778'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = ${RELEASE}-${BUILD_NUMBER}
    }
    stages {
        stage('clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from GIT') {
            steps {
                git branch: 'main', url: 'https://github.com/DevOpsDecoder/a-reddit-clone.git'
            }
        }
        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName = Reddit-Clone-CI \
                    -Dsonar.projectKey=Reddit-Clone-CI'''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    wiatForQualityGate abortPipeline: false, credentialsId: 'SonarQuble-Token'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        
    }
}