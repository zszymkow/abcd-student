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
                    def juiceShopLaunch = sh(
                    	script: 'docker run -d -p --rm 3000:3000 juice-shop',
                    	returnStatus: true
                    )
                    if (juiceShopLaunch == 0) {
                    	echo 'Juice-Shop is running.'
                    } else {
                    	error 'Failed to launch Juice-Shop container. Breaking pipeline.'
                    }
            	}
            }
        }
        stage('3: ZAP Passive Scan') {
            steps {
            	script {
            	    echo 'Launching ZAP Passive Scan...'
		    sh '''
		    	docker run --name zap \
				--add-host=host.docker.internal:host-gateway \
				-v /path/to/dir/with/passive/scan/yaml:/zap/wrk/:rw
				-t ghcr.io/zaproxy/zaproxy:stable
				bash -c "zap.sh -cmd -addonupdate; zap.sh -cmd -addoninstall communityScripts -addoninstall pscanrulesAlpha -addoninstall pscanrulesBeta -autorun /zap/wrk/passive_scan.yaml" || true
		    '''
		}
	   }
    	   post {
        	always {
            		sh '''
                		docker cp zap:/zap/wrk/reports/zap_html_report.html ${WORKSPACE}/scan_results/zap_html_report.html
                		docker cp zap:/zap/wrk/reports/zap_xml_report.xml ${WORKSPACE}/scan_results/zap_xml_report.xml
                		docker stop zap juice-shop
                		docker rm zap
            		'''
            	}
            }
    	   post {
        	always {
            		echo 'Saving results...'
            		archiveArtifacts artifacts: 'scan_results/**/*', fingerprint: true, allowEmptyArchive: true
            		echo 'Sending reports to DefectDojo...'
            		defectDojoPublisher(artifact: 'scan_results/zap_xml_report.xml, productName: 'Juice Shop', scanType: 'ZAP Scan', engagementName: 'ziemowit.szymkow@pentacomp.pl
            	}
            }
        }   
    }
}
