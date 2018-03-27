pipeline {
	agent any
	
	stages {
		stage('Deploy') {
			steps {
                ansiblePlaybook(
                    playbook: 'playbook.yml',
                    inventory: 'staging/pocdev/inventory'
                )
			}
		}
	}

	post {
		success {
			updateGitlabCommitStatus(name: 'build', state: 'success')
		}
		failure {
			updateGitlabCommitStatus(name: 'build', state: 'failed')
		}
	    always {
	        step([$class: 'Mailer', recipients: 'delegation_dev@datacom.co.nz', notifyEveryUnstableBuild: true])
	    }
	}
}
