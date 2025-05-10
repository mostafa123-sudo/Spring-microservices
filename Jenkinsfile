pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5'
        jdk '17.0.12'
    }

    environment {
        EMAIL_ADMIN = 'melhenawy@ejada.com'
        EMAIL_DEVOPS = 'melhenawy@ejada.com'
        EMAIL_DEV_TEAM = 'melhenawy@ejada.com'
        POM_PATH='catalog-service'

    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Ensure that we're in the correct directory if needed
                    dir(env.POM_PATH) {   // Replace with the correct service directory if needed
                        bat 'mvn clean install'  // If you are on Linux, replace `bat` with `sh`
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    dir(env.POM_PATH) {   // Replace with the correct service directory if needed
                        bat 'mvn test'  // If you are on Linux, replace `bat` with `sh`
                    }
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    dir(env.POM_PATH) {   // Replace with the correct service directory if needed
                        bat 'mvn package'  // If you are on Linux, replace `bat` with `sh`
                    }
                }
            }
        }
    }

    post {
        success {
            emailext(
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build succeeded on branch ${env.BRANCH_NAME}\nSee details at: ${env.BUILD_URL}",
                to: "${env.EMAIL_ADMIN}"
            )
        }

        failure {
            emailext(
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed on branch ${env.BRANCH_NAME}\nSee details at: ${env.BUILD_URL}",
                to: "${env.EMAIL_DEV_TEAM}, ${env.EMAIL_DEVOPS}"
            )
        }
    }
}
