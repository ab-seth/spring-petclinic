pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('PetClinic-SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Run') {
            steps {
                sh 'java -Dserver.port=8081 -jar target/spring-petclinic-3.0.0-SNAPSHOT.jar &'
            }
        }
    }
}
