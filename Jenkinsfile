pipeline {
    agent m

    tools {
        maven 'Maven 3.8.5'
        jdk '17.0.12'
        sonarScanner 'sonar-scanner'  // Ensure this matches the SonarQube Scanner configuration in Jenkins
    }

    environment {
        EMAIL_ADMIN = 'melhenawy@ejada.com'
        EMAIL_DEVOPS = 'melhenawy@ejada.com'
        EMAIL_DEV_TEAM = 'melhenawy@ejada.com'
        POM_PATH = 'catalog-service'
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Starting the Checkout process"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Starting the Build process"
                script {
                    // Ensure that we're in the correct directory if needed
                    dir(env.POM_PATH) {   // Replace with the correct service directory if needed
                        bat 'mvn clean install'  // If you are on Linux, replace `bat` with `sh`
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Starting the SonarQube Analysis process"
                withSonarQubeEnv('MySonarQube') {  // Ensure 'MySonarQube' matches your SonarQube configuration in Jenkins
                    bat 'mvn sonar:sonar'  // Use Maven to run SonarQube analysis
                }
            }
        }

        stage('Test') {
            steps {
                echo "Starting the Test process"
                script {
                    dir(env.POM_PATH) {   // Replace with the correct service directory if needed
                        bat 'mvn test'  // If you are on Linux, replace `bat` with `sh`
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
