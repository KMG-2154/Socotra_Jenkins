
	
	pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "keymgtgrp"
	        UIPATH_ORCH_TENANT_NAME = "KeyManagementGroupUSDefault"
	        UIPATH_ORCH_FOLDER_NAME = "Shared"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Run Stages
	        stage('Build') {
	            steps {
	                echo "Run ${BRANCH_NAME} to UAT "
	                UiPathRunJob (
                        credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
                        
                        failWhenJobFails: true,
	                folderName: 'Shared',
					jobType:Unattended(),
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
                        parametersFilePath: '',
                        priority: 'Low',
                        processName: 'Socotra_Jenkins_Automation',
                        resultFilePath: 'TEST-com.qa.testcases.TestAddExposuresCommercialBuilding.xml',
                        strategy: Dynamically(jobsCount: 1), timeout: 3600, waitForJobCompletion: true, traceLevel:   'None'
	                    

	        )
	            }
	        }
stage('Integration') {
steps{
 
  junit 'TEST-com.qa.testcases.TestAddExposuresCommercialBuilding.xml'
}}
	

	
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	

	}
