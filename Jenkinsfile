pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Docker Build & Push') {
            steps {
                sh 'docker build -t Lucaju13/demo-java-app:v1.0 .'
                sh 'docker push Lucaju13/demo-java-app:v1.0'
            }
        }
    }
}
