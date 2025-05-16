pipeline {
    agent any
    
    // Force Jenkins to use your Mac's Node.js installation
    environment {
        PATH = "/usr/local/bin:${env.PATH}"
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ayushindapure/8.2CDevSecOps.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                // Verify installation
                sh 'npm list --depth=0'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test || true'  // Continue even if tests fail
            }
        }
        
        stage('Generate Coverage') {
            steps {
                sh 'npm run coverage || true'
                // Archive the coverage report
                archiveArtifacts artifacts: 'coverage/**/*'
            }
        }
        
        stage('Security Scan') {
            steps {
                sh 'npm audit || true'
                // Optional: Save audit report
                sh 'npm audit --json > audit-report.json'
                archiveArtifacts artifacts: 'audit-report.json'
            }
        }
    }
    
    post {
        always {
            // Clean up workspace
            cleanWs()
            // Send basic build notification
            emailext (
                subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME}",
                body: "View results at: ${env.BUILD_URL}",
                to: 'your-email@example.com',
                attachLog: true
            )
        }
        success {
            // Optional success notification
            slackSend channel: '#builds',
                color: 'good',
                message: "Build Successful: ${env.JOB_NAME} - ${env.BUILD_URL}"
        }
    }
}