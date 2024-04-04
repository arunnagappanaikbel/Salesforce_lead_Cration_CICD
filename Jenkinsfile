pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/enquewuxwbew/MyTenant/orchestrator_/"
	        UIPATH_ORCH_LOGICAL_NAME = "enquewuxwbew"
	        UIPATH_ORCH_TENANT_NAME = "MyTenant"
	        UIPATH_ORCH_FOLDER_NAME = "Test"
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
	        
			stage('Build') {
      steps {
        echo "Building..with ${WORKSPACE}"
        UiPathPack(
          //outputPath: "Output\\${env.BUILD_NUMBER}",
	  outputPath: "C:\\Users\\arnaik2\\OneDrive - Cisco\\Desktop\\UiPath\\Salesforce_UseCase_POC\\${env.BUILD_NUMBER}",
          projectJsonPath: "C:\\Users\\arnaik2\\OneDrive - Cisco\\Desktop\\UiPath\\Salesforce_UseCase_POC\\project.json",
          version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
          traceLevel: 'None'
        )
      }
    }

stage('Deploy to UAT') {
      steps {
        echo "Deploying ${BRANCH_NAME} to UAT "
        UiPathDeploy(
          //packagePath: "Output\\${env.BUILD_NUMBER}",
	  packagePath: "C:\\Users\\arnaik2\\OneDrive - Cisco\\Desktop\\UiPath\\Salesforce_UseCase_POC\\${env.BUILD_NUMBER}",
          orchestratorAddress: "${UIPATH_ORCH_URL}",
          orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
          folderName: "${UIPATH_ORCH_FOLDER_NAME}",
          //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
          credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
          traceLevel: 'None',
	  environments: '',
	  createProcess: true,
          entryPointPaths: 'Main.xaml'

        )
      }
    }


	         // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
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
