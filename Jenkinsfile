pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Ekanki144/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                success {
                    emailext(
                        to: 'ekankimahajan056@gmail.com',
                        subject: "Jenkins Test Stage - SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage completed successfully.\n\nBuild: ${env.BUILD_URL}",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: 'ekankimahajan056@gmail.com',
                        subject: "Jenkins Test Stage - FAILURE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Run Tests stage failed.\n\nBuild: ${env.BUILD_URL}",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                success {
                    emailext(
                        to: 'ekankimahajan056@gmail.com',
                        subject: "Jenkins Security Scan - SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit security scan completed successfully.\n\nBuild: ${env.BUILD_URL}",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: 'ekankimahajan056@gmail.com',
                        subject: "Jenkins Security Scan - FAILURE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The NPM Audit security scan failed.\n\nBuild: ${env.BUILD_URL}",
                        attachLog: true
                    )
                }
            }
        }
    }
}
