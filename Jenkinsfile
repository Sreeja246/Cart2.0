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
               steps {
      nexusArtifactUploader {
        nexusVersion('nexus3')
        protocol('http')
        nexusUrl('http://35.229.90.75:8081')
        groupId('com.repo')
        version('1.0')
        repository('CartArtifacts')
        credentialsId('44620c50-1589-4617-a677-7563985e46e1')
        artifact {
            artifactId('Cart2.0')
            type('jar')
            classifier('debug')
            file('cart2.0.jar')
        }
      }
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
