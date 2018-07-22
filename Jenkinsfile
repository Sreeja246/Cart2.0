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
      		    sh 'mvn clean install sonar:sonar -Dsonar.host.http://35.196.19.59:9000'
		}
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging'
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to nexus'
		sh 'mvn clean deploy -DaltDeploymentRepository=maven-snapshots::default::http://35.196.68.15:8081/repository/maven-snapshots/'
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

