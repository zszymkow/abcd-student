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
        stage('2: OSV Scan') {
            steps {
            	script {
            	    sh 'osv-scanner scan --lockfile package-lock.json --json > osv_scan_report.json || true'
		}
	   }

        }
        stage('3: Print the result') {
            steps {
            	script {
            	    sh 'cat osv_scan_report.json'
		}
	   }

        }
    }
}
