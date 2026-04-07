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
                    sh 'docker build -t Lucaju13/demo-java-app:v1.0 .'
                    sh 'docker push Lucaju13/demo-java-app:v1.0'
                }
            }
        }
    }
}
