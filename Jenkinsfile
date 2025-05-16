pipeline {
    agent any
    
    environment {
        // Force Jenkins to use your Mac's Node.js installation
        PATH = "/usr/local/bin:${env.PATH}"
        // Alternative method if PATH doesn't work
        NODE = '/usr/local/bin/node'
        NPM = '/usr/local/bin/npm'
    }
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/ayushindapure/8.2CDevSecOps.git'
            }
        }
        
        stage('Verify Environment') {
            steps {
                sh '''
                    echo "PATH: $PATH"
                    which node || echo "Node not found in PATH"
                    which npm || echo "NPM not found in PATH"
                    ls -la /usr/local/bin/node || echo "Node not found at /usr/local/bin"
                    ${NODE} --version
                '''
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh '${NPM} install'
                sh '${NPM} list --depth=0'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh '${NPM} test || true'
            }
        }
        
        stage('Generate Coverage') {
            steps {
                sh '${NPM} run coverage || true'
                archiveArtifacts artifacts: 'coverage/**/*'
            }
        }
        
        stage('Security Scan') {
            steps {
                sh '${NPM} audit || true'
                sh '${NPM} audit --json > audit-report.json'
                archiveArtifacts artifacts: 'audit-report.json'
            }
        }
    }
    
    post {
        always {
            cleanWs()
            emailext (
                subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME}",
                body: "View results at: ${env.BUILD_URL}",
                to: 'your-email@example.com',
                attachLog: true
            )
        }
    }
}