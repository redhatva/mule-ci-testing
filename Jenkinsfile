pipeline {
    agent any
    
    tools { 
        maven 'maven3.5.4' 
        jdk 'jdk8' 
    }
    
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 

		echo 'Pulling...' + env.BRANCH_NAME

		sh 'env'
		sh 'printenv'
            }
        }        

        stage('Build and Test') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean package sonar:sonar -Dsonar.language=mule4 -Dsonar.sources=src/main/mule'
                }
            }
        }

  	stage("Quality Gate"){
      		steps {
          		timeout(time: 600, unit: 'SECONDS') {
              			script{
                  			def qg = waitForQualityGate()
                  			if (qg.status != 'OK') {
                      				error "Problem with SonarQube Quality Gate - status = ${qg.status}"
                  			}
              			}
          		} 
      		}
  	}

        stage('Deploy to Development') {
	    when {
                branch 'develop'
            }
            steps {
                echo 'Deploying to deevlopment...'
            }
        }

	stage('Deploy to QA') {
	    when {
		branch 'release'
	    }
	    steps {
		echo 'Deploying to QA...'
	    }
	}

	stage('Deploy to PROD') {
	    when {
		branch 'master'
	    }
	    steps {
		echo 'Deploying to PROD...'
	    }
	}

    }
}
