pipeline {
    agent any
	tools{
		maven 'Maven'
		
	}
    stages {
        stage('Checkout') {
            steps {
                echo 'Checkout'
		git url: 'git@github.com:Sreeja246/Cart2.0.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Clean Build'
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh 'mvn test'
            }
        }
        stage('JaCoCo') {
            steps {
                echo 'Code Coverage'
                jacoco()
            }
        }
        stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
		withSonarQubeEnv('sonarqube') {
      		sh 'mvn clean install -D sonar.host.http://35.237.97.186:9000'
		}
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging'
                sh 'mvn package'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to nexus'
		echo '@@@@@@@@@@@@@@@@'
		    nexusArtifactUploader artifacts: [
   [artifactId: 'nexus-artifact-uploader', classifier: 'debug', file: 'nexus-artifact-uploader.jar', type: 'jar'], 
   [artifactId: 'nexus-artifact-uploader', classifier: 'debug', file: 'nexus-artifact-uploader.hpi', type: 'hpi']
],  
nexusUrl: 'http://35.229.90.75:8081/nexus', 
nexusVersion: 'nexus2', 
protocol: 'http', 
repository: 'NexusArtifactUploader', 
version: '2.4'
            }
        }
    }
    
    post {
        always {
            echo 'JENKINS PIPELINE'
        }
        success {
            echo 'JENKINS PIPELINE SUCCESSFUL'
        }
        failure {
            echo 'JENKINS PIPELINE FAILED'
        }
        unstable {
            echo 'JENKINS PIPELINE WAS MARKED AS UNSTABLE'
        }
        changed {
            echo 'JENKINS PIPELINE STATUS HAS CHANGED SINCE LAST EXECUTION'
        }
    }
}
