pipeline {
    agent any
    
    environment {
        SNYK_TOKEN = credentials('snyk-api-token')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Nick-P-Adams/usdc-hackathon', branch: 'main'
            }
        }
        
        stage('Install npm packages') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm run test'
            }
        }
        
        stage('Snyk Scan') {
            steps {
                echo 'Snyk Scanning'
		snykSecurity(
			snykInstallation: 'snyk_install',
			snykTokenId: 'fef9c154-1d2c-429a-8071-e8304ba75c6a'
		)
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploy!'
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Pipeline completed successfully!'
            mail to: 'nick.patrick.adams@gmail.com',
                 subject: "SUCCESS: Build ${env.BUILD_NUMBER}",
                 body: "Good news! The build ${env.BUILD_NUMBER} was successful."
        }
        failure {
            echo 'Pipeline failed!'
            mail to: 'nick.patrick.adams@gmail.com',
                 subject: "FAILURE: Build ${env.BUILD_NUMBER}",
                 body: "Unfortunately, the build ${env.BUILD_NUMBER} failed. Please check the logs for details."
        }
    }
}
