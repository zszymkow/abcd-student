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
    post {
       	always {
    		echo 'Saving results...'
       		archiveArtifacts artifacts: 'scan_results/**/*', fingerprint: true, allowEmptyArchive: true
       		echo 'Sending reports to DefectDojo...'
       		defectDojoPublisher(artifact: 'scan_results/semgrep_scan_result.json', productName: 'Juice Shop', scanType: 'Semgrep JSON Report', engagementName: 'ziemowit.szymkow@pentacomp.pl')
       	}
    }   
}
