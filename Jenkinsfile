pipeline {
    agent any 
    environment {
          PATH = "/opt/maven/bin:$PATH"
    }

    stages {
	stage("build") {                      // 4  // Creates a stage named 'build'
            steps {                           // 5  // Defines the steps that will be executed in this stage
                echo "----------- build started ----------"  
                                              // Logs a message indicating the start of the build
                sh 'mvn clean deploy -Dmaven.test.skip=true'  
                                              // Runs Maven clean and deploy commands, skipping tests
                echo "----------- build completed ----------"  
                                              // Logs a message indicating the build completion
            }                                 // 5  // Ends the steps block for 'build' stage
        }                                     // 4  // Ends the 'build' stage

        stage("test") {                       // 6  // Creates a stage named 'test'
            steps {                           // 7  // Defines the steps that will be executed in this stage
                echo "----------- unit test started ----------"  
                                              // Logs a message indicating the start of unit tests
                sh 'mvn surefire-report:report'  
                                              // Runs the Maven Surefire report to execute unit tests
                echo "----------- unit test completed ----------"  
                                              // Logs a message indicating unit test completion
            }                                 // 7  // Ends the steps block for 'test' stage
        }
	stage('SonarQube Analysis') {
	    environment {
		scannerHome = tool 'saidemy-sonar-scanner'
	    }
	
	    steps {
		withSonarQubeEnv('saidemy-sonarqube-server'){
			sh "${scannerHome}/bin/sonar-scanner"
                }
	    }
	}
	stage("Quality Gate") {               // 11  // Creates a stage named 'Quality Gate'
            steps {                           // 12  // Defines the steps that will be executed in this stage
                script {                      // 13  // Allows running custom Groovy script inside the pipeline
                    timeout(time: 1, unit: 'HOURS') {  
                                              // Sets a timeout of 1 hour for the quality gate check
                        def qg = waitForQualityGate()  
                                              // Waits for the quality gate result from SonarQube
                        if (qg.status != 'OK') {  
                                              // Checks if the quality gate status is not OK
                            echo "Warning: Quality gate failed but continuing pipeline: ${qg.status}"  
                                              // Aborts the pipeline if the quality gate fails
                        }
                    }
                }                             // 13  // Ends the script block for the Quality Gate stage
            }                                 // 12  // Ends the steps block for 'Quality Gate' stage
        }                                     // 11  // Ends the 'Quality Gate' stage
    }
}
