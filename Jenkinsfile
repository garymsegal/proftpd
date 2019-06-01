node {
		stage ('Clean') {
			deleteDir ()
		}
		stage ('Checkout') {
			checkout scm
		}
		stage ('Configure') {
			sh './configure'
		}
		stage ('Coverity Build') {
			sh '/home/siguser/cov-analysis-2019.03/bin/cov-build --dir idir make'
			}
		stage ('Coverity analyze') {
			  sh '/home/siguser/cov-analysis-2019.03/bin/cov-import-scm --dir idir --scm git'
		      sh '/home/siguser/cov-analysis-2019.03/bin/cov-analyze --dir idir'
		}
		stage ('Coverity commit') {
				withCredentials([string(credentialsId: 'covpwd', variable: 'COVPWD')]) {
					sh '/home/siguser/cov-analysis-2019.03/bin/cov-commit-defects --dir idir --stream ProFTPd  --scm git --host localhost --port 8081 --user admin --password ${COVPWD}'
				}
		}
}
