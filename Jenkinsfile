pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
        
        stage('Unit and Integration Tests') {
            steps {
                script {
                    sh 'mvn test'
                }
                script {
                    sh 'run_integration_tests.sh'
                }
            }
        }
        
        stage('Code Analysis') {
            steps {
                script {
                    sh 'sonar-scanner'
                }
            }
        }
        
        stage('Security Scan') {
            steps {
                script {
                    sh 'owasp-zap-cli --scan'
                }
            }
        }
        
        stage('Deploy to Staging') {
            steps {
                script {
                    sh 'deploy_to_staging.sh'
                }
            }
        }
        
        stage('Integration Tests on Staging') {
            steps {
                script {
                    sh 'run_integration_tests_staging.sh'
                }
            }
        }
        
        stage('Deploy to Production') {
            steps {
                script {
                    sh 'deploy_to_production.sh'
                }
            }
        }
    }
    
    post {
        success {
            emailext body: "Pipeline executed successfully.",
                subject: "Pipeline Success",
                to: "your-email@example.com"
        }
        failure {
            emailext body: "Pipeline execution failed. Check logs for details.",
                subject: "Pipeline Failure",
                to: "your-email@example.com"
        }
    }
}
