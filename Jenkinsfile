pipeline {
    agent any 
    environment {
          PATH = "/opt/maven/bin:$PATH"
    }

    stages {
        stage("Build") {
	  steps {
	      sh "mvn clean deploy"
	  }
        }

	stage('SonarQube Analysis') {
	    environment {
		scannerHome = tool 'saidemy-sonar-scanner'
	    }
	
	    steps {
		withSonarQubeEnv('saidemy-sonar-server'){
			sh "${scannerHome}/bin/sonar-scanner"
                }
	    }
	}
    }
}
