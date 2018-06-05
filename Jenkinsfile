if( "$env.BRANCH_NAME" != "master") {

	try {
		timeout(time: 300, unit: 'SECONDS') { // change to a convenient timeout for you
			input "Would you like to build the branch $env.BRANCH_NAME?"
		}
	} catch(err) { // timeout reached or input false
		def user = err.getCauses()[0].getUser()
		if('SYSTEM' == user.toString()) { // SYSTEM means timeout.
			echo "Building of branch $env.BRANCH_NAME Aborted due to Timeout"
		}else {
			// Do not build the branch
			echo "Building of branch $env.BRANCH_NAME Aborted by: [${user}]."
		}
		currentBuild.result = 'SUCCESS'
		return
	}

}
			
node(){
	stage('Cleaning Workspace') {
		deleteDir()
	}
	stage('Checkout'){
		checkout scm
	}
	stage('Generating DSL Jobs'){
		jobDsl removedConfigFilesAction: 'DELETE', removedJobAction: 'DELETE', removedViewAction: 'DELETE', sandbox: true, targets: '**/*.dsl', unstableOnDeprecation: true

	}	
}

