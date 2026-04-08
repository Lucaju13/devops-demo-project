pipeline {
    agent any
    environment {
        DOCKERHUB_CREDS = credentials('dockerhub-credentials')
        GITHUB_TOKEN = credentials('github-token')
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
        stage('Update values.yaml') {
            steps {
                sh 'git checkout main'
                sh "sed -i 's/tag:.*/tag: v1.0/' helm/app/values.yaml"
                sh 'git config user.email "jenkins@ci.com"'
                sh 'git config user.name "Jenkins"'
                sh 'git add helm/app/values.yaml'
                sh 'git diff --cached --quiet || git commit -m "Update image tag to v1.0"'
                sh 'git push https://$GITHUB_TOKEN_USR:$GITHUB_TOKEN_PSW@github.com/Lucaju13/devops-demo-project main'
            }
        }
    }
}
