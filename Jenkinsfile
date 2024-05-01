node {
	stage('Build') {
		echo 'Building..'
	}
	stage('Test') {
		echo 'Testing..'
	}
	try {
		stage('Deploy') {
			echo 'Deploying..'
		}
	} catch (Exception e) {
			echo 'Deployment failed.'
		}
}
