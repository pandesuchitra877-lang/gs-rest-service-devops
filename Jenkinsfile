pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'YOUR_GITHUB_REPO_URL'
            }
        }

        stage('Build') {
            steps {
                dir('complete') {
                    sh 'chmod +x mvnw'
                    sh './mvnw clean compile'
                }
            }
        }

        stage('Test') {
            steps {
                dir('complete') {
                    sh './mvnw test'
                }
            }
        }

        stage('Package') {
            steps {
                dir('complete') {
                    sh './mvnw package -DskipTests'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'complete/target/*.jar',
                                 fingerprint: true
            }
        }
    }
}
