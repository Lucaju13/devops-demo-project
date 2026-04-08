pipeline {
    agent any
    environment {
        DOCKERHUB_PASSWORD = credentials('dockerhub-credentials')
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
                    sh 'docker login -u lucaju13 -p ${DOCKERHUB_PASSWORD}'
                    sh 'docker push lucaju13/demo-java-app:v1.0'
                }
            }
        }
    }
}
