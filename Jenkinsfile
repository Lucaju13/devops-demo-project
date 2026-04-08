pipeline {
    agent any
    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
            }
        }
        stage('Build') {
            steps {
                dir('demo-java-app') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                dir('demo-java-app') {
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage('Docker Build & Push') {
            steps {
                dir('demo-java-app') {
                    sh 'docker build -t lucaju13/demo-java-app:v1.0 .'
                    sh 'echo $DOCKERHUB_CREDS_PSW | docker login -u $DOCKERHUB_CREDS_USR --password-stdin'
                    sh 'docker push lucaju13/demo-java-app:v1.0'
                }
            }
        }
    }
}
