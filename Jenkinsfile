pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "sanshoosvwh"
	        UIPATH_ORCH_TENANT_NAME = "Naveen"
	        UIPATH_ORCH_FOLDER_NAME = "London"
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
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: 'None'
	        )
	            }
	        }
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'
	            }
	        }
	

	         // Deploy Stages
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
	                UiPathDeploy (
	                packagePath: "Output\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
				    environments: "",
	                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
					//credentials: ExternalApp(accountForApp: "CICDJenkins", applicationId: "fe5321cd-c7bc-4b6d-90ae-a0b1e6cc3652",applicationSecret: "*se_bPX6k7EOLgRS",applicationScope: "OR.Monitoring.Read OR.TestSetSchedules OR.TestSetExecutions.Write OR.TestSetSchedules.Read OR.Analytics.Read OR.Tasks OR.Tasks.Read OR.Tasks.Write OR.Analytics OR.TestSetExecutions.Read OR.Analytics.Write OR.Folders OR.Folders.Read OR.Folders.Write OR.BackgroundTasks OR.BackgroundTasks.Read OR.BackgroundTasks.Write OR.TestSets OR.TestSets.Read OR.TestSets.Write OR.TestSetExecutions OR.ML OR.ML.Write OR.ML.Read OR.Assets.Write OR.Queues OR.Queues.Read OR.Queues.Write OR.Jobs OR.Jobs.Read OR.Jobs.Write OR.Users OR.Assets.Read OR.Users.Read OR.Administration OR.Administration.Read OR.Administration.Write OR.Audit OR.Audit.Read OR.Audit.Write OR.Webhooks OR.Webhooks.Read OR.Users.Write OR.Assets OR.Execution.Write OR.License OR.License.Read OR.License.Write OR.Settings OR.Settings.Read OR.Settings.Write OR.Robots OR.Robots.Read OR.Robots.Write OR.Machines OR.Machines.Read OR.Machines.Write OR.Execution OR.Webhooks.Write OR.Monitoring OR.Execution.Read OR.Monitoring.Write OR.TestSetSchedules.Write OR.TestDataQueues OR.TestDataQueues.Read OR.TestDataQueues.Write OR.Hypervisor OR.Hypervisor.Read OR.Hypervisor.Write",identityUrl:"https://cloud.uipath.com/sanshoosvwh/Naveen/orchestrator_/identity/connect/token"),
					credentials: ExternalApp(accountForApp: "pipeline", applicationId: "301e939b-8396-4fd2-8b60-1c62e2eacb6c",applicationSecret: 'client_secret_id',applicationScope: "OR.Monitoring.Read OR.TestSetSchedules OR.TestSetExecutions.Write OR.TestSetSchedules.Read OR.Analytics.Read OR.Tasks OR.Tasks.Read OR.Tasks.Write OR.Analytics OR.TestSetExecutions.Read OR.Analytics.Write OR.Folders OR.Folders.Read OR.Folders.Write OR.BackgroundTasks OR.BackgroundTasks.Read OR.BackgroundTasks.Write OR.TestSets OR.TestSets.Read OR.TestSets.Write OR.TestSetExecutions OR.ML OR.ML.Write OR.ML.Read OR.Assets.Write OR.Queues OR.Queues.Read OR.Queues.Write OR.Jobs OR.Jobs.Read OR.Jobs.Write OR.Users OR.Assets.Read OR.Users.Read OR.Administration OR.Administration.Read OR.Administration.Write OR.Audit OR.Audit.Read OR.Audit.Write OR.Webhooks OR.Webhooks.Read OR.Users.Write OR.Assets OR.Execution.Write OR.License OR.License.Read OR.License.Write OR.Settings OR.Settings.Read OR.Settings.Write OR.Robots OR.Robots.Read OR.Robots.Write OR.Machines OR.Machines.Read OR.Machines.Write OR.Execution OR.Webhooks.Write OR.Monitoring OR.Execution.Read OR.Monitoring.Write OR.TestSetSchedules.Write OR.TestDataQueues OR.TestDataQueues.Read OR.TestDataQueues.Write OR.Hypervisor OR.Hypervisor.Read OR.Hypervisor.Write",identityUrl:"https://cloud.uipath.com/sanshoosvwh/Naveen/orchestrator_/identity/connect/token"),
					traceLevel: 'None',
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