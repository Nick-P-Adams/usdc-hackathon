pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Nick-P-Adams/usdc-hackathon', branch: 'main'
            }
        }
        
        stage('Install npm packages') {
            steps {
                sh 'npm install'
		sh 'npm install @sentry/nextjs@8.10.0'
            }
        }
        
		parallel {
			stage('Unit Tests') {
            			steps {
                			sh 'npm run test'
            			}
        		}
        
        		stage('Snyk Scan') {
				steps {
					echo 'Snyk Scanning'
					snykSecurity(
						snykInstallation: 'snyk_install',
						snykTokenId: 'snyk_api_token'
					)
				}
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
