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
            }
        }        
        stage('Build') {
            steps {
                echo 'Building..'
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn clean package sonar:sonar -Dsonar.language=mule4 -Dsonar.sources=src/main/mule'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
