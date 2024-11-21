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
        stage('2: Launch Juice-Shop') {
            steps {
            	script {
            	    echo 'Launching Juice-Shop...'
            	    sh 'docker run --name juice-shop -d --rm -p 3000:3000 bkimminich/juice-shop'
            	    sleep 5
            	}
            }
        }
        stage('3: OSV Scan') {
            steps {
            	script {
            	    //sh 'osv-scanner scan --lockfile package-lock.json > ${WORKSPACE}/scan_results/osv_scan_report.json || true'
            	    sh 'osv-scanner scan --lockfile package-lock.json --json --output ${WORKSPACE}/scan_results/osv_scan_report.json || true'
		}
	   }

        }
    }
}
