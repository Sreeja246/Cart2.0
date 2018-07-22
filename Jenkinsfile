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
                sh 'mvn clean install'
            }
        }
        stage('Sonar') {
		tools{
			maven 'Maven'
		}
            steps {
                echo 'Sonar Scanner'
		withSonarQubeEnv('sonarqube') {
      		    sh 'mvn clean install sonar:sonar -Dsonar.host.http://35.196.19.59:9000'
		}
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying to nexus'
		sh 'mvn clean deploy -DaltDeploymentRepository=maven-snapshots::default::http://35.196.19.59:8081/repository/maven-snapshots/'
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

