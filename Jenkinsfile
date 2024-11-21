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
            	    sh 'osv-scanner scan --lockfile package-lock.json --json > scan_results/osv_scan_report.json || true'
		}
	   }

        }
        stage('3: Print the result') {
            steps {
            	script {
            	    sh 'cat scan_results/osv_scan_report.json'
		}
	   }

        }
    }
    post {
       	always {
    		echo 'Saving results...'
       		archiveArtifacts artifacts: 'scan_results/**/*', fingerprint: true, allowEmptyArchive: true
       		echo 'Sending reports to DefectDojo...'
       		defectDojoPublisher(artifact: 'scan_results/osv_scan_report.json', productName: 'Juice Shop', scanType: 'OSV Scan', engagementName: 'ziemowit.szymkow@pentacomp.pl')
       	}
 
    }    
}
