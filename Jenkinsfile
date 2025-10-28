pipeline {
    agent any

    tools {
        // Use the exact names from Global Tool Configuration
        jdk 'jdk17' 
        maven 'Maven_3_9_5' 
    }

    stages {
        stage('Checkout') {
            steps {
                // This pulls your GitHub repo into Jenkins workspace
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn -B clean test'
                        sh 'mvn -B package'
                    } else {
                        bat 'mvn -B clean test'
                        bat 'mvn -B package'
                    }
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                junit 'target/surefire-reports/*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Please check the logs.'
        }
    }
}
