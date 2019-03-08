pipeline {
	agent any

	environment {
	PROJECT_NAME = "myprojecgt"
	REPO_NAME = "devops_example"
	// DEV_ENV = ""
	// QA_ENV = ""
	// STAGING_ENV = ""
	// PROD_ENV = ""
	// ZONE = ""

	// ARTIFACTORY_REGISTRY_NAME= "" jfrog adress
	// PACKAGE_VERSION = sh (
	//   script: "cat package.json | grep version | head -1 | awk -F: '{ print \$2 }' | sed 's/[\",]//g'| tr -d '[[:space:]]'",
	//   returnStdout: true
	// )
	BUILD_TAG = "${env.BUILD_ID}-${env.GIT_COMMIT.take(7)}"
	VERSION_TAG = "${env.GIT_COMMIT.take(7)}"
  } //enviornment


	stage('Build') {
	  when { 
		anyOf { 
		  branch 'develop';  branch 'staging' ;  branch 'master'  ;  branch 'qa'
		}
	  }
	  steps {
		echo 'Building docker image...'
		sh "docker build -t ${PROJECT_NAME}/${REPO_NAME}:latest ."
	  }
	}//build


	stage('Test') {
	  when { 
		anyOf { 
			branch 'develop';  branch 'staging' ;  branch 'master' ;  branch 'qa'
		}
	  }
	  steps {
		echo 'Testing....'
		//sh "docker run ${PROJECT_NAME}/${REPO_NAME}:latest yarn test"
	  }
	} //Test


	stage('Tag') {
	  when { 
		anyOf { 
			branch 'develop';  branch 'staging'  ; branch 'master' ;  branch 'qa'
		}
	  }
	  steps {
		echo 'Tagging....'
		// //Tagging for Artifactory
	  }
	} // Tag

	stage('Publish') {
		when { 
			anyOf { 
				branch 'develop';  branch 'staging'  ;  branch 'master' ;  branch 'qa'
			}
		}
		steps {
		echo 'Publishing....'
		// Publish to Artifactory

		//   withCredentials([[$class: '', credentialsId: '',
		//             usernameVariable: '', passwordVariable: '']]) {
		//   }
		}

	stage('Deploy-Dev') {
		when {
		anyOf {
			branch 'develop';
		}
		}
		steps {
			echo 'Deploying to DEV Environment....'

		// WIP
		// // pull master branch of environment-build repo
			git branch: 'master',
				// credentialsId: '', use your own credentialsId
				url: 'git@github.com:riskgod/devops_example.git'

		// // authenticate to gcp project and kubernetes cluster
			withCredentials([file(credentialsId: "$DEV_ENV-jenkins-keys", variable: 'SVC_KEY')]) {
				//need to replace for ansible

			} //withCredentials
		}
	} // stage(Deploy-Dev)

	post {
		always {
			echo 'success'
			// junit 'build/reports/results/build_report.xml'
		}

		success {
			echo 'need to send messages to slack'
			// slackSend channel: '#aspen-backend',
			//         color: 'good',
			//         message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
		}
		failure {
			 echo 'need to send email'
			// mail to: 'riskgod@sina.com',
			//     subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
			//     body: "Something is wrong with ${env.BUILD_URL}"
		}
		unstable {
			echo 'This will run only if the run was marked as unstable'
		}
		changed {
			echo 'This will run only if the state of the Pipeline has changed'
			echo 'For example, if the Pipeline was previously failing but is now successful'
		}
	}
}


