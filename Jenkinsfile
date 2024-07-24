pipeline {
    agent any
    
    environment {
        SNYK_TOKEN = credentials('snyk_api_token')
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
			snykTokenId: '$SNYK_TOKEN'
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
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
