pipeline {
    agent any

    tools {
        maven 'Maven 3.8.5'
        jdk 'JDK 17'
    }

    environment {
        EMAIL_ADMIN = 'melhenawy@ejada.com'
        EMAIL_DEVOPS = 'melhenawy@ejada.com'
        EMAIL_DEV_TEAM = 'melhenawy@ejada.com'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
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
