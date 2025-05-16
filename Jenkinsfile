
pipeline {
    agent any
     environment {
        // Force Jenkins to use your Mac's Node.js installation
        PATH = "/usr/local/bin:${env.PATH}"
    }
    tools {
        nodejs 'System_Node'  // Matches the name configured in Jenkins Global Tools
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/ayushindapure/8.2CDevSecOps.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test || true'  // Continue even if tests fail
            }
        }
        
        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'  // Continue even if coverage fails
            }
        }
        
        stage('Security Scan') {
            steps {
                sh 'npm audit || true'  // Continue even if vulnerabilities found
            }
        }
    }
    
    post {
        always {    
            archiveArtifacts artifacts: 'coverage/**/*'  // Save coverage reports
            cleanWs()  // Clean workspace after build
        }
    }
}