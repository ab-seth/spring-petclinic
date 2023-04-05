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
    }
}
