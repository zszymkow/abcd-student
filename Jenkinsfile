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
        stage('2: Semgrep Scan') {
            steps {
            	script {
            	    sh 'semgrep scan --config auto --json > scan_results/semgrep_scan_result.json'
		}
	   }

        }
    }   
}
