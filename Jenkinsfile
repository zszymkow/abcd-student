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
            	    sh 'trufflehog git https://github.com/zszymkow/abcd-student --branch main --only-verified --json > scan_results/trufflehog_scan_result.json'
		}
	   }

        }
    }
    post {
       	always {
    		echo 'Saving results...'
       		archiveArtifacts artifacts: 'scan_results/**/*', fingerprint: true, allowEmptyArchive: true
       		echo 'Sending reports to DefectDojo...'
       		defectDojoPublisher(artifact: 'scan_results/trufflehog_scan_result.json', productName: 'Juice Shop', scanType: 'Trufflehog Scan', engagementName: 'ziemowit.szymkow@pentacomp.pl')
       	}
 
    }    
}
