
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
        disableConcurrentBuilds()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Building branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Build') {
            steps {
                dir('complete') {
                    sh 'bash ./mvnw clean compile'
                }
            }
        }

        stage('Test') {
            steps {
                dir('complete') {
                    sh 'bash ./mvnw test'
                }
            }
        }

        stage('Package') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }
            steps {
                dir('complete') {
                    sh 'bash ./mvnw package -DskipTests'
                }
            }
        }

        stage('Archive JAR') {
            when {
                anyOf {
                    branch 'develop'
                    branch 'main'
                }
            }
            steps {
                archiveArtifacts(
                    artifacts: 'complete/target/*.jar',
                    fingerprint: true
                )
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Production deployment is not configured yet'
            }
        }
    }

    post {
        success {
            echo "SUCCESS: ${env.BRANCH_NAME}"
        }
        failure {
            echo "FAILED: ${env.BRANCH_NAME}"
        }
    }
}

