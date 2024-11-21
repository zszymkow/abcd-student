pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('1: Prepare') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-pat', url: 'https://github.com/zszymkow/abcd-student', branch: 'main'
    
                    echo 'Making report directory'
                    sh 'mkdir -p scan_results/'
                }
            }
        }
        stage('2: Trufflehog Scan') {
            steps {
            	script {
            	    sh 'trufflehog git https://github.com/zszymkow/abcd-student --since-comit main --branch main --only-verified --fail'
		}
	   }

        }
    } 
}
